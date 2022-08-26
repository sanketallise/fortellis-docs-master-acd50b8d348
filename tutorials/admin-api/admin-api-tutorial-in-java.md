---
title: Admin API Tutorial in Java
author: Nathaniel Sandberg
last updated: 5/23/2022
---

Fortellis has created a schema that API Developers can use to integrate with Fortellis.
API Developers may want to integrate and implement that schema for the following reasons:

* You can use the schema to protect your APIs and vet users.
* Fortellis only sends requests from users that you approve if you implement the Admin API.

You can use the [Admin API Spec](/docs/tutorials/admin-api/admin-api-specs) to receive requests from Fortellis at your endpoint,
and you can use the [Connection Callback Spec](/docs/tutorials/admin-api/fortellis-connection-callback) to approve those requests.

## What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create the `/activate` endpoint.  
* Create the `/deactivate` endpoint.
* Create an endpoint to retrieve your requests.
* Delete requests.
* Create a GitHub repository for your project.
* Deploy the endpoint to Heroku.

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

## Workflow

1. Enter your Admin API URL
    when you are updating your API.
1. Implement the Admin API spec to receive requests at that endpoint
    when Fortellis calls your Admin API URL.
1. Use an API or another method to get the connection requests
    that Fortellis sends to your Admin API URL.
1. Send the connectionId to `$[connectionCallbackEndpoint]/connections/:connectionId/callback` endpoint to accept the request.  

You do not have to take any action to refuse the connection.

## Prerequisites

You should have a good understanding of the following before starting this tutorial:

* A basic understanding of Java.
* A basic understanding of Git.
* A [JDK 8 or later](https://www.oracle.com/java/technologies/downloads) installed on your computer.
* A basic understanding of Maven.

## Setting the Path for Java

Use the following tutorial to set the path variable if you have not done that already: <https://www.geeksforgeeks.org/how-to-set-java-path-in-windows-and-linux>.

## Creating the Project

1. On the command line in the new directory, type `mvn archetype:generate -DgroupId=com.activate.endpoint -DartifactId=AdminAPI -Dpackagename=com.adminAPI -DarchetypeArtifactId=maven-archetype-webapp`.
1. Update the dependencies to the following:

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

1. Update the build in the `pom.xml` to the following:  

    ```xml
    <build>
        <finalName>Admin API</finalName>
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

1. Delete the `mainClass` tag from the `pom.xml` file.
1. Move the `index.jsp` file into the webapp folder.
1. Run the server.
    Type the following to start the server:

    ```bash
    mvn jetty:run
    ```

You can now go to `localhost:8080` and see `Hello World!` in the browser.

## Creating the Activate Endpoint

1. In the `src\main` folder, create the folder `java` folder with the `Activate.java` file.
1. Add the following in the `web.xml` file between the `web-app` tags to configure the server to receive requests at the endpoint.  

    ```xml
    <servlet>
        <servlet-name>
        Activate
        </servlet-name>  
        <servlet-class>
        Activate
        </servlet-class>
    </servlet>
    <servlet-mapping>
        <url-pattern>/activate</url-pattern>
        <servlet-name>Activate</servlet-name>
    </servlet-mapping>
    ```

1. In the `src\main\java\Activate.java` file, add the following imports:  

    ```java
    import java.io.IOException;
    import java.io.PrintWriter;

    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;

    import java.io.BufferedReader;
    import java.io.DataOutputStream;
    import java.io.InputStreamReader;
    import java.net.URL;
    import java.net.URLConnection;

    import java.io.*;

    import java.io.File;
    import java.nio.file.Files;
    import java.nio.file.Path;
    import java.nio.file.Paths;
    ```

1. In the `src\main\java\Activate.java` file, add the following to see the request in the console:  

    ```java
    @WebServlet("/activate")
    public class Activate extends HttpServlet{

        public Activate(){
            super();
        }

        protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
            PrintWriter out = response.getWriter();
            StringBuilder buffer = new StringBuilder();
            BufferedReader reader = request.getReader();
            String line;
            while ((line = reader.readLine()) != null) {
                buffer.append(line);
            }
            String connectionRequest = buffer.toString();
            System.out.println("This is your connection request: " + connectionRequest);
            newConnectionRequest(connectionRequest);

        }
        public void newConnectionRequest(String connectionRequest) {
            try{

                String localDir = System.getProperty("user.dir");
                File file = new File(localDir + "/webapps/connectionRequests.json");
                Path path = Paths.get(file.toString());
                FileInputStream fstream = new FileInputStream(file);
                DataInputStream in = new DataInputStream(fstream);
                BufferedReader br = new BufferedReader(new InputStreamReader(in));
                String strLine;
                String connectionRequestsPlaceholder = "";
                String connectionRequestReassembler = "";
                String connectionRequestWithAddedConnection = "";
                String completeListOfConnections = "";
                int x = 0;

                System.out.println("This is the connection request: " + connectionRequest);

            }catch(Exception e){
                System.err.println("Error: " + e.getMessage());
            }
        }
    }
    ```

1. Stop the server.  

    > Usually, you can press **Ctrl** + **C** to stop the server.
    > You may have to use the following operation for this tutorial:  
    >
    > 1. In another terminal, type **netstat -aon | grep 8080** to find the task running.
    > 2. To stop the task, type **tskill {PID}** where PID is the number that you get on the right side of the search.

1. Restart your server.  
    On the command line, type the following:  

    ```bash
    mvn jetty:run
    ```

You can now send the request to your local endpoint and see the request in the Java console.

```curl
curl -X POST 'http://localhost:8080/activate' -d '{
  "organizationInfo": {
    "id": "f807c9b4-d91f-4003-a4a4-de69c1d5b2ad",
    "name": "Super Car Dealership",
    "address": "1234 Some St. Columbus, OH 43235",
    "countryCode": "US",
    "phoneNumber": "(614) 555-5555"
  },
  "appInfo": {
    "id": "f807c9b4-d91f-4003-a4a4-de69c1d5b2ad",
    "name": "Awesome Application",
    "developer": "Awesome App Developer",
    "contactEmail": "contact@example.com",
    "appOrgId": "f807c9b4-d91f-4003-a4a4-de69c1d5b2ad"
  },
  "entityInfo": {
    "id": "f807c9b4-d91f-4003-a4a4-de69c1d5b2ad",
    "name": "Super Car Dealership",
    "address": "1234 Some St. Columbus, OH 43235",
    "countryCode": "US",
    "phoneNumber": "(614) 555-5555"
  },
  "solutionInfo": {
    "id": "f807c9b4-d91f-4003-a4a4-de69c1d5b2ad",
    "name": "Awesome Application",
    "developer": "Awesome App Developer",
    "contactEmail": "contact@example.com",
    "appOrgId": "f807c9b4-d91f-4003-a4a4-de69c1d5b2ad"
  },
  "userInfo": {
    "fortellisId": "user@example.com"
  },
  "apiInfo": {
    "id": "v1-appointments",
    "name": "Appointments",
    "implementationName": "Fortellis Spec 8"
  },
  "subscriptionId": "f807c9b4-d91f-4003-a4a4-de69c1d5b2ad",
  "connectionId": "f807c9b4-d91f-4003-a4a4-de69c1d5b2ad"
}'
```

## Saving the Data to a File

Java does not natively support JSON.
For this tutorial, we implement the `org.json.*` dependency to read the JSON and store it in a file for processing later.

1. Add the following imports:  

    ```java
    import java.io.BufferedInputStream;
    import java.io.FileInputStream;
    ```

1. Add the import for `json`.  
    In the imports at the top of the file, include the following:  

    ```java
    import org.json.*;
    ```

1. Add the dependency in the `pom.xml` file.  
    In the dependencies in the `pom.xml` file, type the following:  

    ```xml
    <dependency>
      <groupId>org.json</groupId>
      <artifactId>json</artifactId>
      <version>20180130</version>
    </dependency>
    ```

1. Add the script below the concatenated lines to add the new `connectionRequest` to the array:  

    ```java
    System.out.println("This is the concatenatedFile: " + concatenatedFile);
    //Make the concatenated lines a JSON object again.
    JSONObject objectForConcatenatedFile = new JSONObject(concatenatedFile);
    //Change the connectionRequests from the file into just an array.
    JSONArray parsedConnectionRequests = objectForConcatenatedFile.getJSONArray("connectionRequests");
    System.out.println("This is just the array of the connectionRequests: "+ parsedConnectionRequests);
    //Change the connectionRequest into an object
    JSONObject addObject = new JSONObject(connectionRequest);
    ```

1. Below that, add the object to the array and print out the final object:

    ```java
    //Put the connection request object into the array.
    parsedConnectionRequests.put(addObject);
    System.out.println("This is the object for the concatenated file: "+ objectForConcatenatedFile.toString(4));

    ```

1. Write the new connection request with the updated connection request to the `connectionRequest.json` file.  
    Type the following below the last line to write the concatenated `connectionRequests` to the `connectionRequests.json` file.

    ```java
    Path path = Paths.get(wholeFile.toString());
    String str = objectForConcatenatedFile.toString(4);
    byte[] arr = str.getBytes();
    try{
        Files.write( path, arr);
    }catch(IOException ex){
        System.out.print("Invalid Path");
    }
    ```

## Implementing the Admin API

Fortellis calls the `/activate` endpoint after a subscriber configures their app to use your API.
Fortellis includes the following:

* `organizationInfo`
* `appInfo`
* `userInfo`
* `apiInfo`
* `connectionId`

The `connectionId` uniquely identifies the request,
and you use it to accept the request using Fortellis Connection Callback API,
which we cover later in this tutorial.
You must also authorize requests to your `/activate` endpoint.
See below for more information.

**Note:** You can now call the endpoint from Postman or any other API testing tool at `http://localhost:8080/activate`:

* Use the Postman collection that you get when you initially submit your API for testing.
* Replace the URL in the activation request with the `http://localhost:8080/activate`.
* Continue to call the endpoint throughout the tutorial to test it before moving on to the next steps.
* Use the payload that you get when Fortellis sends activation requests to your `activate` endpoint found in the test collection.

### Sending the Activation Request to Your Server

```curl
curl -X POST http://localhost:8080/activate -H "accept: application/json" \
-H  "Content-Type: application/json" -d '{"organizationInfo":{"id":"2529a384-aee3-4b9a-af35-ed08e77dee15","name":"Super Car Dealership","address":"1234 Some St. Columbus, OH 43235","countryCode":"US","phoneNumber":"(614) 555-5555"},"appInfo":{"id":"f83eaff0-3f88-4ebd-9fc8-25f051632968","name":"Awesome Application","developer":"Awesome App Developer","contactEmail":"contact@example.com","appOrgId":"2059d5f5-bccb-40c8-937e-f217311feabd"},"entityInfo":{"id":"2529a384-aee3-4b9a-af35-ed08e77dee15","name":"Super Car Dealership","address":"1234 Some St. Columbus, OH 43235","countryCode":"US","phoneNumber":"(614) 555-5555"},"solutionInfo":{"id":"f83eaff0-3f88-4ebd-9fc8-25f051632968","name":"Awesome Application","developer":"Awesome App Developer","contactEmail":"contact@example.com","appOrgId":"2059d5f5-bccb-40c8-937e-f217311feabd"},"userInfo":{"fortellisId":"user@example.com"},"apiInfo":{"id":"v1-appointments","name":"Appointments","implementationName":"Fortellis Spec 8"},"subscriptionId":"2529a384-acc3-4b6a-a835-ed09e77dee15","connectionId":"2529a384-add3-4b6a-a935-ed09e45def12"}'
```

#### Response from the Server

```json
{
    "links": [
        {
            "href": "string",
            "method": "string",
            "title": "string",
            "rel": "string"
        }
    ]
}
```

### Verifying the Token

You should verify the tokens that Fortellis sends with activation requests.

1. Add the dependency in your `pom.xml` file to get the jwt verifier.  
    In the dependencies, type the following to get the library.  

    ```xml
    <dependency>
        <groupId>com.okta.jwt</groupId>
        <artifactId>okta-jwt-verifier</artifactId>
        <version>0.5.1</version>
    </dependency>
    <dependency>
        <groupId>com.okta.jwt</groupId>
        <artifactId>okta-jwt-verifier-impl</artifactId>
        <version>0.5.1</version>
        <scope>runtime</scope>
    </dependency>
    ```

1. Import the file in your `Activate.java` file.  
    At the top of the file with the imports, type the following:  

    ```java
    import com.okta.jwt.*;
    import java.time.Duration;
    ```

1. Get the authorization header and log it to the console.  
    At the top of the `doPost` method, type the following:  

    ```java
    //Get authorization header.
    String authorizationHeader = request.getHeader("Authorization");
    //Use the below code for troubleshooting the authorizationHeader.
    //System.out.println("This is the authorization header: " + authorizationHeader.replace("Bearer", ""));
    ```

1. Include the JWT verifier in the `Activate.java` file.  
    After getting the authorization header, include the following to add the JWT verifier:  

    ```java
    try{
        AccessTokenVerifier jwtVerifier = JwtVerifiers.accessTokenVerifierBuilder()
            .setIssuer("$[authServerUrl]")
            .setAudience("api_providers")
            .setSubject("{yourClientID}")
            .setConnectionTimeout(Duration.ofSeconds(1))
            .build();
        Jwt jwt = jwtVerifier.decode(authorizationHeader.replace("Bearer", ""));

    }catch(Exception e){
        System.out.println("You had a problem with the token.");
    }
    ```

1. Put the code below the `try/catch` statement in the `try` block.  
    Copy the code and put it in the try block to result in the following:  

    ```java
    try{
        AccessTokenVerifier jwtVerifier = JwtVerifiers.accessTokenVerifierBuilder()
            .setIssuer("$[authServerUrl]")
            .setAudience("api_providers")
            .setSubject("{yourClientID}")
            .setConnectionTimeout(Duration.ofSeconds(1))
            .build();
        //You need to set the subject to the client ID in here somewhere.
        Jwt jwt = jwtVerifier.decode(authorizationHeader.replace("Bearer", ""));
        System.out.println("This is the authentication decoded: " + jwt);
        PrintWriter out = response.getWriter();
        StringBuilder buffer = new StringBuilder();
        BufferedReader reader = request.getReader();
        String line;
        while ((line = reader.readLine()) != null) {
            buffer.append(line);
        }
        String connectionRequest = buffer.toString();
        System.out.println("This is your connection request: " + connectionRequest);
        newConnectionRequest(connectionRequest);

    }catch(Exception e){
        System.out.println("You had a problem with the token.");
    }
    ```

    **Note:** You now must include a valid token when you send a new connection request to your endpoint.
        Now send the response that Forellis expects to receive from the activate endpoint.

1. Return the response that Forellis expects to receive from the `/activate` endpoint.  
    Include the following below `nexConnectionRequest(connectionRequest)`:

    ```java
    PrintWriter activationResponse = response.getWriter();
    response.setContentType("application/json");
    response.setCharacterEncoding("UTF-8");
    activationResponse.print("{\"links\": [{\"href\": \"localhost:3000\", \"rel\": \"self\", \"method\": \"post\", \"title\": \"Activation Request\"}]}");
    activationResponse.flush();
    ```

### Successfully Sending a Request to Your Local Admin Implementation

You can now get a token from Fortellis and send requests to your local Admin API implementation.

1. Get a token from Fortellis.  
    In the Postman collection in the Admin API Testing folder,
    click the **Generate Infrastructure Bearer Token**,
    and then click **Send** to get your token.
1. Copy the `access_token` that you receive in the response body.
1. Call your local endpoint `http://localhost/3000/activate` with the Bearer token in the header.  
    In the `/activate` request in Postman, click **Authorization**,
    and paste your token into **Token**.  
    **Note:** The token expires in one hour.
    You should now be able to successfully send a request.

```curl
curl -X POST 'http://localhost:8080/activate' -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImI5YjY0ZWQwLWI2YTItMTFlOC1hYjY2LWU5OTdmMzA4OGI0ZSJ9.eyJjaWQiOiJRbk54TG1hUkVUV0NlVndsRlR5SDBScG5ydDdLanJPbyIsImp0aSI6IkFULndIcUFSOE8weWJOT2dFZFpOTHZOeHpUN1FicXI3bGpQTHg2TGRsRjZ5V3ciLCJzY3AiOlsiYW5vbnltb3VzIl0sInN1YiI6IlFuTnhMbWFSRVRXQ2VWd2xGVHlIMFJwbnJ0N0tqck9vIiwiaWF0IjoxNjUzNTc0NjM5LCJleHAiOjE2NTM1NzgyMzksImF1ZCI6ImFwaV9wcm92aWRlcnMiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3In0.UDkrRYcUJVtMdd4JPIlmSm0OG1DjY7M_ebPRsRDG73Xk_aLS4gGrzFVlS7POzQt12kV_Q9US18ixBcaQFCO08kAsNv-W35H7P0e7Cpuf5dIkuyb6jQYQBkmbd7roEzvqUK5Gp8A8DRadZzVoESw8fEUm1JumuMkZ5HHRkpG2gsWDQ2dYUnDzQxVbVe2Bz1wlTma5d4x-5JMRCGJofytiBHF26ZTPFCHNPqNsaIgImw2mRCKi6_3Hi2a3HmWUiM8MzP5U3kGuVXZlbn-LjwYVUWediWKBmuD4Tpkxz48ZACUPFpAklW9TfJS_g3W1LYe9PRBdG7EfJRnBPWtK3DSIZQ" -d '{
  "organizationInfo": {
    "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
    "name": "Super Car Dealership",
    "address": "1234 Some St. Columbus, OH 43235",
    "countryCode": "US",
    "phoneNumber": "(614) 555-5555"
  },
  "appInfo": {
    "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
    "name": "Awesome Application",
    "developer": "Awesome App Developer",
    "contactEmail": "contact@example.com",
    "appOrgId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00"
  },
  "entityInfo": {
    "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
    "name": "Super Car Dealership",
    "address": "1234 Some St. Columbus, OH 43235",
    "countryCode": "US",
    "phoneNumber": "(614) 555-5555"
  },
  "solutionInfo": {
    "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
    "name": "Awesome Application",
    "developer": "Awesome App Developer",
    "contactEmail": "contact@example.com",
    "appOrgId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00"
  },
  "userInfo": {
    "fortellisId": "user@example.com"
  },
  "apiInfo": {
    "id": "v1-appointments",
    "name": "Appointments",
    "implementationName": "Fortellis Spec 8"
  },
  "subscriptionId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
  "connectionId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00"
}'
```

## Creating an Endpoint to Get Your Requests

You can use this endpoint to get the requests that Fortellis has sent to you. You can then accept the connection at the [Fortellis Connection Callback](/docs/tutorials/admin-api/fortellis-connection-callback/) endpoint.

1. Create a new file in the java folder, `GetConnectionRequests.java`.
1. Include the import to receive the GET request and retrieve the data in the `connectionRequests.json` file.  
    Include the following imports at the top of the `GetConnectionRequests.java` file:  

    ```java
    import java.io.IOException;
    import java.io.PrintWriter;

    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;

    import java.io.BufferedReader;
    import java.io.DataOutputStream;
    import java.io.InputStreamReader;
    import java.net.URL;
    import java.net.URLConnection;

    import java.io.*;

    import java.io.File;
    import java.nio.file.Files;
    import java.nio.file.Path;
    import java.nio.file.Paths;
    ```

1. Create the servlet endpoint.  
    Include the following method in the file:  

    ```java
    @WebServlet("/getConnectionRequests")
    public class GetConnectionRequests extends HttpServlet{

        public GetConnectionRequests(){
            super();
        }
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{
  
        }
    }
    ```

1. Add the code to read the file and return the file in the response to the get request.  
    With the `doGet` method, type the following:  

    ```java
    try{
        ClassLoader classloader = Thread.currentThread().getContextClassLoader();
        InputStream is = classloader.getResourceAsStream("connectionRequests.json");
        DataInputStream in = new DataInputStream(is);
        BufferedReader br = new BufferedReader(new InputStreamReader(in));
        String concatenatedFile = "";
        String strLine;
        //Put all the lines from the file back together to make the original object.
        while((strLine = br.readLine()) != null){
            //System.out.println(strLine);
            concatenatedFile = concatenatedFile + strLine;
        }
        System.out.println("This is the concatenatedFile: " + concatenatedFile);

        PrintWriter activationRequests = response.getWriter();
        getConnectionRequests.print(concatenatedFile);
        getConnectionRequests.flush();

    }catch(Exception ex){
        ex.printStackTrace();
    }
    ```

1. In `src/main/webapp/WEB-INF/web.xml` file, add the `/getConnectionRequests` endpoint.  
    In the file, type the following below the `activate` servlet:  

    ```xml
    <servlet>
        <servlet-name>
        GetConnectionRequests
        </servlet-name>  
        <servlet-class>
        GetConnectionRequests
        </servlet-class>
    </servlet>
    <servlet-mapping>
        <url-pattern>/getConnectionRequests</url-pattern>
        <servlet-name>GetConnectionRequests</servlet-name>
    </servlet-mapping>
    ```

You can now send a get request to the `getConnectionRequests` endpoint and receive all your connection requests that Fortellis has sent you.

```curl
curl -v 'http://localhost:8080/getConnectionRequests' -H "Cache-Control: no-cache"
```

**Note:** You can now get your connection requests,
    but for this tutorial, we do not protect or secure the endpoint in any way.

### Validating Data to the Activate Endpoint

You may want to validate the payload coming to your Admin API to ensure the data matches the schema that you have implemented,
but we do not cover this is this tutorial.

## Creating the Endpoint to Delete the Requests

You do not need to take any action to reject a connection.
For this step, we simply remove the request,
and Fortellis never connects the subscriber to the API.
For this tutorial, we do not protect or secure the delete endpoint,
but you may want to in your implementation.

### Adding the Delete Endpoint

1. Create the `DeleteRequests.java` file.  
1. Get all the imports for the delete request.  
    At the top of the file, include the following imports:  

    ```java
    import java.io.IOException;
    import java.io.PrintWriter;

    import javax.servlet.ServletException;
    import javax.servlet.annotation.WebServlet;
    import javax.servlet.http.HttpServlet;
    import javax.servlet.http.HttpServletRequest;
    import javax.servlet.http.HttpServletResponse;

    import java.io.BufferedReader;
    import java.io.DataOutputStream;
    import java.io.InputStreamReader;
    import java.net.URL;
    import java.net.URLConnection;

    import java.io.*;

    import java.io.File;
    import java.nio.file.Files;
    import java.nio.file.Path;
    import java.nio.file.Paths;
    ```

1. Create the endpoint to delete the request.  
    Below the imports, add the following to create the delete method:  

    ```java
    @WebServlet("/deleteRequest")
    public class DeleteRequest extends HttpServlet{
        public GetConnectionRequests(){
            super();
        }
        protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException{

        }
    }
    ```

1. In `src/main/webapp/WEB-INF/web.xml` file, add the `/deleteRequest` endpoint.  
    In the file, type the following below the `activate` servlet:  

    ```xml
    <servlet>
        <servlet-name>
        DeleteRequest
        </servlet-name>  
        <servlet-class>
        DeleteRequest
        </servlet-class>
    </servlet>
    <servlet-mapping>
        <url-pattern>/deleteRequest</url-pattern>
        <servlet-name>DeleteRequest</servlet-name>
    </servlet-mapping>
    ```

1. Read the data from the body of the request.  
    Include the following code at the beginning of the delete request:  

    ```java
    StringBuilder buffer = new StringBuilder();
    BufferedReader reader = request.getReader();
    String line;
    while ((line = reader.readLine()) != null) {
        buffer.append(line);
        buffer.append(System.lineSeparator());
    }
    JSONObject requestToDelete = new JSONObject(buffer.toString());
    System.out.println("This is the JSON Object for the data: " + requestToDelete);
    ```

    **Note:** With the Maven server running, you can now send a request to delete a request and see the data in the console.

    ```curl
    curl -v -X "DELETE" 'http://localhost:8080/deleteRequest' -H "Cache-Control: no-cache" -d '{
        "organizationInfo": {
            "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
            "name": "Super Car Dealership",
            "address": "1234 Some St. Columbus, OH 43235",
            "countryCode": "US",
            "phoneNumber": "(614) 555-5555"
        },
        "appInfo": {
            "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
            "name": "Awesome Application",
            "developer": "Awesome App Developer",
            "contactEmail": "contact@example.com",
            "appOrgId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00"
        },
        "entityInfo": {
            "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
            "name": "Super Car Dealership",
            "address": "1234 Some St. Columbus, OH 43235",
            "countryCode": "US",
            "phoneNumber": "(614) 555-5555"
        },
        "solutionInfo": {
            "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
            "name": "Awesome Application",
            "developer": "Awesome App Developer",
            "contactEmail": "contact@example.com",
            "appOrgId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00"
        },
        "userInfo": {
            "fortellisId": "user@example.com"
        },
        "apiInfo": {
            "id": "v1-appointments",
            "name": "Appointments",
            "implementationName": "Fortellis Spec 8"
        },
        "subscriptionId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
        "connectionId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00"
    }'
    ```

### Writing the New Object to `connectionRequest.json`

1. Get the object from the `connectionRequest.json` file.  
    Add the following code beneath the line to get the body from the request:  

    ```java
    ClassLoader classLoader = getClass().getClassLoader();
    File  wholeFile =new File (classLoader.getResource("connectionRequests.json").getFile());
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
    ```

1. Get just the array from the object to parse it.  
    Below the code to get the JSON object, type the following:  

    ```java
    //Make the concatenated file into an object.
    JSONObject objectForConcatenatedFile = new JSONObject(concatenatedFile);
    //Change the connectionRequests from the file into just an array.
    JSONArray parsedConnectionRequests = objectForConcatenatedFile.getJSONArray("connectionRequests");
    System.out.println("This is the array: " + parsedConnectionRequests);
    ```

1. Find the index for the connection request.  
    Below the array, type the following:  

    ```java
     int index = 0;
    for( int i = 0; i < parsedConnectionRequests.length(); i++){
        String savedConnectionRequest = parsedConnectionRequests.getJSONObject(i).getString("connectionId");
        String requestToDeleteConnectionId = requestToDelete.getString("connectionId");
        System.out.println("A save connection request " + savedConnectionRequest);
        if(savedConnectionRequest.equals(requestToDeleteConnectionId)){
            index = i;
        }
    }
    System.out.println("This is the index for the connection to delete: " + index);
    ```

1. Remove the object in the delete request from the `connectionRequests` object.  
    Add the following under the code to create and log the new `connectionRequests`:  

    ```java
    parsedConnectionRequests.remove(index);
    System.out.println("This is the new object that we are sending to the file: " + objectForConcatenatedFile);
    ```

1. Write the new object to the `connectionRequests.json` file.  
    Add the following code below the new object:  

    ```java
    Path path = Paths.get(wholeFile.toString());
    String str = objectForConcatenatedFile.toString(4);
    byte[] arr = str.getBytes();
    try{
        Files.write( path, arr);
    }catch(IOException ex){
        System.out.print("Invalid Path");
    }
    ```

You can now test your delete endpoint.
Send a delete request to your endpoint with the full body of one of your connection requests.
The delete endpoint uses the `connectionId` in the body of your request to find the index of the connection request
that you are deleting and removes it from the array of connection requests.
You can use the curl request below to delete one of your requests.

**Note:** Use a connection request from your `connectionRequests.json`.

```curl
curl -v -X "DELETE" 'http://localhost:8080/deleteRequest' -H "Cache-Control: no-cache" -d '{
  "organizationInfo": {
    "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
    "name": "Super Car Dealership",
    "address": "1234 Some St. Columbus, OH 43235",
    "countryCode": "US",
    "phoneNumber": "(614) 555-5555"
  },
  "appInfo": {
    "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
    "name": "Awesome Application",
    "developer": "Awesome App Developer",
    "contactEmail": "contact@example.com",
    "appOrgId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00"
  },
  "entityInfo": {
    "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
    "name": "Super Car Dealership",
    "address": "1234 Some St. Columbus, OH 43235",
    "countryCode": "US",
    "phoneNumber": "(614) 555-5555"
  },
  "solutionInfo": {
    "id": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
    "name": "Awesome Application",
    "developer": "Awesome App Developer",
    "contactEmail": "contact@example.com",
    "appOrgId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00"
  },
  "userInfo": {
    "fortellisId": "user@example.com"
  },
  "apiInfo": {
    "id": "v1-appointments",
    "name": "Appointments",
    "implementationName": "Fortellis Spec 8"
  },
  "subscriptionId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00",
  "connectionId": "110ca93a-a1c2-49a6-a156-b3fd01d76d00"
}'
```

## Deploying to Heroku

Please remember that Heroku lets the server sleep after 30 minutes of inactivity.
To keep the server awake, you may want to implement a strategy for waking up your server, such as a cron job, or use a different platform to deploy your Admin API.

### Creating a GitHub Repository

You may have to get a personal access token
if you are using your personal account.
See [Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) to create your token.
You should enter this token instead of your GitHub password
when you are pushing repositories to GitHub.

1. Sign up for a GitHub account at <https://github.com/>.
1. In the upper-left corner, click the **New** button.
1. Type the **Repository name**.
1. Type a **Description**.  
1. Click **Create Repository**.
1. From the command line, type the following to push the repository to GitHub:  
    1. **git init**  
    1. **git add ***  
    1. **git commit -m "Initial Commit"**  
    1. **git remote add origin {your repository}**  
        **Note:** Get the repository name from GitHub.  
    1. **git push -u origin master**
1. Enter your the following when GitHub prompts use:  
    * Your GitHub username.
    * Your GitHub password or your authentication code in the password field.

### Setting Up the Project For Deployments

1. Add the plugin so the build can get the dependencies for your project.  
    In the `pom.xml` file, include the following in the `plugins`:  

    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-dependency-plugin</artifactId>
        <version>3.0.1</version>
        <executions>
            <execution>
                <id>copy-dependencies</id>
                <phase>package</phase>
                <goals><goal>copy-dependencies</goal></goals>
            </execution>
        </executions>
    </plugin>
    ```

1. Delete the other plugin:  

    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <version>2.22.0</version>
    </plugin>
    ```

1. Specify the version of Java that you used for the project.  
    Create a `system.properties` file in the Admin API folder and include the following line:  

    ```bash
    java.runtime.version=1.8
    ```

1. Tell Heroku how to start the app.  
    Create a Procfile in the `AdminAPI` folder and include the following line:  

    ```heroku
    web: java $JAVA_OPTS -jar target/dependency/webapp-runner.jar --port $PORT target/*.war
    ```

    **Note:** You must capitalize Procfile for Heroku to find and implement it.

1. Add the plugin for the Heroku SDK:  

    ```xml
    <plugin>
        <groupId>com.heroku.sdk</groupId>
        <artifactId>heroku-maven-plugin</artifactId>
        <version>3.0.3</version>
    </plugin>
    ```

1. Add the configuration of the Heroku plugin:  

    ```xml
    <plugin>
        <groupId>com.heroku.sdk</groupId>
        <artifactId>heroku-maven-plugin</artifactId>
        <version>3.0.3</version>
        <configuration>
            <appName>AdminAPI</appName>
        </configuration>
    </plugin>
    ```

1. Add the build pack to the to the configuration:  

    ```xml
    <plugin>
        <groupId>com.heroku.sdk</groupId>
        <artifactId>heroku-maven-plugin</artifactId>
        <version>3.0.3</version>
        <configuration>
            <appName>AdminAPI</appName>
            <buildpacks>
                <buildpack>heroku/jvm</buildpack>
            </buildpacks>
        </configuration>
    </plugin>
    ```

### Creating the War File to Deploy

1. Add the following executions to the `maven-dependency-plugin` in the `pom.xml` file to deploy the app.  

    ```xml
    <executions>
        <execution>
            <id>copy-dependencies</id>
            <phase>package</phase>
            <goals>
                <goal>copy</goal>
            </goals>
            <configuration>
                <artifactItems>
                    <artifactItem>
                        <groupId>com.heroku</groupId>
                        <artifactId>webapp-runner</artifactId>
                        <version>9.0.38.0</version>
                        <destFileName>webapp-runner.jar</destFileName>
                    </artifactItem>
                </artifactItems>
            </configuration>
        </execution>
    </executions>
    ```

1. Install Maven to create the `AdminAPI.war` file.
    On the command line, type the following:  

    ```bash
    mvn install
    ```

    > You must have `<packaging>war</packaging>` in your server to run it locally

1. Run the package command to create the tomcat files.  
    On the command line, type the following:  

    ```bash
    mvn package
    ```

1. Remove the target folder from source control.  
    1. Ignore the target folder in source control.  
        Create the `.gitignore` file and include the target folder with the following line:  

        ```bash
        target
        ```

    1. Remove the target folder from source control.  
        On the command line, type the following:  

        ```bash
        git rm -r --cached target
        ```

    1. Add the changes to GitHub.  
        On the command line, type the following:  

        ```bash
        git add *
        ```

    1. Add the `.gitignore` file.  
        On the command line, type the following:  

        ```bash
        git add .gitignore
        ```

    1. Commit your changes.  
        In the terminal, type a commit, such as the following:  

        ```bash
        git commit -m "Updated to remove the target directory, add the .gitignore file, and add the updates to the other new files."
        ```

    1. Push your changes to the remote repository.  
        On the command line, type the following:  

        ```bash
        git push
        ```

You can now deploy the Admin API to Heroku.  

### Creating the App in Heroku

1. Create a [Heroku](https://heroku.com) account if you don't already have one.  
1. In the upper-right corner, click **New**.  
1. Click **Create new app**.  
1. Type the app name.  
    **Note:** You must use a unique app name with only the following:  
    * Lower case letters  
    * Dashes  
    * Numbers  
1. In Heroku, choose a region.  
1. Click **Create app**.  

### Integrating Your Heroku App with GitHub

1. In the middle of the page, click **GitHub**.  
1. Click **Connect to GitHub** to connect your account to your GitHub account.
1. If you are prompted, click **Authorize Heroku** to grant access to your GitHub account.
1. Type the repo name.
1. Click **Search**.
1. Click **Connect**.  
1. At the bottom of the page, click **Deploy Branch**.  
    **Note:** You may have to wait a few moments as Heroku downloads the dependencies.

You can now click **View** at the bottom of the page to see your app.  

## Adding the Admin API URL to Your API

1. Sign in to the [Developer Network]($[devNetworkUrl]).
1. Do the following to select the organization that the API belongs to:
    1. Hover over your name in the upper-right corner.
    1. Click **Change**.
    1. Click the organization that the API belongs to.
    1. Click **OK**.
1. Click **View All** next to **APIs**.
1. Click the API you want to add the **Admin API URL** to.
1. Click **Edit** next to **Subscription Activation** to change the method to **Admin API**.
1. Click **Admin API**, and enter the **Admin API URL**.  
    **Note:** Do not include the `/activate` endpoint.
    Fortellis appends the `/activate` endpoint to the URL that you enter.
1. Click **Save**.

## Approving Connection Requests

You need the `connectionId` from the activation request made by Fortellis.
The `connectionId` uniquely identifies the connection in Fortellis' system.

Store the connection request in a database to do the following:

* Verify subscribers.
* Maintain subscription information.
* Improve performance of your API.

1. Use the `/connectionRequests` endpoint to get your connections.
1. Using your Fortellis Client Key and Client Secret, get your Fortellis token from `$[authServerUrl]`.  
    See [Authorization on Fortellis](/docs/tutorials/solution-integration/auth/) for more information.
1. Create a new request in Postman.
1. Under **Auth**, enter your `access_token`.
1. Use the following URL to accept the connection: `$[subscriptionCallbackUrl]/connections/{connectionId}/callback`.

    ```bash
    curl --location --request POST '$[subscriptionCallbackUrl]/connections/3fc27153-1131-45db-8ffd-c987087055e8/callback' \
    --header 'Content-Type: application/json' \
    --data-raw '{"status": "accepted","error": "string"}' \
    --header 'Authorization: Bearer {your token}'
    ```

    **Note:** You should receive the following back: `OK`.

1. After you approve connections, send the JSON object for the request that you approved to the `deleteRequest` endpoint to remove approved entries.  

## Deactivation Requests

The Subscriber may request to deactivate the subscription for the following reasons:

* The Subscriber is going out of business.
* The Subscriber no longer uses the solution that works with that API.
* The Subscriber wants to change API Developers and moves to a different API.

API Developers may request to have a subscriber deactivated through Fortellis by contacting [Fortellis Support](mailto:support@fortellis.io).

### Creating the Deactivate Endpoint

API Developers receive the `connectionId` at the `/deactivate` endpoint and can clean up their backend system when they are ready.

1. Copy the `Activate.java` file.
1. Create a new `Deactivate.java` file in the java folder.
1. Paste a the contents of the `Activate.java` file in the `Deactivate.java` folder.
1. Change `@WebServlet("/activate")` to  `@WebServlet("/deactivate")`.
1. Add the `/deativate` endpoint to the `src/main/webapp/WEB-INF/web.xml` file with the following code:  

    ```xml
    <servlet-mapping>
        <url-pattern>/deactivate</url-pattern>
        <servlet-name>Deactivate</servlet-name>
    </servlet-mapping>
    <servlet>
        <servlet-name>
        Deactivate
        </servlet-name>
        <servlet-class>
        Deactivate
        </servlet-class>
    </servlet>
    <servlet-mapping>
        <url-pattern>/deactivate/*</url-pattern>
        <servlet-name>Deactivate</servlet-name>
    </servlet-mapping>
    ```

1. Get the value in the parameter and save it to the file.  
    Get the parameter in the endpoint with the following:  

    ```java
    String UUID = request.getPathInfo().toString();
    System.out.println("This is the UUID: " + UUID);

    String deactivateRequest = "{\"connectionId\": \"" + UUID + "\"}";
    System.out.println("This is your deactivation request: " + deactivateRequest);
    newDeactivateRequest(deactivateRequest);
    PrintWriter deactivationResponse = response.getWriter();
    response.setContentType("application/json");
    response.setCharacterEncoding("UTF-8");
    deactivationResponse.print("{\"links\": [{\"href\": \"localhost:3000\", \"rel\": \"self\", \"method\": \"post\", \"title\": \"Deactivation Request\"}]}");
    deactivationResponse.flush();
    ```

1. Create a new file `src/main/resources/deactivationRequests.json`, and include the following JSON object in the file:  

    ```javascript
    {"deactivationRequests":[]}
    ```

### Saving the Deactivation `connectionId`

1. Create a new file `src/main/resources/deactivationRequests.json` to save the request to.
1. Include the following JSON object in the file.  
    Type the following to create the `deactivationRequests` object:

    ```javascript
    {"deactivationRequests":[]}
    ```

    ```curl
    curl -X POST 'http://localhost:8080/deactivate/{connectionId}' -H "Authorization: Bearer eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6ImI5YjY0ZWQwLWI2YTItMTFlOC1hYjY2LWU5OTdmMzA4OGI0ZSJ9.eyJjaWQiOiJRbk54TG1hUkVUV0NlVndsRlR5SDBScG5ydDdLanJPbyIsImp0aSI6IkFULnh1azJrbFJSWXZ1bXJSd0thWFhhaXpSTzZ0U3JqNURGY0lKR1AzeDFvOTAiLCJzY3AiOlsiYW5vbnltb3VzIl0sInN1YiI6IlFuTnhMbWFSRVRXQ2VWd2xGVHlIMFJwbnJ0N0tqck9vIiwiaWF0IjoxNjU0NTUwMDQwLCJleHAiOjE2NTQ1NTM2NDAsImF1ZCI6ImFwaV9wcm92aWRlcnMiLCJpc3MiOiJodHRwczovL2lkZW50aXR5LWRldi5mb3J0ZWxsaXMuaW8vb2F1dGgyL2F1czFuaTVpOW45V2t6Y1lhMnA3In0.UMjhjcatvnN_pP-gVqsSnOcK2bdCyDXoJ-UP-keuq9IY9i7-6-3vhODynStc2W760q_U3xtkwroDHAWcLznhhAoX6tlDHuFiWSlQgZVDPOvkrFJc5pCB22_J36YjeHINjHmUafcK3Yfm8CcuEtML3Rp_7ZVVhx9Xn0ceRB3xgFNl9loCQ0bXOIZ1rowAi2fEQQgiNTVPmhF2zwDnGzagKFP-fofeB9UmqSEXUsFMkSdzgYsCHJOkVVnNuR1VwwiYaaekYhyxAPxIbg1h4v5bTPXidRqFZs9EWSURSiiS1jGmjhHCivlJVyW3PPUqjHbFk42zuoK7Fw1ffY9rJNyowA" -H "Cache-Control: no-cache"
    ```

### Getting the Connections to Deactivate

1. Copy the `GetConnectionRequests.java` file to a new file and name the file `GetDeactivationRequests.java`.
1. Change the endpoint in the `@WebServlet` to `getDeactivationRequests`.
1. Change the file the code reads from `connectionRequests.json` to `deactivationRequests.json`.
1. Change the name of the public class from `GetConnectionRequests` to `GetDeactivationRequests`.
1. Change the variable name from `getConnectionRequests` to `getDeactivationRequests`.  
1. Change the name in the constructor from `GetConnectionRequests` to `GetDeactivationRequests`.  
1. Add the endpoint to the `src/main/webapp/WEB-INF\web.xml` file.  
    Add the following servlet:  

    ```xml
    <servlet-mapping>
        <url-pattern>/getDeactivationRequests</url-pattern>
        <servlet-name>GetDeactivationRequests</servlet-name>
    </servlet-mapping>
    <servlet>
        <servlet-name>
        GetDeactivationRequests
        </servlet-name>  
        <servlet-class>
        GetDeactivationRequests
        </servlet-class>
    </servlet>
    ```

### Compile the Code for the Deactivate Endpoint

1. Run the package command to compile the code and create your new class.  
    On the command line in your Java folder, type the following:  

    ```bash
    mvn package
    ```

1. Run the app.  
    On the command line, type the following:  

    ```bash
    mvn jetty:run
    ```

1. Use Postman or another API testing tool to get the connections that have been deactivated.
1. Push your repository to GitHub and redeploy to Heroku to get your deactivation requests.

You can now receive deactivation requests and remove them from your backend systems.
Use the following curl request to get your deactivation requests.

```curl
curl http://localhost:8080//getDeactivationRequests -H "accept: application/json" \
-H  "Content-Type: application/json"
```

Fortellis sends the Admin API Fortellis tokens and not API Provider tokens.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create the `/activate` endpoint.  
* Create the `/deactivate` endpoint.
* Create an endpoint to retrieve your requests.
* Delete requests.
* Create a GitHub repository for your project.
* Deploy the endpoint to Heroku.

Store the information you receive when you get an activation request to find subscriptions and other information you need to process requests.
Remember the following about the Heroku deployment:

* The Heroku server sleeps after 30 minutes.
* You may need to implement a more scalable solution or a production environment.

You can download the project from GitHub at [Admin-API-Implementation-Java](https://github.com/Fortellis/Admin-API-Implementation-Java).

If you choose to download the example repository, do the following:

1. In the directory of your choice download the repository from GitHub.  
    On the command line, type the following:  

    ```curl
    git clone git@github.com:Fortellis/Admin-API-Implementation-Java.git
    ```

1. Install the dependencies.  

    ```bash
    mvn install
    ```

1. Follow the instructions in [Deploying to Heroku](#deploying-to-heroku).
    1. Follow the instructions for [Creating a GitHub Repository](#creating-a-github-repository) to create your own GitHub repository.  
    1. Follow [Creating the App in Heroku](#creating-the-app-in-heroku).  
    1. Follow [Integrating Your Heroku App with GitHub](#integrating-your-heroku-app-with-github) to pr
    1. Follow [Adding the Admin API URL to Your API](#adding-the-admin-api-url-to-your-api) to add your URL to your API on Fortellis.
1. Do one of the following:
    * To approve connection requests, follow the steps in [Approving Connection Requests](#approving-connection-requests).
    * To get deactivation requests, follow the steps in [Getting the Connections to Deactivate](#getting-the-connections-to-deactivate).
