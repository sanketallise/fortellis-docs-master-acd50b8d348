---
title: API Usage Report
author: Nathaniel Sandberg
last updated: 12/31/2020
---

API Developers can now see a usage report with information on the month-to-date usage of their APIs.
With the API usage report, API Developers can see the following:

* The Total Traffic for the API  
    **Note:** Please do not use the information in the usage report for billing because it does not provide information for an entire billing cycle.
* The Payload Size for each API
* The Total Payload Size for each API by app

**Note:** API Developers can only see usage reports for APIs associated with organizations, not with their personal accounts.

## Viewing the Usage Report

1. Sign in to your Fortellis account.
1. Hover over your name in the upper-right corner.
1. Click **Change**.
1. Select the organization that you would like to see the reporting information for.
1. Click **OK**.
1. To the right of **APIs**, click **Usage Report**.
1. Click **Last Month's Usage Report** to get a copy of the previous month's usage as an Excel file.  
    **Note:** If you do not get a usage report for the previous month,
    no one used your API in the previous month.  
    ![API Usage Report]($[docsUrl]/static/images/usageReportForAPIProviders.PNG)

You can see the following fields in the usage report CSV:

* API Name.
* Solution that shows the app that the user has activated.
* Solution Org that shows the organization that created the app.
* Subscription ID that shows the `Subscription-Id` that the App Developer sends with every call to the API from that user.
* The Organization that shows the name of the Organization that activated the app and calls the API.
* Total traffic shows the total calls from that app user to the API.
* Payload Size that shows the total size of the information that the API sent back to the app users.
