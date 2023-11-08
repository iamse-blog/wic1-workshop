# Access Certification with Workflows

## Use Case Overview

This challenge uses Okta Workflows to perform a number of actions if a user’s access is revoked. Rather than removing access via the Identity Governance functionality, there may be instances where additional actions need to be taken. This is where the Okta’s Workflows platform can be used to customize complex identity governance requirements without code.

In this use case, if the users access is revoked by the reviewer, we are going to get Okta Workflows to remove the user from the respective group, which in-turn removes access to the application. The reason why we might want workflows to do this is to allow for additional processing. The workflow is then going to provision access to a different group, providing the user with a different level of access. The workflow is also going to notify the user of the change in access via email.

**Let’s get started!**

## Solution

To implement the solution to this challenge, follow the steps below.

**Important:**  Before you start, make sure the following users are assigned to group "SampleAppOne":

1.  bart.simpson#@mailinator.com
2.  dean.simpson#@mailinator.com
3.  homer.simpson#@mailinator.com
4.  barney.gumble#@mailinator.com

No users should be assigned to group "SampleAppTwo".

Additionally, two users (barney.gumble and dean.simpson) will have a value of "sales" for attribute "department". This challenge will select users based on this value.

### Step 1 - Create Access Campaign

In the Okta Administration console, go to  **Identity Governance > Access Certifications**. Then click the button in the top right titled "Create Campaign".

Provide a meaningful name for the campaign name, like "Review Sample App One". You can also optionally provide a description. The remainder of the fields can be left as the defaults, as we will be manually starting the campaign in a later step. The "General" configuration should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image1.png?raw=true")

Then click the "Next" button on the bottom right of the screen.

Change the resource "Type" to "Groups" and select the "SampleAppOne" group. The "Resources" configuration should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image2.png?raw=true")

Then click the "Next" button on the bottom right of the screen.

On the next screen, change the default from "All users assigned to the selected resources" to "Specify user scope". Then in the "Scope users" expression box, enter the following:

    user.profile.department == 'sales'

Then to ensure this has been entered correctly, preview a user. Choose either  **barney.gumble#@mailinator.com**  or  **dean.simpson#@mailinator.com**  as these two users have a value of "sales" in their "department" attribute on their profile. The user preview should indicate that the respective user will be included in the campaign.

The "Users" configuration should match the following:
  
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image3.png?raw=true")

Then click the "Next" button on the bottom right of the screen.

Select the Manager as the reviewer. As the "Fallback reviewer", enter the Admin account the you are currently logged in as,  **oktaadmin@atko.email**. This reviewer will not be used, as each group member has a manager assigned. Its mandatory to enter a fallback reviewer. The "Reviewer" configuration should match the following:

  
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image4.png?raw=true")

At this point, you can optionally preview the reviewer, which will be chief.wiggum#@mailinator.com.

Next, select "Notifications" at the bottom of the Reviewer page. Tick "Reviews assigned" so the selected reviewer will be notified via email notifying them that they have a review to complete. There is no need to enable any other notifications.
  
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image5.png?raw=true")

Then click the "Next" button on the bottom right of the screen.

On the Remediation page, do not change the default setting. The setting for "Reviewer revokes access" should be left as "Don't take any action" as we are going to use Okta Workflows to remove the user from the resource.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image6.png?raw=true")

Then click "Schedule Campaign" button on the bottom right of the screen. Once scheduled, you will see the campaign under the Scheduled tab on the main page.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image7.png?raw=true")

Select the campaign and then in the top right corner, you will see an "Actions" drop down list. Click the drop down list and select "Launch". And then on the popup, click "Launch" again.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image8.png?raw=true")

**Before**  we either approve or revoke the users access, we are going to build a workflow to process some custom logic

### Step 2 - Create Revoke Access Workflow

In this step, we are going to create a flow that receives and processes the Access Review event.

#### 1. Create Flow
In the workflow console, create a new flow in either the "Default Folder" or your own custom folder. In the top left of the flow, click on the flow name and give the flow a meaningful name like "Revoke Sample App One Access" and ensure the check box has been enabled for "Save all data that passes through the Flow".
  
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image9.png?raw=true")

Click "Save" to close the popup.

On the left of the flow, click the button titled "Add event" and select the Okta connector. Then select the event "Access Certification Decision Submitted" as the event that starts this flow.

#### 2. Update Event Card
Next we need to update the Access Certification Decision Submitted card to define the attributes under "Debug Data". On the Access Certification Decision Submitted card, scroll down until you find "Debug Context". It will have a child element called "Debug Data". Under "Debug Data" add the following child elements:

1.  campaignItemDecision
2.  campaignItemResourceName
3.  campaignItemPrincipalId

The Debug Context section of the Access Certification Decision Submitted card should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image10.png?raw=true")

**Note:** The alternative to defining the child elements in the event card is to user the Object Get Multiple card within the flow. Either option is fine.

#### 3. Check for Revoke Action
Then click the button to the right of the Okta card titled "Add function". Then select "Branching" in the logic section and select the operation titled "Continue If". This will add the Continue If card to your flow.

Now drag the event value for "campaignItemDecision" onto the input for "value a" on the Continue If card. Then set the value for "comparison" to "equal to" and type in "REVOKE" for "value b". This will cause the flow to exit if the reviewers decision is not to revoke the users access.

The start of your flow should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image11.png?raw=true")

#### 4. Check for Application
Next, we are going to add another "Continue If" card and check that the value for "campaignItemResourceName" is equal to "SampleAppOne".

Repeat the above process and add a second "Continue If" card and drag the event value for "campaignItemResourceName" onto the input for "value a". Then set the value for "comparison" to "equal to" and type in "SampleAppOne" for "value b". This will cause the flow to exit if the user is being revoked for any other resource.

The start of your flow should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image12.png?raw=true")

#### 5. Remove User from Group
Next, we are going to remove the user from the group "SampleAppOne", which in turn will remove the users access to application "Sample App One".

On the left of the flow, click the button titled "Add app action" and select the Okta connector. Then search for the "Search Groups" action. Select "Search Groups" and add the card to your flow.

Update the value for "Result Set" to "First Matching Record". Click Save and select Inputs of "Name" only and Outputs of "ID" only. Click Save. Then type "SampleAppOne" as the input for "Name".

This card should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image13.png?raw=true")

On the left of the flow, click the button titled "Add app action" and select the Okta connector. Then search for the "Remove User from Group" action. Select "Remove User from Group" and add the card to your flow.

Now drag the "ID" output from the previous "Search Groups" card to the input for "Group ID". Then from the event card (first card in the flow), drag the event value for "campaignItemPrincipalId" to the input value for "User ID".

This card should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image14.png?raw=true")

#### 6. Add User to Group

In this use case, we are going to provision the user a different level of access. That can be done in a number of ways. In this instance, we are going to add the user to a different group, "SampleAppTwo". Attached to that group will be a an application, "Sample App Two".

On the left of the flow, click the button titled "Add app action" and select the Okta connector. Then search for the "Search Groups" action. Select "Search Groups" and add the card to your flow.

Update the value for "Result Set" to "First Matching Record". Click Save and select Inputs of "Name" only and Outputs of "ID" only. Click Save. Then type "SampleAppTwo" as the input for "Name".

This card should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image15.png?raw=true")

On the left of the flow, click the button titled "Add app action" and select the Okta connector. Then search for the "Add User to Group" action. Select "Add User to Group" and add the card to your flow.

Now drag the "ID" output from the previous "Search Groups" card to the input for "Group ID". Then from the event card (first card in the flow), drag the event value for "campaignItemPrincipalId" to the input value for "User ID".

This card should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image16.png?raw=true")

#### 7. Create and Send Email
We are now going to notify the respective user of the change in access. To do this, we first need to retrieve the users email address via the Read User card. On the left of the flow, click the "Add app action" button and select the Okta connector. Then search for "Read User". Click on the "Read User" operation and add the card to your flow.

Under the cards Outputs, select Username, First name, Last name and Primary email. You can optionally deselect the default user properties returned. Then click "Save". Then from the event card (first card in the flow), drag the event value for "campaignItemPrincipalId" to the input value for "ID or Login".

The Read User card should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image17.png?raw=true")

Next we are going to compose the message via the Text Compose card. On the left of the flow, click the "Add function" button. Select "Test" on the left of the popup and then select "Compose".

Using the "First name" from the previous Read User card, compose a message to the user. Here is an example:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image18.png?raw=true")

Finally we are going to send an email. On the left of the flow, click the button titled "Add app action" and select the Gmail Connector. The scroll down and select the "Send Email" operation.

Drag the value for "Primary email" returned on the Read User card to the "To" value on the Send Email card. Then drag the output from the Text Compose card to the input for "Body" on the Send Email card.

The last three cards in your flow should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image19.png?raw=true")

### Step 3 - End to End Test

Save your flow and using the toggle switch at the top, turn your flow on.

Next, using a private or incognito browser window, log into your Okta tenant as user chief.wiggum#@mailinator.com. On the Okta dashboard, select the Okta Access Certification Reviews tile/chicklet.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image20.png?raw=true")

Then on the Access Certification screen, select your assigned campaign and revoke the access for one of the users.

Go back to the workflow console and select the "Flow History" for your flow. You should see a flow execution. Ensure all the cards have a green tick indicating that each one executed successfully.

In a new browser tab, go to  [https://www.mailinator.com/](https://www.mailinator.com/)  and enter in the email address of the respective user that had their access revoked. You should see the email produced by your workflow.
  
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image21.png?raw=true")

Finally, open the Okta Administration console and under  **Directory > People**, select the respective user. They should no longer have access the group SampleAppOne and instead have access to group SampleAppTwo.

*That completes this challenge.*
