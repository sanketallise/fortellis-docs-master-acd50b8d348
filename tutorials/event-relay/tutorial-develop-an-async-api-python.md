---
title: Developing an Asynchronous API in Python
author: Nathaniel Sandberg
last updated: 1/20/2022
---

$[eventRelayIntroductionBoilerPlate]

## What You Will Learn in This Tutorial

$[eventRelayBoilerPlateWhatYouWillLearn]

## Things to Remember in This Tutorial

You should remember the following:

* Use **python app.py** to start the script.
* Use **Ctrl**+**C** to stop the script.
* Install the following before starting the tutorial:
    * Python
    * A command-line interface
    * A text editor

For this tutorial, we also use [https://webhook.site](https://webhook.site) so you don't have to set up a web tunnel,
but you can set up the receiving endpoint however you would like to.

You can follow the instructions at the end to download the completed project and get a working copy from GitHub: <https://github.com/Fortellis/python-async-api-example>.

## Creating the Python Project

1. From the command line, create a new directory for the project.  
    Type the following:

    ```curl
    mkdir {your directory name}
    ```

1. Change to the directory you just created.  
    Type the following:

    ```curl
    cd {your directory name}
    ```

1. Create the Python environment.  
    On the command line, type the following:  

    ```curl
    pip3 install virtualenv
    ```

1. Update the Python interpreter you are using.  
    On the command line, type the following:  

    ```curl
    virtualenv -p python3.9 venv
    ```

1. Activate the environment.  
    On Windows, type the following on the command line:  

    ```curl
    source venv/Scripts/activate
    ```

    **Or**  
    On Mac, type the following on the command line:  

    ```curl
    source venv/bin/activate
    ```

You can now see `(venv)` indicating that your environment is running.
To stop the environment, type **deactivate** at any time.

## Creating the Payload and Monitoring for Changes

1. Create the `payload.json` file to monitor.  
    On the command line, type the following:

    ```curl
    touch payload.json
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

1. Create the `app.py` file that monitors `payload.json`.  
    On the command line, type the following:  

    ```curl
    touch app.py
    ```

1. Install watchdog to track changes to files.  
    On the command line, type the following:  

    ```curl
    pip3 install watchdog
    ```

1. Include the following dependencies:  
    * `time`  
    * `sys`  
    * `watchdog`  
    * `json`  
    In your `app.py` file, type the following with your imports:  

    ```python
    import time, sys
    from watchdog.observers import Observer
    from watchdog.events import FileSystemEventHandler
    import json
    ```

1. Add the class to print the changes.  
    In your `app.py` file, type the following:  

    ```python
    class EventHandler(FileSystemEventHandler):
        print("Handler Called")
        def on_modified(self, event):
            print(f'event type: {event.event_type}  path : {event.src_path}')
            with open('payload.json', 'r') as f:
                lines = f.readlines()
                print(json.dumps(lines, indent=4))
    ```

1. Monitor for file change every second.  
    At the bottom of the `app.py` file, type the following:  

    ```python
    src_path = sys.argv[1] if len(sys.argv) > 1 else '.'

    event_handler=EventHandler()
    observer = Observer()
    observer.schedule(event_handler, path=src_path, recursive=True)
    print("Monitoring started")
    observer.start()
    try:
        while(True):
            print('timer', flush=True)
            time.sleep(10)
    except KeyboardInterrupt:
        observer.stop()
    observer.join()
    ```

**Note:** Make sure to turn the flush on in the loop or Python aggressively buffers the output.

You can now start the script and see the file log to the console
when you change it.
On the command line, type the following to start the script:  

```bash
python app.py
```

## Sending the File Contents

Now, we send the payload to our webhook endpoint.

1. Stop the script.  
    On the command line, type the following:  
    **Ctrl**+**C**

1. Install the requests dependency.  
    On the command line, type the following:  

    ```bash
    pip3 install requests
    ```

1. Import the library to use it to send the payload.  
    At the top of the `app.py` file with the other imports, type the following:  

    ```bash
    import requests
    ```

1. Get your webhook.  
    Go to <https://webhook.site> and copy it.

1. Get the response from the request
    when you post requests to your webhook.  
    Beneath the `print` lines, type the following with your webhook to send the request and get the response when your JSON file changes.  

    ```python
    response = requests.post(
        '{yourWebhookSite}',
        data = json.dumps(lines, indent=4),
        headers= {
            'Accept':'application/json',
            'Cache-Control':'no-cache'
            }
        )
    print(response)
    ```

1. Start the `app.py` script.  
    On the command line, type the following:

    ```bash
    python app.py
    ```

You can now update the `payload.json`,
and the Python script will send the updates to the file to the webhook endpoint.

> Change the `payload.json` file and save it to send the updated information.
> The `on_modified` command produces two events and sends both.
> This is a known issue in watchdog.

View the updates at your <https://webhook.site>.

**Note:** The webhook site may change your URL sometimes.
Double-check the URL at <https://webhook.site>
before you run the Python script.
Replace the UUID in the URL of your `app.py` file if needed with the new UUID
that the <https://webhook.site> assigned to you.

## Next Steps

This tutorial should have shown you how to do the following:

* Create an asynchronous API producer that runs locally.
* Create a simple model to update your Subscribers with new information.
* Have the asynchronous API producer send the payload to an endpoint.
* Use a webhook service to test your asynchronous API producer.

Go to the next tutorial to find out how to publish your asynchronous API producers to Fortellis.

You can find the complete project on GitHub: [Python Asynchronous API Example](https://github.com/Fortellis/python-async-api-example).

To use the project from GitHub, do the following:

1. Clone the repository.  
    On the command line in the folder of your choice, type the following:  

    ```curl
    git clone git@github.com:Fortellis/python-async-api-example.git
    ```

1. Change directories to get to the new folder.  
    On the command line, type the following:  

    ```curl
    cd python-async-api-example
    ```

1. Prepare the Python environment in the folder.  
    Do the following to get the Python environment ready:  
    1. On the command line, type the following to install the virtual env:  

        ```python
        pip3 install virtualenv
        ```

    1. On the command line, type the following to create the virtual environment directory:  

        ```curl
        virtualenv venv
        ```

    1. On the command line, type the following to update the Python interpreter you are using:  

        ```curl
        virtualenv -p python3.9 venv
        ```

    1. On Windows, type the following on the command line to activate the environment:  

        ```bash
        source venv/Scripts/activate
        ```

        **Or**  
        On Mac, type the following on the command line to activate the environment:  

        ```bash
        source venv/bin/activate
        ```

1. Install the dependencies for Python.  
    On the command line, type the following:  

    ```curl
    pip install -r requirements.txt
    ```

1. Update the URL to your webhook.  
    In the `app.py`, replace `{yourWebhookURL}` with the URL you get from <https://webhook.site> to receive requests.
1. Start the script.  
    On the command line, type the following:  

    ```curl
    python app.py
    ```

1. Send the event.  
    Change the contents of the `payload.json`.

You can now see the payload at your webhook site.
