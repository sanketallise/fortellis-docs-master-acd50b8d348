---
title: Building an HTTP Callback to Receive Messages in Java
author: Nathaniel Sandberg
last updated: 8/1/2022
---

In this tutorial, we expand on the previous tutorial [Integrating Your App and an Asynchronous API Producer in Java](/docs/tutorials/event-relay/tutorial-integrate-an-async-api-integration-with-an-app-java) in creating an asynchronous app to deploy your endpoint to a public URL
that Fortellis can send events to.
Use this tutorial to find information on how to create your webhook and register your app on Fortellis to receive events.

> You must associate the `Data-Owner-Id` in the event header with the organization
> that is using the app.
> If you have multiple organizations subscribed to your app,
> you must send the event to the appropriate subscribed organization within the app.
> You can use the tutorial [Accessing Subscription Details Programmatically](/docs/tutorials/app-toolbox/using-the-subscriptions-endpoint) to get your subscriptions and match the `Data-Owner-Id` to an organization subscribed to your app.

## Tutorial

### What You Will Learn in This Tutorial

In this tutorial, you should learn how to do the following:

* Create an endpoint to show the contents of the `./queue.json` file.
* Register the app to Fortellis.
* Create an app that can consume events from Fortellis.
* Verify the token that Fortellis sends with the event.

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

### Creating the Endpoint to Show the Queue

First, we create an endpoint so we can see when the `queue.json` file updates on the server.

* Create another get endpoint to show the contents of the file.  
    Beneath the `doPost` endpoint that receives the events in the `HelloWorld.java`, use the following to return the contents of the file:

    ```java
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException,IOException{
        ClassLoader classLoader = getClass().getClassLoader();
        File  wholeFile =new File (classLoader.getResource("queue.json").getFile());
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
        PrintWriter healthCheckResponse = response.getWriter();
        response.setContentType("application/json");
        response.setCharacterEncoding("UTF-8");
        response.setStatus(HttpServletResponse.SC_OK);
        healthCheckResponse.print(concatenatedFile);
        healthCheckResponse.flush();
    }
    ```

You can now use the get request to get updates and process them when ready.
For this tutorial, we assume that you would automate the process of removing requests
that you have already processed.
To see events sent to you, do the following:

1. Type the following to start the server:  

    ```curl
    mvn jetty:run
    ```

1. Go to `http://localhost:9999/helloworld/event` in your browser.

Do one of the following to stop the server:  

* On the command line, type **Ctrl**+**C** to stop the server.
* Use another command line to find the server and stop it.  
    1. On the command line, type the following to find the running app:  

        ```bash
        netstat -aon | grep 9999
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

### Creating Your Public Endpoint

You must create a publicly available endpoint for Fortellis to send events to your API.
You can use any platform you have.
For this tutorial, we use a simple deployment with Heroku.

> Heroku apps fall asleep after 30 minutes of inactivity.
> Only use this deployment for demonstration purposes.
> For a more robust solution, use a server with more up-time.

### Creating Your Heroku Project

You now create a public project with an endpoint that Fortellis can post events to.

#### Creating a GitHub Repository

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
    > If you downloaded the repository,
    > first type the following on the command line to clear the origin:  
    >
    > ```bash
    > git remote rm master
    > ```

1. From the command line, type the following to push the repository to GitHub:  
    1. **git init**  
    1. **git add ***  
    1. **git commit -m "Initial Commit"**  
    1. **git remote add origin {your repository}**  
        **Note:** Get the repository name from GitHub.  
    1. **git push -u origin master**
1. Enter the following when GitHub prompts you:  
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
                <goals><goal>copy</goal></goals>
            </execution>
        </executions>
    </plugin>
    ```

1. Specify the version of Java that you used for the project.  
    Create a `system.properties` file in the `webhookEndpoint` folder and include the following line:  

    ```bash
    java.runtime.version=1.8
    ```

    **Note:** Use `11` if you configured the Maven project to use Java 11 in the previous tutorial.

1. Tell Heroku how to start the app.  
    Create a Procfile in the `webhookEndpoint` folder and include the following line:  

    ```heroku
    web: java $JAVA_OPTS -jar target/dependency/webapp-runner.jar --port $PORT target/*.war
    ```

    **Note:** You must capitalize Procfile for Heroku to find and implement it.

    > Use the next three steps to create one plugin.  

1. Add the plugin for the Heroku SDK.  
    In the `pom.xml` file, add the following plugin:  

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
            <appName>webhookEndpoint</appName>
        </configuration>
    </plugin>
    ```

1. Add the build pack to the configuration:  

    ```xml
    <plugin>
        <groupId>com.heroku.sdk</groupId>
        <artifactId>heroku-maven-plugin</artifactId>
        <version>3.0.3</version>
        <configuration>
            <appName>webhookEndpoint</appName>
            <buildpacks>
                <buildpack>heroku/jvm</buildpack>
            </buildpacks>
        </configuration>
    </plugin>
    ```

### Creating the War File to Deploy

1. Add the following in the `pom.xml/plugins/plugin/executions/execution/configuration` of the `maven-dependency-plugin` in the `pom.xml` file to deploy the app.  

    ```xml
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        ...
        <execution>
            ...
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
        ...
    </plugin>
    ```

1. Install Maven to create the `webhookEndpoint.war` file.  
    On the command line, type the following:  

    ```bash
    mvn install
    ```

    > You must have `<packaging>war</packaging>` in your server to run it locally.

1. Run the package command to create the `.war` file.  
    On the command line, type the following:  

    ```bash
    mvn package
    ```

1. Remove the target folder from source control
    if you haven't already.  
    1. Ignore the target folder in source control.  
        Create the `.gitignore` file in the `webhookEndpoint` folder and include the target folder with the following line:  

        ```bash
        target
        ```

    1. Remove the target folder from source control.  
        On the command line, type the following:  

        ```bash
        git rm -r --cached target
        ```

    1. Add the `.gitignore` file.  
        On the command line, type the following:  

        ```bash
        git add .gitignore
        ```

1. Add the changes to GitHub.  
    On the command line, type the following:  

    ```bash
    git add *
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

You can now deploy the webhookEndpoint to Heroku.  

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

You can now do the following:

* Click **View** at the bottom of the page to see your app.  
    * Add `/health` to the URL to see the `HealthCheck` endpoint.
    * Add `/helloworld/event` to the endpoint to see the events in the `queue.json` file.
* Post new events to your `/hello/world/event` endpoint to add events to your `queue.json` file.
    Use the following curl request to post new events:

    ```curl
    curl --location --request POST '{yourProjectURL}/helloworld/event' \
    --header 'Type: application/json' \
    --header 'Content-Type: application/json' \
    --data-raw '{"id": "6620800b-7029-4a7a-8e80-cdd852fc01c8","number": 16,"haveYouSaidHello": true, "waysToSayHello": ["Hello","Hola","Hallo","Bonjour","Ola"],"helloID": {"language":"English","id": "b2f7494a-5dcf-448c-9585-5301fc0dd231"}}'
    ```

* Refresh the browser to see the newly posted events.

Heroku lets the server sleep after 30 minutes of inactivity.
It must be active to receive requests.
To wake it up, visit the `/health` endpoint
before attempting to receive requests.

### Registering Your App to Fortellis

You must register your app to receive calls from asynchronous APIs through Fortellis.
You can register for as many synchronous and asynchronous APIs as you want.
You must register the app to get your credentials.
Fortellis sends a token with your credentials to your webhook
when it sends an event to your endpoint.
Verify that the credentials in the token are yours to authorize the request to your endpoint.

#### Sign In and Choose the Organization

1. Sign in to [Fortellis]($[devNetworkUrl]).
1. Hover over your name in the upper-right corner.  
1. Click **Change**.  
    ![Change Organizations]($[docsUrl]/static/images/signInToFortellis.PNG)
1. Click the organization.
1. Click **OK**.  
    ![Click Okay For Organization]($[docsUrl]/static/images/clickOkayForOrganization.PNG)

#### Create the App

1. Click **New App**.  
    ![Change Organizations]($[docsUrl]/static/images/pictureOfApps.PNG)
1. Enter the app details:
    * **App Name**
    * **App Description**
    * **Website**
    * **Callback URL**  
    **Note:** See [Authorization Flows](/docs/tutorials/solution-integration/auth/) to determine if you need a callback URL.  
        ![API Integrations]($[docsUrl]/static/images/registeringApps.PNG)
1. Select the asynchronous API that you want your app to use.
    1. Click **Async API** for the **API Type** to show only the asynchronous APIs.
    1. Click the **Category** of the API if you know it.
    1. Click the **Publisher** of the API if you know it.
    1. Search for the asynchronous API using the search box if you know the name.
    1. Click the asynchronous API that you want to use.  
        ![asynchronous APIs]($[docsUrl]/static/images/asynchronousAPI.PNG)  
    **Note:** Select your asynchronous API
    if you created one to see the webhook receiving calls from your asynchronous API at the end of the tutorial.
1. Click **Webhook** to enter your webhook where you receive events.  
    **Note:** You may use different webhooks for different asynchronous APIs.
1. Enter the URL for your Heroku app with your `/helloworld` endpoint and click **Save**.  
    **Note:** Fortellis appends the `/event` endpoint to the end of the URL and sends the event to that endpoint.
1. Click **Register** at the bottom of the page to complete your registration.  
    ![Solution Register]($[docsUrl]/static/images/solutionRegister.PNG)

You can now click the app to get to the apps page and get your **API Key** and **API Secret**.
You can find your credentials in the **Credentials** section of the page.
Use the [Client Credentials Flow for Authorization](/docs/tutorials/solution-integration/client-credentials-flow) to get a token.

### Verifying the Access Token

Fortellis sends a token with your credentials to your webhook
when it sends an event to your endpoint.
Verify that the credentials in the token are yours to authorize the request to your endpoint.
We are going to create this locally first and then update Heroku.

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

1. Import the file in your `HelloWorld.java` file.  
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

1. Include the JWT verifier in the `HelloWorld.java` file.  
    After getting the authorization header, include the following to add the JWT verifier:  

    ```java
    try{
        AccessTokenVerifier jwtVerifier = JwtVerifiers.accessTokenVerifierBuilder()
            .setIssuer("$[authServerUrl]")
            .setAudience("api_providers")
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
            .setConnectionTimeout(Duration.ofSeconds(1))
            .build();
        Jwt jwt = jwtVerifier.decode(authorizationHeader.replace("Bearer", ""));
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
        newAsynchronousAPIPost(data);

    }catch(Exception e){
        System.out.println("You had a problem with the token.");
    }
    ```

    **Note:** Fortellis also sends your **Client ID** in the `sub` of the token. You should verify this in your token to ensure the token comes from Fortellis.

1. Add the following below `Jwt jwt = jwtVerifier.decode(authorizationHeader.replace("Bearer", ""));`:  

    ```java
    System.out.println("This is the authentication decoded: " + jwt);
    System.out.println("This is the subject decode: " + jwt.getClaims().get("sub"));
    if(jwt.getClaims().get("sub").equals("{yourAPIKey}")){
        System.out.println("The strings are equal.");
    }else{
        throw new ServletException("You must have the same subject in your token");
    }
    ```

1. Replace `{yourAPIKey}` with the **API Key** from your app.

You can now call the endpoint,
but in the request, you must include a token
you created with your **API Key**.
You can start the server to confirm the results.

To get a token, use the following:

```curl
curl -X POST --url $[authServerUrl]/v1/token \
--header 'accept: application/json' \
--header 'authorization: Basic {Base64EncodedAPIKey:APISecret}' \
--header 'Cache-Control: no-cache' \
--data 'grant_type=client_credentials&scope=anonymous'
```

To add events to the local queue, use the following with your token.

```curl
curl --location --request POST 'http://localhost:9999/helloworld/event' --header 'Type: application/json' --header 'Content-Type: application/json' --header 'Authorization: Bearer {yourToken}' --data-raw '{
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

You must now get an authorization token with your credentials and send those to your endpoint.
For more information on getting your token, see the [Client Credentials Flow for Authorization](/docs/tutorials/solution-integration/client-credentials-flow).

### Updating Heroku

Follow the process for updating GitHub and then redeploy to Heroku.  

1. Stop the server if it is running.  
1. Add the changes to your repository.  
    On the command line, type the following:  

    ```bash
    git add *
    ```

1. Commit your changes.  
    On the command line, type the following to leave your commit message:  

    ```bash
    git commit -m "Your Commit Message."
    ```

1. Push your changes to your repository.  
    On the command line, type the following:  

    ```bash
    git push
    ```

1. Deploy the project.  
    In your Heroku project, click **Deploy Branch** to deploy the new changes.  

You must now include your token in requests to your Heroku project URL.
You can call Heroku with the following request and then refresh the browser to see the new event:

```curl
curl --location --request POST '{yourHerokuURL}/helloworld/event' \
--header 'Type: application/json' \
--header 'Authorization: Bearer {yourAuthorizationToken}' \
--header 'Content-Type: application/json' \
--data-raw '{"id": "6620800b-7029-4a7a-8e80-cdd852fc01c8","number": 16,"haveYouSaidHello": true, "waysToSayHello": ["Hello","Hola","Hallo","Bonjour","Ola"],"helloID": {"language":"English","id": "b2f7494a-5dcf-448c-9585-5301fc0dd231"}}'
```

### Integrating Your App with Your Asynchronous API Producer

> Currently, a Fortellis Administrator must create the subscription to your app and activate it
> before an asynchronous API sends events to your app webhook.
> Contact [support@fortellis.io](mailto:support@fortellis.io) to ask an administrator to create your subscription and activate it.

If you followed the previous tutorial on [Publishing Messages and Verifying Results](/docs/tutorials/event-relay/publishing-messages-and-verifying-results),
you can now do the following:

1. Start your asynchronous API.
1. Update the `payload.json` file to send a new event to Fortellis.
1. Receive the updates through Fortellis at your webhook in your Heroku project.
1. Refresh the page to see the updated `queue.json` file.

## Next Steps

In this tutorial, you should have learned how to do the following:

* Create an endpoint to show the contents of the `./queue.json` file.
* Register the app to Fortellis.
* Create an app that can consume events from Fortellis.
* Verify the token that Fortellis sends with the event.

You can find the completed project on GitHub at [Java-Public-Webhook-Example](https://github.com/Fortellis/Java-Public-Webhook-Example).

If you download the example repository, do the following:

1. Follow the steps to [Creating Your Heroku Project](#creating-your-heroku-project).  
1. Follow the steps to [Registering Your App on Fortellis](#registering-your-app-to-fortellis).  
1. Change the `sub` value in the `HelloWorld.java` file to your **API Key** for your registered app.  
