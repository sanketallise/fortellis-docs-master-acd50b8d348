---
title: Fortellis Key Concepts
author: Kelly Rich
last updated: 3/23/2022
---

The terms and concepts introduced below are the building blocks for many Fortellis APIs and apps.

## The Fortellis Platform

Fortellis is a service platform for the vehicle dealership vertical. The platform provides services to the following Fortellis-membership audiences:

* App Buyers
* App Developers
* API Developers

The platform gives dealership service providers (App Buyers) a trusted way to integrate new features into the services they offer, no matter which data provider a dealer has configured on their back-end.

Fortellis also provides a certification service for API and App Developers, driving the standardization of back-end software in the auto dealership industry.

## The Players

The important players in the Fortellis ecosystem include the ones described here.

### App Buyers

App Buyers are the group of service providers and independent software vendors (ISVs) who activate and manage the connectivity to apps listed in [Fortellis Marketplace](https://marketplace.fortellis.io). App Buyers integrate sets of Fortellis apps to create the automotive-industry solutions they provide to their customers.

### App Developers

App Developers create apps that make use of Fortellis APIs to do activity on the Fortellis platform. App Developers market the apps they create on Fortellis Marketplace.

### API Developers

API Developers implement services that interact with the DMS and they provide an API Specification that defines the features and functionality of the service they provide.

App Developers make use of Fortellis APIs to interact with the data in the DMS.

### DMS Service Providers

DMS Service Providers, also referred to as "service providers," offer solutions to the vehicle dealership vertical. Service providers and ISV's integrate Fortellis apps into the services and solutions that end users interact with at site locations, such as a DMS user at dealership interacting their terminals.

### Independent Software Vendors

Third-party service providers, or ISVs, who want to integrate their applications with a CDK service or application.

### DMS Users

DMS Users access and update the data in DMS systems using apps displayed on terminals. There are many types of DMS Users, including service advisors, service technicians, those in parts departments and in scheduling and administrative departments.

### The Dealer / Dealership

A dealer sells new or used vehicles, vehicle parts, or provides vehicle services and repairs. While The dealer can be a single person, this term often represents the organization that purchases and maintains dealership solutions.

### The Customer

The customer is the user of a dealer's business, such as an individual or a business (organization) who has purchased a vehicle, has shown an intent to purchase a vehicle, has bought parts or accessories, has procured aftermarket F&I products or services, or has had their vehicle serviced or repaired at the dealership and their customer information exists in the Dealer Management System (DMS).

## High-level Fortellis Concepts

These concepts are commonplace in the Fortellis ecosystem.

### Dealership Management System

Commonly referred to as the DMS, the Dealership Management System is the underlying database and platform running a dealership's software system. See *DMS* below for more.

### The Marketplace

The [Fortellis Marketplace](https://marketplace.fortellis.io) is where App Developers and App Buyers meet.

App Developers list and sell their apps in Marketplace, and App Buyers review and purchase apps for the automotive industry workflows they create.

### The Developer Network

The [Fortellis Developer Network]($[devNetworkUrl]) is a web interface that Fortellis developers use to create, manage, and list their Fortellis APIs and Apps.

### Organization

A Developer Network feature where developers can create teams to collaborate with other Organization members on private APIs and apps before making them public on the Fortellis platform.

See [Working with Organizations](/docs/general/overview/organizations) for more information on Organizations in Fortellis.

### Private Pricing Plans

App Developers can request private pricing plans from API Developers. For more information, see [Requesting Special Pricing](/docs/general/app-developers/designing-apps/#requesting-special-pricing).

### Deals

A deal in the Fortellis system is a record of an informal agreement created for a prospective vehicle sale or lease, or a part sale and attached to a potential buyer.

## Industry Concepts

These concepts are common to those working in the vehicle dealership industry and are not specific to Fortellis.

### DMS

Dealer Management System (DMS) is an ERP that enables automobile dealerships to integrate their key business processes, including lead management, CRM, marketing, sales, service, parts, vehicle inventory, accounting, and payroll. The DMS also enables integration with specific-purpose layered applications and allows for collecting, storing, managing, and sharing data between various integrated applications.

### Vehicles

A vehicle is in the context of an automobile dealership’s business is any of the following:

* Any form of ground transport, motorized, or electronic
* Used for private, public, passenger, commercial, or recreational purpose
* New or used
* Meant to carry passengers, goods, or both
* Available in stock for sale, in the manufacturer’s inventory, in transit, or on order

### Inventory Vehicle

New or used vehicle, which may be in stock or may not be currently present in the inventory; possible on-order, in transit, and so on.

### Finance and Insurance (F&I)

The finance and insurance (F&I) department in an automotive dealership is where the customers who intend to buy a vehicle are offered options to buy additional products and services. Example, GAP insurance, extended warranties, etc.

### Inventory versus Service File Type

The DMS has two separate files used to maintain vehicle data for the dealership. One file is the Inventory file (CARINV), which contains the dealership-owned vehicle inventory. The other file is the Service file (VEHICLES), which contains customer-owned vehicles. The Vehicle Type file field will define the file.

### CCPA

California Consumer Privacy Act is a consumer privacy law being enacted in the state of California, which requires businesses to provide some or all of the following services at the consumer’s request:

* Allow the consumer to “opt out” of having their data sold
* Provide a list of personal information (PI) collected about the consumer
* Explain why the PI data was collected
* Upon request, remove the consumer’s PI data from the dealership’s database

### VMS

Vehicle Management System is a business solution to manage inventory in a dealership. The inventory could consist of a used lot, new lot, or both, and the VMS can show what vehicles and how many of them are available or not available on the floor, and their status. The details of vehicles in the inventory, including the vehicles ordered, vehicles in transit, and vehicles’ prices, dealership commissions, costs, and invoice (bill from the vendor) will be mapped to sales and accounting systems in a dealership and will be identified by VIN, vehicle make, and stock number, and other details.

## Vehicle Servicing Concepts

The terms below relate to vehicle servicing and scheduling.

### Service Appointment

A service appointment is a booking of a vehicle into the service department for work that needs to be carried out on the vehicle. This could be a service, repair, body repair, government test, etc.

### Repair Order

A repair order is a working record of the work carried out by the service department on a vehicle. This could be any set of services, repairs, body repairs, government tests, etc.

This is the type of work done by a technician on a vehicle that has come into the dealership for service or repair. The labor rates will be based on the labor type of a job involving a repair, replacement of a part, or a simple service or replenishment of material such as break oil/engine oil. Each labor type will have a type code, which will be included in a service RO for a vehicle.

### Labor Type

This is the type of work done by a technician on a vehicle that has come into the dealership for service or repair.

Labor rates are based on the labor type of a job involving a repair, replacement of a part, or a simple service or replenishment of material such as break oil/engine oil. Each labor type has a type code, which is included in a service repair order for a vehicle.

### OpCode

OpCode is short for Operation Code. OpCodes are required to add various labor jobs done for a servicing vehicle as part of a vehicle repair order. This allows for defaulting to rate and book time for each service job line in a repair order.

## Developer Concepts

These are some common terms of the developer community.

### Fortellis Apps

Fortellis provides App Developers with the following:

* A common authorization method to use across all APIs
* A known method for connecting DMS service providers to API Developers

An app is the end product that the DMS User sees and interacts with. Apps are combined by Dealership service providers to create solutions. In short, apps:

* Provide the user experience
* Grant access to the DMS via API requests
* Transform the specific data used by the app for the user
* Update the DMS as needed

Apps come in many forms, including any of the following:

* A web app
* A mobile app
* A data service
* An integration with an existing application

### Fortellis APIs

The APIs provide the data that apps can work with to provide information to end users.

On Fortellis, API Developers provide services that interface with the Fortellis DMS, and they provide service interfaces that app developers can use to access the services they provide.

The features and functionality of the service are detailed in an API Spec that the API Developer creates to describe their service implementation.

API Developers offer their production-ready APIs to App Developers via the API Directory.

Fortellis provides the following to third-party API Developers:

* An integrated marketplace for their Fortellis APIs
* A proxy that routes calls from authorized app users
* An common authorization method that provides secure access to APIs

### Fortellis API Gateway

The API Gateway offers:

* A gateway that uses an industry-standard authorization process
* A proxy through which calls to Fortellis APIs are routed to the correct API endpoints

With OAuth 2.0 support for authorization, App Developers can market apps that work in trusted environment. OAuth 2.0 provides an industry-standard model for secure access to third-party data.
