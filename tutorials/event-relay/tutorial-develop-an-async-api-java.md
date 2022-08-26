---
title: Developing an Asynchronous API in Java
author: Nathaniel Sandberg
last updated: 6/24/2022
---

$[eventRelayIntroductionBoilerPlate]

## What You Will Learn in This Tutorial

$[eventRelayBoilerPlateWhatYouWillLearn]

## Things to Remember in This Tutorial

You should remember the following:

* Use the following when you are installing dependencies:  

    ```bash
    mvn install
    ```

* Use the following to create the package:  

    ```bash
    mvn package
    ````

* Use the following to start the server:  

    ```bash
    mvn jetty:run
    ```

You must also install Maven to complete this tutorial.
You can download Maven from the following link: [Downloading Apache Maven 3.8.5](https://maven.apache.org/download.cgi).
Unzip the folder in the directory of your choosing.

$[settingTheJavaPath]

## Creating the Project

1. Create the project in Maven.  
    On the command line in the Maven directory, type the following:  

    ```bash
    mvn archetype:generate -DgroupId=com.event.endpoint -DartifactId=asynchronousAPI -Dpackagename=com.asynchronousAPI -DarchetypeArtifactId=maven-archetype-webapp
    ```
  
1. Type the following to finish creating the prompt:  
    * At the following prompt, press enter:  

    ```bash
    1.0-SNAPSHOT
    ```

    * Type the following at the prompt to finalize the project:  

    ```bash
    Y
    ```

1. Add the dependencies to the project.  
    In the `pom.xml` file, update the dependencies to the following:

    ```xml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>1.2.6</version>
    </dependency>

    <!-- for web servlet -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>

    <!-- Some containers like Tomcat don't have jstl library -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>jstl</artifactId>
        <version>1.2</version>
        <scope>provided</scope>
    </dependency>
    ```

1. Add the plugins to the build.  
    Update the build in the `pom.xml` with the following plugins:  

    ```xml
    <build>
        ...
        <plugins>
            <!-- http://www.eclipse.org/jetty/documentation/current/jetty-maven-plugin.html -->
            <plugin>
                <groupId>org.eclipse.jetty</groupId>
                <artifactId>jetty-maven-plugin</artifactId>
                <version>9.4.12.v20180830</version>
            </plugin>

            <!-- Default is too old, update to latest to run the latest Spring 5 + jUnit 5 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>2.22.0</version>
            </plugin>

            <!-- Default 2.2 is too old, update to latest -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.2.2</version>
            </plugin>

        </plugins>
    </build>
    ```

1. Add the properties to get Maven to work.  
    In the `pom.xml` file, type the following:  

    ```xml
    <project>
      ...
      <properties>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
      </properties>
      ...
    </project>
    ```

1. Change directories to your project.  
    On the command line, type the following:  

    ```bash
    cd asynchronousAPI
    ```

1. Run the server.  
    Type the following to start the server:  

    ```bash
    mvn jetty:run
    ```

You can now go to `localhost:8080` and see `Hello World!` in the browser.

You may need to do one of the following to stop the server:  

* On the command line, type **Ctrl**+**C** to stop the server.
* Use another command line to find the server and stop it.  
    1. On the command line, type the following to find the running app:  

        ```bash
        netstat -aon | grep 8080
        ```

    1. On the command line, type the following to stop the app:  
        **Note:** Use the PID on the right on the command line with this command.  

        ```bash
        tskill {PID}
        ```

* Stop the server from the **Task Manager** or **Activity Monitor**:  
    * On Windows, do the following:  
        1. Find Java running in the **Task Manager** on Windows.  
        1. Click the Java task.  
        1. Click **End task**.  
    * On Mac, do the following:  
        1. Find the Java running in the **Activity Monitor** on Mac.
        1. Click the Java activity.  
        1. End the Java activity.

## Creating the Payload to Send to the Webhook

1. Create the `payload.json` file.  
    On the command line, type the following:  

    ```curl
    touch src/main/resources/payload.json
    ```

1. Create the contents of the `payload.json` file.  

    ```json
    {
        "id": "6620800b-7029-4a7a-8e80-cdd852fc01c8",
        "number": 16,
        "haveYouSaidHello": true,
        "waysToSayHello": [
            "Hello",
            "Hola",
            "Hallo",
            "Bonjour",
            "Ola"
        ],
        "helloID": {
            "language": "English",
            "id": "b2f7494a-5dcf-448c-9585-5301fc0dd231"
        }
    }
    ```

1. Create the Java directory.  
    On the command line, type the following:  

    ```bash
    mkdir src/main/java
    ```

1. Create the `AsynchronousAPI.java` file.  
    On the command line, type the following:  

    ```bash
    touch src/main/java/AsynchronousAPI.java
    ```

    > You may see some errors in your editor in the next step
    > because we have not imported all the dependencies.

1. Create the class to watch the file for changes.  
    In the `AsynchronousAPI.java` file, type the following to create the class:  

    ```java
    import javax.servlet.http.HttpServlet;

    import java.io.File;

    import java.io.FileNotFoundException;
    import java.util.Scanner;

    public class AsynchronousAPI extends HttpServlet{
        public static long LAST_TIME = 0L;

        public static String data ="";

        public AsynchronousAPI() throws UnsupportedEncodingException {
            ClassLoader classLoader = getClass().getClassLoader();
            File  wholeFile =new File (classLoader.getResource("payload.json").getFile());

            String fileName = wholeFile.toString();

            while(true){
                long timestamp = readLastModified(fileName);
                if (timestamp != LAST_TIME) {
                    System.out.println("file updated:" + timestamp);

                    //Read file:
                    try{
                        Scanner readFile = new Scanner(wholeFile);
                        while(readFile.hasNextLine()){
                            data = data + readFile.nextLine();
                        }
                        readFile.close();

                        System.out.println(data);

                        HttpClient httpclient = HttpClients.createDefault();
                        HttpPost httpPost = new HttpPost("{yourWebhookSiteURL}");

                        httpPost.setEntity(new StringEntity(data, "UTF-8"));
                        try{
                            HttpResponse response = httpclient.execute(httpPost);
                            HttpEntity entity = response.getEntity();
                            if (entity != null) {
                                InputStream instream = entity.getContent();
                                System.out.println(instream);
                            }
                        }catch(IOException e){
                            e.printStackTrace();
                        }

                    }catch(FileNotFoundException e){
                        System.out.println("An error occured");
                        e.printStackTrace();
                    }

                    LAST_TIME = timestamp;
                    data = "";
                }
            }
        }

        public static long readLastModified(String fileName) {
            File file = new File(fileName);
            return file.lastModified();
        }
    }
    ```

    **Note:** Replace `{yourWebhookSiteURL}` with the webhook you get from <https://webhook.site>.
    Remove the `s` on `https` in the protocol.

1. Create the `asynchronousAPI` servlet in the project file.  
    In the `src/main/webapp/WEB-INF/web.xml` file, type the following in the `<web-app>` tag to create the servlet:  

    ```xml
    <web-app>
      ...
      <servlet>
        <servlet-name>
          AsynchronousAPI
        </servlet-name>  
        <servlet-class>
          AsynchronousAPI
        </servlet-class>
        <load-on-startup>1</load-on-startup>
      </servlet>
    </web-app>
    ```

    **Note:** Use the `load-on-startup` tag to load the servlet file and monitor the `payload.json` file.

## Sending the File Contents

Now, we send the payload with Java to create the event.

1. Add the dependency in the `pom.xml` file for http requests.  
    In the `pom.xml` file, type the following:  

    ```xml
    <dependency>
        <groupId>org.apache.httpcomponents</groupId>
        <artifactId>httpclient</artifactId>
        <version>4.4</version>
    </dependency>
    <dependency>
      <groupId>org.json</groupId>
      <artifactId>json</artifactId>
      <version>20180130</version>
    </dependency>
    ```

1. Add the imports to create the post request.  
    At the top of the `AsynchronousAPI.java` file, type the following:  

    ```java
    import org.apache.http.HttpEntity;
    import org.apache.http.client.HttpClient;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.HttpClients;
    import org.apache.http.HttpResponse;
    import java.io.IOException;
    import java.io.InputStream;
    import java.io.UnsupportedEncodingException;
    ```

1. Start the Maven server.  
    On the command line, type the following:  

    ```bash
    mvn jetty:run
    ```

1. Update the payload file with valid JSON data.  
    Replace the existing JSON object with the following:

    ```json
    {
        "id": "1541b98b-ec92-4428-ad96-9300ac0be2b6",
        "number": 16,
        "haveYouSaidHello": true,
        "waysToSayHello": [
            "Hello",
            "Hola",
            "Hallo",
            "Bonjour",
            "Ola"
        ],
        "helloID": {
            "language": "French",
            "id": "d8521586-066f-4326-9468-5dcba89a324b"
        }
    }
    ```

You can now update the `payload.json`,
and the Java server will send the updates to the file to the webhook endpoint.

> Change the `payload.json` file and save it to send the updated information.

View the updates at your <https://webhook.site>.

**Note:** The webhook site may change your URL sometimes.
Double-check the URL at <https://webhook.site>
before you run the Java server after if you haven't used it recently and replace the UUID in the URL of your `index.js` if needed with the new UUID
that the <https://webhook.site> assigned to you.

## Next Steps

This tutorial should have shown you how to do the following:

* Create an asynchronous API producer that runs locally.
* Create a simple model to update your Subscribers with new information.
* Have the asynchronous API producer send the payload to an endpoint.
* Use a webhook service to test your asynchronous API producer.

Go to the next tutorial to find out how to publish your asynchronous API producers to Fortellis.

You can find the complete project on GitHub: [java-event-tutorial](https://github.com/Fortellis/java-event-tutorial).

To use the project from GitHub, do the following:

1. Clone the repository.  
    On the command line in the folder of your choice, type the following:  

    ```curl
    git clone git@github.com:Fortellis/java-event-tutorial.git
    ```

1. Change directories to get to the new folder.  
    On the command line, type the following:  

    ```curl
    cd java-event-tutorial
    ```

1. Update the URL to your webhook.  
    In the `AsynchronousAPI.java`, replace `{yourWebhookURL}` with the URL you get from <https://webhook.site> to receive requests.  

1. Start the server.  
    On the command line, type the following:  

    ```curl
    mvn jetty:run
    ```

1. Send the event.  
    Change the contents of the `payload.json`.  

You can now see the payload at your webhook site.  
