---
title: Viewing Your App Metrics
author: Nathaniel Sandberg
last updated: 8/1/2022
---

Fortellis now gives you the tools to monitor your APIs and gain insight on the quality of service your app is receiving from its APIs.
You can now see the following metrics on your APIs:

* API Traffic.
* Latency:
    * P90
    * P95
    * P99
* Client and server error rate.

With these updates, you can monitor your app's APIs and ensure you're meeting service-level objectives for your customers.

## Viewing an App's Metrics

1. Sign in to your [Developer Account]($[devNetworkUrl]).  
1. Choose the organization that the app belongs to.  
    1. Hover over your name in the upper-right corner.  
    1. Click **Change**.  
    1. Select the organization.  
    1. Click **OK** to select the organization.  
1. Click **View All** to see all your apps.  
1. Click the app.  
1. Click **Metrics** to see the metrics for each of the APIs.  

You can see the following metrics for the app:  

* **Traffic**
* **Latency**
* **4XX Error Rate**
* **5XX Error Rate**

You can choose the following options from the app metrics page:  

* **API:** Select the API you want to see the metrics for, or select **All APIs** to see the metrics for all the APIs for that app.
* **Environment:** Select the environment for the API or APIs that you want to see the metrics for.  
    * **Production**
    * **Test**
    * **Dev**
* **Timespan**  
    Choose the timespan for the errors.  
* **Latency**  
    * **P90:** Select this option to see the maximum latency observed by 90% of requests.
    * **P95:** Select this option to see the maximum latency observed by 95% of the requests.
    * **P99:** Select this option to see the maximum latency observed by 99% of the requests.
* **Refresh:** Click **Refresh** to see the most recent updates for your selection of metrics.  

On the **Latency** chart, click **API List** to see the key for the APIs corresponding to the graph.

![API Charts]($[docsUrl]/static/images/latency-traffic-and-errors.PNG)

You can use the metrics on your app's APIs to make sure your customers are having a great experience with your app.
With the app metrics, you can monitor APIs that your app uses and ensure your app meets service-level objectives.
