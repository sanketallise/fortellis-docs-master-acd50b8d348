---
title: Viewing Your API Metrics
author: Nathaniel Sandberg
last updated: 5/26/2021
---

You can gauge the health of your APIs using the following Service Level Indicators:  

* **Traffic**  
* **Latency**  
* **Error Rates**  

With API Performance metrics, you can monitor the following:

* API utilization
* Achievement of service-level objective criteria
* Consumer experience

## Viewing the Performance Metrics for All APIs

You can view performance metrics to determine the level of service offered across your APIs and detect the ones with higher than normal error rates and latencies:

* APIs that are green are performing well
* APIs that are orange are experiencing higher than normal errors and latencies
* APIs that are red are returning a high amount of errors and latencies

From this page, you can drill down to the performance of the individual API and see the error codes
that the API is throwing.

Do the following to see the **Performance Metrics** for all your APIs:

1. Sign in to your [Developer Account]($[devNetworkUrl]).
    1. Click **Sign In** at the top of the page.
    1. Enter your **Work Email** and **Password**.
    1. Click **Sign In**.
1. Switch to the organization you want to view the metrics for.
    1. At the top of the page, hover over your name, and then click **Change**.
    1. Click the organization that you want to see the APIs for.
    1. Click **OK** to select that organization.
1. Click **Performance Metrics** to view the **Performance Metrics** for all your APIs.  
1. To drill down to a specific API, click the API.  
    **Note:** On the Performance Metrics page, the charts show the following:  
    * **Environment**
    * **Version**
    * **Time Span**
1. To update the default values, select each one, and then click **Refresh**.  
    You can view the following for the individual API:  
    * **Traffic**  
    * **Latency**  
    * **Error Rates**  

## Viewing Performance Metrics for an Individual API

Do the following to view the specific information without selecting it from the API Dashboard:

1. Sign in to your [Developer Account]($[devNetworkUrl]).
    1. Click **Sign In** at the top of the page.
    1. Enter your **Work Email** and **Password**.
    1. Click **Sign In**.
1. Switch to the organization you want to view the metrics for.
    1. At the top of the page, hover over your name, and then click **Change**.
    1. Click the organization that you want to see the APIs for.
    1. Click **OK** to select that organization.
1. Click **View All** to see a list of all your APIs.  
1. Do one of the following to see the metrics for that API:
    * Click the menu button next to the API,
        and then click **Metrics**.  
    **Or**  
    * Click the API that you want to see **Performance Metrics** for,
        and then click **Performance Metrics** to see the **Performance Metrics** for that individual API.  
        **Note:** On the Performance Metrics page, the charts show the following:  
        * **Environment**
        * **Version**
        * **Time Span**
1. To update the default values, select each one, and then click **Refresh**.  
    You can view the following for the individual API:  
    * **Traffic**  
    * **Latency**  
    * **Error Rates**  

## Metrics Explained

You can now view the following API metrics within Fortellis
when you create an API:  

* **Traffic:** This represents the total number of requests going to the API through Fortellis.  
* **Latency:** This chart has 3 different latencies:  
    * Ninetieth percentile  
    * Ninety-fifth percentile  
    * Ninety-ninth percentile  
* **Error Rates:** This represents the percentage of all transactions that resulted in one of the following status codes from your API:  
    * **2XX:** The percentage of transactions that result in a success  
    * **4XX:** The percentage of transactions that result in client errors  
        > If you are receiving a large number of these errors,
        > check your API Spec to make sure it is valid.  
    * **5XX:** The percentage of transactions that result in server errors  
