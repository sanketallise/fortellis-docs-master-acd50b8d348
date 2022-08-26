---
title: Working with Organizations
author: Kelly Rich
last updated: 12/28/2021
---

The Fortellis Developer Network uses **Organizations** to help you create teams to work on APIs and apps. Access to APIs and their artifacts within an Organization is controlled by the Organization's owners, who control the members of the Organization. Everybody who is a member of an Organization has access to all the private APIs and apps created in that Organization.

Organizations let you:

* Collaborate on APIs and apps
* Share resources (such as private specs and implementations)
* Share completed apps within the same Organization
* Maintain business continuity after employee churn

The currently active Organization is listed at the top of the page under your name.

## Changing Organizations

Most Developer Network accounts have memberships in multiple Organizations. Before you work on an API or app, *activate* the Organization in which the API or app is contained.

The currently active Organization is listed under your account name in the Fortellis menu bar. To change the active Organization:

1. Sign in to the [Developer Network]($[devNetworkUrl]), then hover over your name and click the **Change** button.  
    The **Change Organization** dialog displays showing all the Organizations of which your account is a member.
1. Select the radio button next to the Organization you want to activate, then click **OK**.  
    The Developer Network changes your active Organization to the one selected.

Note that you can also change Organizations through the **Organization Management** context-menu item.

## The Developer Account Page

Each Organization has a **Developer Account** page where you can view, create, and manage APIs and apps within that Organization.
In Developer Network, hover over your name and click **Developer Account** from the context menu. The Developer Account page has:

* API Specs — The private API Specs published in the Organization
* Implementations — The `private` backend implementations of published APIs
* Apps — Apps within the Organization that make use of Fortellis APIs

> **Tip:** Create all new APIs and Apps from your Organization's Developer Account page.

## Inviting New Members to your Organization

You can share access to your Organization's private APIs and apps with people outside your Organization, but be aware that members of your Organization enjoy full privileges to all API and App resources available in the Organization.

As a member of an Organization, you can invite another to join the Organization through the **Organization Management** menu item.

When you complete the **Invite New Member** form and click the **Send Invite** button,

* Fortellis sends an invitation to the email address you provided that prompts the individual to join the Organization.  
    The email contains an **Accept Invitation** button which, when clicked, begins the provisioning process.

Before joining an Organization, an individual must have a Fortellis account. If the invitee has an account, they are promoted to **Sign In**. If they don't have an account, they're prompted to create one by completing the following:

1. Enter the following information:
    * Your full name
    * You mobile number
    * Your password
1. Check to accept the Terms and Conditions.
1. Check that you are not a robot, then click **Create Account**.

## Getting the Entity ID of an Organization

Each Organization is assigned a unique ID that you use when creating new APIs and apps.

To get your Organization's Entity ID:

1. Sign in to the [Developer Network]($[devNetworkUrl]).
1. Hover over your name, then click **Organization Management**.  
    The Entity ID is displayed under the Organization's name.  
    ![Entity Id]($[docsUrl]/static/images/entityId.PNG)
