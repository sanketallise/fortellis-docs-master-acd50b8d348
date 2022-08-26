---
title: Creating and Managing Organizations
author: Kelly Rich
last updated: 12/29/2021
---

After creating a Fortellis Organization, you are the *Organization owner* and you can perform the following tasks:

* Invite people to your team
* Manage the members of the Organization
* Edit the Organization account details
* Delete the Organization

> **Note:** A user must have a Developer Network account with an email address that matches the domain of the Organization's email address before they can join the Organization.

If a user does not have a Fortellis account when they try to join an Organization, Fortellis steps the user through the account-creation process so they can join the Organization.

## Creating an Organization

To create a new Fortellis Organization:

1. Sign in to [Developer Network]($[devNetworkUrl]).
1. Hover over your name, then click **Organization Management**, then click **Register a New Organization**.
1. Complete the **Enter Organization Information** form, then click the **Create Organization** button at the bottom of the page.

A notification gets sent to your account's email address when your Organization gets provisioned, and you can then access the new Organization.

## Managing Organizations

As an Organization owner, you can manage the Organization's membership:

1. Sign in to the [Developer Network]($[devNetworkUrl]) and activate the Organization you want to manage.  
    1. Hover over your name in the upper-right corner, then click **Organization Management** from the Fortellis menu.
    1. Select the Organization you want to manage by clicking the menu button to the right of the Organization's name and choose **Switch Organization**.  
1. Hover over your name, again, and click **Organization Management**.
    The Organization Management page displays, which lists all of the Organizations of which you are a member.  
    The Organization Details pane displays for the currently-active Organization.
    The **Manage Members** button displays in the lower-right corner of the Details page if you are an Owner of the active Organization.
1. Click the **Manage Members** button to view **Members** page of the Organization.

From here, you can manage the members of the Organization, as described below.

### Inviting New Members to Your Team

To add a new member to your Organization:

1. From the **Manage Members** page, click **Invite New Member**.  
1. Enter the new member's email address.  
    **Note:** The new email address must share the email domain of the Organization.  
1. Click **Send Invitation**.  
    Fortellis responds with one of two messages:  
    * If your email is sent, Fortellis confirms with an email with the name of the person you invited.
    * If Fortellis is unable to send the invite, Fortellis notifies you of the cause of the error.

Once a user joins an Organization, they can see the following resources associated with the org:

* Apps
* APIs
* API Specs
* Any other artifacts associated with the API or app

### Associating Individuals with an Organization

* Contact [developers@fortellis.io](mailto:developers@fortellis.io) to request user access to an Organization.  
    **Note:** You can also manage team members via the Organization's menu.  
    ![Organization Menu Button]($[docsUrl]/static/images/managingOrganizations.PNG)

### Managing Requests to Join Your Organization

As an Organization owner, you must field the requests from your co-workers to join the Organization.

Sign in to Developer Network and activate the Organization with open requests.

1. Click **Requests**.
1. Click one of the following to manage each request:  
    * **Approve** — Confirms the request
    * **Deny** — Rejects the request

### Assigning Permissions to Users

You can assign permissions to users to restrict the access that they have within your Organization's account.

1. Sign in to Developer Network and activate the Organization you want to manage.
1. Click **Manage Members** on the Organization's Details page.
1. Click the arrow under **Role** next to the user that you want to change.
1. Check the permissions that you want the user to have, or uncheck the role to remove it from the user.  
    Fortellis saves the role of the user when you click it.

#### Roles

You can assign the roles with the associated permissions as outlined in the following sections. Users get notified if they try to access a specific feature when they don't have the needed permissions.

**Note:** Currently, Fortellis only enforces the **Developer** role.

##### Developer

The *Developer* role can create *APIs*, *Apps*, and *Events*.

With these, Developers can perform the following actions:

* Review
* Update
* Delete
* Create a new version

### Editing the Organization Contact

If you need to update the contact information for your Organization, do the following:

1. Sign in to [Developer Network]($[devNetworkUrl]) and activate the Organization you want to manage.
1. Select **Organization Management** to go to the **Organization Management** page.  
    Note that the currently-active Organization is expanded by default.
1. If it's not already expanded, click the Organization you want to manage.
1. In the Organization Details box, click **Edit** next to the Organization's **Primary Contact**.
1. Select the member of the Organization who you want to be the **Primary Contact**.
1. Click **Save** to update the contact information.

You may have to refresh the page to see the information update.

### Deleting a Member of the Organization

1. From the **Manage Members** page, click the **Delete** button next to the user you want to remove from the team.  
1. Click **Delete Member** to confirm the deletion.  
