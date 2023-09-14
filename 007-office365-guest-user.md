## Use Case Overview

One of the many automation use cases that Okta brings to the Microsoft ecosystem is its capability to manage the lifecycle of identities. In this challenge, you will be guided on how to provision Office 365 Guest accounts automatically when a user gets onboarded, leveraging Okta Workflows and the Out of the Box Office 365 Admin Connector. Guest accounts are often used when a fully licensed Office account is not required, like when the user just needs to access a Sharepoint site. So this challenge will also assign the Guest users enough permission to access a Sharepoint site.

**Let’s get started!**

## Solution

The solution involves using a workflows template and then extending it to provide additional access to Sharepoint for the test user. The resulting flow will leverage the Office 365 Admin connector as documented here  [Office 365 Admin Connector](https://help.okta.com/wf/en-us/Content/Topics/Workflows/connector-reference/office365admin/office365admin.htm).

### Step 1 - Add Template

Sign onto the Okta Workflow console and click "Templates" at the top of the page. Then in the search criteria, type in "guest" and enter. Select the template titled "Create Office 365 Guest Accounts".

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image1.png?raw=true")

Then click "Add template" on the next screen and again on ther popup. The template will create a new folder titled "Create Office 365 Guest Accounts" with a single flow titled "Create O365 Guest User".

### Step 2 - Configure Flow

Open the flow titled "Create O365 Guest User". We now need to update the connectors within the flow, and make some changes and additions to the flow configuration.

Go to the first card on the left, which is the User Added to Group event. Click the button titled "Choose Connection" and select the Okta Connector. This should update all the connections for the Okta cards within the flow.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image2.png?raw=true")

Now scroll to the right until you get to the Office 365 Admin Create Guest User card. Click the button titled "Choose Connection" and select the Office 365 Admin Connector. This should update all the connections for the Office 365 Admin cards within the flow.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image3.png?raw=true")

We now need to make some updates to the values for the Create Guest User card. Update the following input values:

1.  Set "Send Invitation Message" to true
2.  Set "Invite Redirect Url" to  [https://xwc7p.sharepoint.com/sites/WICWorkshop](https://xwc7p.sharepoint.com/sites/WICWorkshop)

The Create Guest User card should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image4.png?raw=true")

These settings will result in the new Guest user getting an email to activate their account and once activated, they will be directed to our Sharepoint site.

Next scroll to the right until you get to the end of the flow. We are now going to add some extra cards that will provide the new Guest user with enough access to get to the Sharepoint site. Click the button titled "Add app action" and select the Office 365 Admin Connector. Then scroll down and select the "Search Groups" operation. Next you need to set the options to the following:

1.  Result Set - Set to "First Matching Record"
2.  Search By - Set to "Exact Match"

The card options should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image5.png?raw=true")

Then click Save.

Then on the Inputs and Outputs, deselect everything exception:

1.  Inputs - Display Name
2.  Outputs - Id

The Inputs and Outputs should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image6.png?raw=true")

Then click Save.

For the input for "Display Name", enter the following value "Sharepoint Access". This is the name of the Office security group that is linked to the Sharepoint site. If we place the new Guest users in this group, then they will automatically have access to the Sharepoint site.

The Search Groups card should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image7.png?raw=true")

Next, click the "Add app action" of the right and again select the Office 365 Admin connector. Select the operation titled "Add User to Group".

Now drag the "Invited User Id" return value from the "Create Guest User" card onto the input for "Id or Username" on the "Add User to Group" card.

Then drag the "Id" from the "Search Groups" card onto the "Id" for the "Add User to Group" card.

The last two cards should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image8.png?raw=true")

Now save your flow and using the toggle switch at the top, turn your flow on.

### Step 3 - Testing the Flow

Within your flow, you may have noticed the following card:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image9.png?raw=true")

This means that the flow will only continue execution if the "Add User to Group" event was for the group titled "O365Guest". So to test this flow, we need to add a test user to this group.

Go to the Okta Administration console and go to  **Directory > Groups**  and then select group "O365Guest". Then click the button "Assign people" and add a test user. Then click Done.

Once the user has been assigned to the group, go back to the workflow console and open your flow. Click on the link in the top left of the flow, titled "Flow History". This will open the debug window. You should see one successful flow execution.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image10.png?raw=true")

If the flow generated an error, then use the debug window to work out what went wrong. The workshop instructor will assist if needed.

Now open up a private / incognito browser window and open your email client. If you are using the supplied test users, go to  [https://www.mailinator.com](https://www.mailinator.com/). Then enter your users email to display their inbox. There should be an email from Microsoft, inviting the Guest user.

**Note:** Do not click the "Accept invitation" link in the email as Mailinator replaces the original link. You need to click the "LINKS" tab at the top of the email.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image11.png?raw=true")

Click the "Accept invitation" link under the "LINKS" tab.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image12.png?raw=true")

Once you click the link, the user should then be prompted to click "Send code".

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image13.png?raw=true")

Once you click "Send code", go back to the email inbox for the respective test user. There should be a new email from Microsoft with your code.

**Note:**  On occasions you may need to wait a few minutes for the email to arrive.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image14.png?raw=true")

Now copy that code into the Microsoft login form.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image15.png?raw=true")

Then click "Accept" on the next screen and then Guest user invitation acceptance has been completed. The user should then be redirected to the Sharepoint site  [https://xwc7p.sharepoint.com/sites/WICWorkshop](https://xwc7p.sharepoint.com/sites/WICWorkshop)

Once landed on the Sharepoint site, click the user icon in the top right corner to validate that you are logged in with the respective Guest user account.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/007/image16.png?raw=true")

Thats the completion of this challenge.
