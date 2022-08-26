---
title: Integrating Your App and an Asynchronous API Producer in Java
author: Nathaniel Sandberg
last updated: 7/27/2022
---

You can receive information from an event with webhooks.
Create a webhook to receive the events and then process those on your backend or display the information in a UI.
With webhooks, you can receive events and keep the information in your app from becoming stale.

## Tutorial

### What You Will Learn

In this tutorial, you should learn how to do the following:

* Set a `health` endpoint for your app.
* Receive a call from an asynchronous API producer.
* Store the call from the asynchronous API producer for processing later.

### Things to Remember in This Tutorial

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

### Creating the Java Project

1. Open a command line tool in the Maven directory.  
1. Make the Maven project.  
    On the command line, type the following:  

    ```bash
    mvn archetype:generate -DgroupId=com.webhook.endpoint -DartifactId=webhookEndpoint -Dpackagename=com.webhook.endpoint -DarchetypeArtifactId=maven-archetype-webapp
    ```

1. Change directories to your project.  
    On the command line, type the following:  

    ```bash
    cd webhookEndpoint
    ```

1. Create the Java folder.  
    On the command line in that folder, type the following:  

    ```bash
    mkdir src/main/java
    ```

1. Add the following for the plugins.  
    In the `pom.xml` file in the `build` tag, type the following:  

    ```xml
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
    ...
    ```

    **Note:** You must also configure Maven to use Java 11 or Java 8.

1. Configure Maven to use the correct version of Java.  
    In the `pom.xml` file, type the following to add the properties:  

    ```xml
    <project>
      [...]
      <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
      </properties>
      [...]
    </project>
    ```

You can now do the following:  

1. Start the server.  
    On the command line, type the following:  

    ```bash
    mvn jetty:run
    ```

1. View the website in your browser.  
    Go to the following website to see the server running.  

    ```http
    localhost:8080
    ```

Do one of the following to stop the server:  

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

We also need to have the two servers running on different ports to get them to work at the same time.
We explicitly state `port` 9999 later in the tutorial.
You must use `port` 9999 to find the program after completing that step.

### Creating the Health Check Endpoint

1. Create the endpoint.  
    In the `src/main/webapp/WEB-INF/web.xml` file, type the following:  

    ```xml
    ...
    <servlet>
        <servlet-name>
        HealthCheck
        </servlet-name>  
        <servlet-class>
        HealthCheck  
        </servlet-class>
    </servlet>
    <servlet-mapping>
        <url-pattern>/health</url-pattern>
        <servlet-name>HealthCheck</servlet-name>
    </servlet-mapping>
    ...
    ```

1. Add the dependencies in the `pom.xml` file.  
    In the `pom.xml` file, type the following in the dependencies:  

    ```xml
    ...
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
    <dependency>
        <groupId>org.apache.httpcomponents</groupId>
        <artifactId>httpclient</artifactId>
        <version>4.4</version>
    </dependency>
    ...
    ```

1. Create the `HealthCheck.java` file.  
    On the command line, type the following:  

    ```bash
    touch src/main/java/HealthCheck.java
    ```

1. Create the content in the `HealthCheck.java` file.  
    In the file, type the following:  

    ```java
    import java.io.IOException;
    import java.io.PrintWriter;

    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;

    public class HealthCheck extends HttpServlet{

        public HealthCheck(){
            super();
        }
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException,IOException{
            PrintWriter healthCheckResponse = response.getWriter();
            response.setContentType("application/json");
            response.setCharacterEncoding("UTF-8");
            response.setStatus(HttpServletResponse.SC_OK);
            healthCheckResponse.print("{\"status\": \"Up\"}");
            healthCheckResponse.flush();
        }
    }
    ```

1. Change the port that the server serves the site on.  
    In the `pom.xml` file, type the following:  

    ```xml
    ....
    <plugins>
    <!-- http://www.eclipse.org/jetty/documentation/current/jetty-maven-plugin.html -->
    <plugin>
        <groupId>org.eclipse.jetty</groupId>
        <artifactId>jetty-maven-plugin</artifactId>
        <version>9.4.12.v20180830</version>
        <configuration>
        <httpConnector>
            <!--host>localhost</host-->
            <port>9999</port>
        </httpConnector>
        </configuration>
    </plugin>
    ....
    ```

You can now go to `localhost:9999/health` in your browser to see the status of the server.
Press **Ctrl**+**C** to stop the server.

### Creating the Endpoint to Receive the Event

1. Create the `HelloWorld.java` file.  
    On the command line, type the following:  

    ```bash
    touch src/main/java/HelloWorld.java
    ```

1. Enter the code in the `HelloWorld.java` file.  
    In the file, type the following:  

    ```java
    import java.io.BufferedReader;
    import java.io.IOException;
    import java.io.PrintWriter;

    import javax.servlet.ServletException;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;

    public class HelloWorld extends HttpServlet{

        public HelloWorld(){
            super();
        }

        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException,IOException{
            PrintWriter healthCheckResponse = response.getWriter();
            StringBuilder buffer = new StringBuilder();
            BufferedReader reader = request.getReader();
            String line;
            while ((line = reader.readLine()) != null) {
                buffer.append(line);
                buffer.append(System.lineSeparator());
            }
            String data = buffer.toString();
            System.out.println("This is your request: "+ data);
            response.setContentType("application/json");
            response.setCharacterEncoding("UTF-8");
            response.setStatus(HttpServletResponse.SC_ACCEPTED);
            healthCheckResponse.print("");
            healthCheckResponse.flush();
        }
    }
    ```

1. Add the endpoint in the `web.xml` file.  
    In the file, add the `/helloworld/event` endpoint with the following:  

    ```xml
    ...
    <servlet>
      <servlet-name>
      HelloWorld
      </servlet-name>  
      <servlet-class>
      HelloWorld  
      </servlet-class>
    </servlet>
    <servlet-mapping>
        <url-pattern>/helloworld/event</url-pattern>
        <servlet-name>HelloWorld</servlet-name>
    </servlet-mapping>
    ...
    ```

    > Fortellis appends the events endpoint to the URL that you enter when you are registering your webhook endpoint.

Now when you send a request to the local host at `http://localhost:9999/helloworld/event`,
you get a response with a  `202 Accepted` code and an empty response.
The `Accepted` response tells Fortellis
that you received the message,
even if you don't process it immediately.

```bash
curl --verbose --location --request POST 'http://localhost:9999/helloworld/event' \
--header 'Type: application/json' \
--header 'Content-Type: application/json' \
--data-raw '{
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
}'
```

### Saving the Events

1. Create the method to save the payload to a new file.  
    Add the following method below the `doPost` method:  

    ```java
    ...
    public void newAsynchronousAPIPost(String newEvent) {

        try{
            ClassLoader classLoader = getClass().getClassLoader();
            File  wholeFile =new File (classLoader.getResource("queue.json").getFile());
            InputStream inputStream = new FileInputStream(wholeFile);
            FileInputStream fis = new FileInputStream(wholeFile);
            DataInputStream in = new DataInputStream(fis);
            BufferedReader br = new BufferedReader(new InputStreamReader(in));
            String concatenatedFile = "";
            String strLine;
            //Put all the lines from the file back together to make the original object.
            while((strLine = br.readLine()) != null){
                //System.out.println(strLine);
                concatenatedFile = concatenatedFile + strLine;
            }
            System.out.println("This is the concatenatedFile: " + concatenatedFile);
            //Make the concatenated lines a JSON object again.
            JSONObject objectForConcatenatedFile = new JSONObject(concatenatedFile);
            //Change the connectionRequests from the file into just an array.
            JSONArray parsedRequestsInQueue = objectForConcatenatedFile.getJSONArray("queue");
            System.out.println("This is just the array of the connectionRequests: "+ parsedRequestsInQueue);
            //Change the connectionRequest into an object
            JSONObject addObject = new JSONObject(newEvent);
            //Put the connection request object into the array.
            parsedRequestsInQueue.put(addObject);
            System.out.println("This is the object for the concatenated file: "+ objectForConcatenatedFile.toString(4));

            Path path = Paths.get(wholeFile.toString());
            String str = objectForConcatenatedFile.toString(4);
            byte[] arr = str.getBytes();
            try{
                Files.write( path, arr);
            }catch(IOException ex){
                System.out.print("Invalid Path");
            }
        }catch(Exception ex){
            ex.printStackTrace();
        }
    }
    ...
    ```

1. Add the imports.
    At the top of the `HelloWorld.java` file, add the following imports.

    ```java
    ...
    import java.io.File;
    import java.io.InputStream;
    import java.io.FileInputStream;
    import java.io.DataInputStream;
    import java.io.InputStreamReader;
    import java.nio.file.Path;
    import java.nio.file.Paths;
    import java.nio.file.Files;
    import org.json.*;
    ...
    ```

1. Add the `json` dependency to the `pom.xml` file.  
    In the `pom.xml` file, type the following with the dependencies:  

    ```xml
    ...
    <dependency>
        <groupId>org.json</groupId>
        <artifactId>json</artifactId>
        <version>20180130</version>
    </dependency>
    ...
    ```

1. Create the file to store the request.  
    On the command line, type the following:  

    ```bash
    touch src/main/resources/queue.json
    ```

1. Add the empty object to the file.  
    In the ``queue.json` file, type the following:  

    ```json
    {"queue":[]}
    ```

1. Add the method to the end of the `doPost` method.  
    In the `doPost` method, type the following:  

    ```java
    ...
    newAsynchronousAPIPost(data);
    ...
    ```

Now, you save the events that you receive at `http://localhost:9999/helloworld/event`.  

If you send the request now,
Java saves the updated information to the `target/classes/queue.json` file.  

**Note:** Your `src/main/resources/queue.json` file keeps the original content.  

### Integrating Your App with Your Asynchronous API Producer

If you followed the previous tutorial on [Developing an Asynchronous API in Java](/docs/tutorials/event-relay/tutorial-develop-an-async-api-java),
you can now do the following:

1. Change the URL in your asynchronous API producer from `https://webhook.site/{yourUUID}` to your local environment endpoint `http://localhost:9999/helloworld/event`.
1. Start both the asynchronous API producer server and the asynchronous API consumer server.
1. Change the payload in the asynchronous API producer.
1. See the update in the app.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Set a `health` endpoint for your app.
* Receive a call from an asynchronous API producer.
* Store the call from the asynchronous API producer for processing later.

You can find the completed project on GitHub at [Local-Async-API-Consumer-Java](https://github.com/Fortellis/Local-Async-API-Consumer-Java).

In the next tutorial, you can see how to register your URL on Fortellis to receive requests from Asynchronous API Developers.
