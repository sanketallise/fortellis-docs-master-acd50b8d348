---
title: Renewing App Credentials
author: Nathaniel Sandberg
last updated: 7/21/2020
---

App Developers can regenerate the **API Secret** associated with an app if they fear their app OAuth credentials have been compromised. As a precaution, App Developers can renew their API Secret at intervals to guard against malicious users from impersonating a valid App Developer.

You can generate a new API Secret for an app from its App Details page. Note that you can only regenerate the secret, you cannot renew the API Key value.

## Generating a New API Secret

Use this flow to generate a new API Secret:

1. Sign in to [Fortellis]($[devNetworkUrl]).
1. Enable the Organization in which the app is registered:  
    1. Hover over your name in the upper-right corner and click **Change**.
    1. Click the Organization containing the app, then click **OK**.
1. Hover over your name in the upper right corner and click **Developer Account**.
1. In the Apps section, click **View All** then navigate to the app you want to manage.
1. Click the app listing to open its App Details page.
1. In the Credentials section, click **Generate New Secret**.

Depending on your authorization workflows, you may want to manage the transition from one set of keys to another during maintenance windows or low usage times.

![The API Credentials for an app]($[docsUrl]/static/images/api-credentials.jpg)
