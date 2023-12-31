﻿## Use Case Overview

Okta  Access Requests automates the process of requesting access to applications and resources. Expanding on Okta's existing self-service offerings, Access Requests delivers a simplified and frictionless approach that automatically routes user requests to one or more approvers for action.

This challenge provides an introduction to Okta's Identity Governance and covers Access Request workflows. When we talk about workflows in this challenge we mean the access request flow and not Okta Workflows.

In this challenge, you will get to build a basic Access Request and approval workflow. This will provide you a good foundation on the Okta Access Request concepts from which you can build on.

## The Challenge

Users in the "Staff" user group have access to **Okta Access Request** app in their end user dashboard. Any user in the Staff user group can request access to the application "SampleAppTwo". This request is assigned to the user's manager for approval. You get to build the logic for the approval flow. When the manager approves the request, the application gets provisioned to the user.

## Before you begin

Check that you have the following configurations in your tenant.

## Group Membership

In the administration console, go to **Directory > Groups** and select "Staff" group. Under the people tab, there should be the following group members:
1.  barney.gumble#@mailinator.com
2.  bart.simpson#@mailinator.com
3.  dean.simpson#@mailinator.com
4.  homer.simpson#@mailinator.com
    
If these users are not members of this group, then add them now by clicking the "Assign people" button.

## User Profile

Check the profile of the group members you are going to use in this challenge. This is done by selecting the group member and then selecting the "Profile" tab. The user should have user "chief.wiggum#@mailinator.com" as the value for "ManagerId".

If the user does not have the correct value set for "ManagerId", then set it now by clicking the "Edit" link at the top of the page.

## Application

In the admin console, go to **Applications** and click on the **Okta Access Requests** app. Click on the **Assignments** tab, select Groups on the left column and make sure the **Everyone** group is assigned. If not assign the "Everyone" group to the app.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image1.png?raw=true)

Go to the **Push Groups** tab and select the **Push Groups** button and Find the groups by name "**Staff**". Click Save. You should see Push Status of "Pushing" followed by "Active"

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image2.png?raw=true)

This last step will push the "Staff" group to Access Requests.

**Let’s get started with setting up Access Requests!**

## Setup Access Requests and Build the Flow

In the admin console, go to **Identity Governance** and click on **Access Requests**, this will open up a new tab, the **Access Request UI**.

### Configure an Access Request Team.

Teams are an Access Requests grouping mechanism. Use teams to organize users into logical groups within Access Requests. An Access Request Workflow must be assigned to a Team, and Teams can be used to scope who can run a workflow or participate in it. 

**Note:** An Access Request Workflow is specific to Identity Governance and is not an Okta Workflow.

In the Access Request UI, click on Teams and you will see a default team called IT, we can use this. Click on the **Team** menu option on the left and add yourself as a member. Click on the **Add member** button on the top right and enter your username and then click ** Add Users**. Repeat this step and add **Chief Wiggum** as a team member.

When completed, the team members should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image3.png?raw=true)

### Complete Okta Integration

In the Access Request UI, click on **Settings** menu option on the left and select the **Integrations** tab. Click on the **Edit Connection** button on the **Okta** tile. 

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image4.png?raw=true)

Make sure the IT team is selected and all the actions are allowed. Click on the **Update Connection** button.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image5.png?raw=true)

Next let's make sure the applications and groups are synced across to the Access Requests console. Resources are synced directly from your integrations. Currently, Access Requests can sync resources from Okta, Jira, and Service Now.
Click on the **Resources** tab to the top and select **Applications**. Make sure Sample App Two is present and click on **Okta Groups** and make sure the **Staff** group is present. If you made any changes, click on **Update now**.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image6.png?raw=true)

Let's make sure the IT team has full access to the applications and groups. 
Click on **Applications** on the left and then click on **Manage Access** button and make sure the IT team is enabled if not move the radio button to enable and click **Save**.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image7.png?raw=true)
 
Next, click on **Okta Groups** on the left and then click on **Manage Access** button and make sure the IT team is enabled if not move the radio button to enable and click **Save**.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image8.png?raw=true)
 
Now that the apps and groups are present, let's use them in our flow. Configuration lists are customized collections of resources or admin-defined values. They determine which applications or groups that a team can use in a Request Type. Use them in Request Types to specify options available to your end users or control how automated actions work within a Request Type.

Click on the **Configuration lists** tab at the top and the click on **Create new list**. Give the list a meaningful name like App Sublist. Then under **Teams** select the IT team, a **List type** of Resource list and a **Resource type** of Applications. 
Then click the **Add item** button on the bottom left, and select Sample App Two. Finally, click the **Create list** button on the bottom right.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image9.png?raw=true)

### Create and Access Request Type

Request Types are collections of tasks used to define how to process a request. Each Request Type is made of one or more tasks that are routed to approvers for review.  Access Requests  teams create and own Request Types.

On the **Access Request** UI on the left menu, select Access Requests and click on **Create request type**. Provide a name for the Request Type and choose an icon of your choice. Under **Team** select the **IT** team. 
Audiences control which users can submit a request with a specific Request Type. Under Audience, use the *Select an Okta group...* menu option to select the **Staff** group. Then click **Continue**.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image10.png?raw=true)

This is where you can create your own business logic for the approval flow. You can have multiple levels of approvals etc., but let's start with something simple.

Next to Questions, click **Add to request type**. Type a question on the right most column in the box labeled **Text**. I am going to go with Business Justification and then make it a required field. The **Type** is text and it is assigned to the **Requestor**.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image11.png?raw=true)

You can now choose to add another Question by clicking on the **Question** button on the bottom left if required. In this case, I'm going to add a question on duration of access.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image12.png?raw=true)

Then add an Approval Task by clicking on the **Task** button at the bottom. Give the task a meaningful name like Manager Approval. Leave the default as a required task. Set the task **Type** as Approval task. 

Assignees manage a request after it’s submitted and are always members of the Access Requests Team that owns the Request Type. Set the **Assigned to** to the Requester's manager. Leave the **Due date** as the default of No due date.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image13.png?raw=true)

Now let's add an action to assign the app after approval. Click on the **Action** button at the bottom and select *Add user to a group*. Give the action a meaningful name like Assign Sample App Two. Leave the default of required task to true. Turn the **Run automatically** toggle to on. Set the **Email address** to *Requester email*. Under **Select the group**, select the *SampleAppTwo* group.

Next, select the **Logic** tab on the top left. Change the value of **Always show this task** to **Only show this task if**. Then set the **Field or task** to *Manager Approval*. Leave the default approval status to *is approved*.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image14.png?raw=true)

After you have added the Action, click on the Save **draft** button on the top right corner.

The Access Request workflow should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image15.png?raw=true)

Next click on the pencil button next to the name of this flow on the top left, in this case pencil button next **Sample App 2**.

Enable the radio button next to **Mark as done automatically** and click on Continue.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image16.png?raw=true)

Now click on the **Publish** button on the top right corner.

On the Access Request UI, go to **Access Requests > All**

You should see the new access request workflow you configured.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image17.png?raw=true)

**That completes the Access Request workflow setup.**

## Test the Access Request and Approval

Let’s test out the access request workflow that you configured in the previous section. If you are using chrome, choose another profile and login to your Okta end user dashboard using one of the users. I am logging in as Homer.

### Create an Access Request

From my end user dashboard, click on the **Okta Access Requests** tile.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image18.png?raw=true)

Click on the **Request access** button on the **Sample app 2** Access Request workflow you created earlier.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image19.png?raw=true)

Answer the questions and submit the request. You should see that your request has been submitted and assigned to your manager for approval.

### Approve the Access Request

Now login to the Okta end user dashboard as **Chief Wiggum** in another browser profile to complete the approval process. Click on the **Okta Access Requests** tile and you should see there is a request that needs action from you.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image20.png?raw=true)

Click on the request and you should see the request with justification and other details provided by the requester. On the right most corner, click on **Tasks** and click on the radio button next to **Manager approval** and select Approve.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image21.png?raw=true)

Next click on the **Activity** tab and you should then see that the app has been assigned to the user automatically and the request has been marked completed.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image22.png?raw=true)

You can now login as the requester and see the app “Sample App 2” available for you in the Okta end user dashboard.

### Request and Approval Options

Both the requestor and approvers receive emails regarding the request. You could check this out using [mailinator](https://www.mailinator.com/). The approver can just click on the email and approve the request. Okta Access Request and approval can also be performed using collaboration tools such as Microsoft Teams and Slack.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/012/image23.png?raw=true)

*That completes this challenge.*
