## Use Case Overview

In the first part of this challenge, we are going to create a basic Access Certification Campaign. In this campaign, we are going to get the respective users manager to validate if the user should have access to application "Sample App One". As access to this application is provisioned via membership of group "SampleAppOne", we are going to validate access by validating group membership.

**Let’s get started!**

## Solution

To implement the solution to this challenge, follow the steps below:

### Step 1 - Create Access Campaign

In the Okta Administration console, go to  **Identity Governance > Access Certifications**. Then click the button in the top right titled "Create Campaign".

Provide a meaningful name for the campaign name, like "Review Sample App One". You can also optionally provide a description. The remainder of the fields can be left as the defaults, as we will be manually starting the campaign in a later step. The "General" configuration should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image1.png?raw=true")

Then click the "Next" button on the bottom right of the screen.

Change the resource "Type" to "Groups" and select the "SampleAppOne" group. The "Resources" configuration should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image2.png?raw=true")

Then click the "Next" button on the bottom right of the screen.

On the next screen, leave the default "All users assigned to the selected resources" selected. The "Users" configuration should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image3.png?raw=true")

Then click the "Next" button on the bottom right of the screen.

Select the Manager as the reviewer. As the "Fallback reviewer", enter the Admin account the you are currently logged in as, oktaadmin@atko.email. This reviewer will not be used, as each group member has a manager assigned. Its mandatory to enter a fallback reviewer. The "Reviewer" configuration should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image4.png?raw=true")

Before moving onto the next configuration, we should check that the desired reviewer will be selected. This is done by clicking the link titled "Preview reviewer". Then select one of the users assigned to the group and click "Preview". The preview screen should say that Chief Wiggum (chief.wiggum@mailinator.com) is the reviewer.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image5.png?raw=true")

Close the preview popup.

Next, select "Notifications" at the bottom of the Reviewer page. Tick "Reviews assigned" so the selected reviewer will be notified via email notifying them that they have a review to complete. There is no need to enable any other notifications.
  
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image6.png?raw=true")

Then click the "Next" button on the bottom right of the screen.

On the Remediation page, select "Remove access from user" when the Reviewer revokes access.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image7.png?raw=true")

Then click "Schedule Campaign" button on the bottom right of the screen. Once scheduled, you will see the campaign under the Scheduled tab on the main page.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image8.png?raw=true")

### Step 2 - Run Access Campaign

Now that we have created an Access Campaign, we can either wait for it to be launched automatically based on the start time and date configured, or we can manually launch the campaign. To launch the campaign, in the Administration console, go to  **Identity Governance > Access Certifications**  and under the "Scheduled" tab, there will be the campaign you created in the previous step. Select the campaign and then in the top right corner, you will see an "Actions" drop down list. Click the drop down list and select "Launch". And then on the popup, click "Launch" again.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image9.png?raw=true")

Next, open up a private/incognito browser window and go to  [https://www.mailinator.com/](https://www.mailinator.com/). Then enter in the reviewers username of "chief.wiggum#@mailinator.com". (Replace the # with your test tenants number). The users inbox should contain the following email:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image10.png?raw=true")

Click on the button titled "View Assigned Reviews". The Access Certification portal will open and the user will be landed on the Reviewers Dashboard.

**Note:**  Users can also get to the Reviewers Dashboard by clicking the "Okta Access Certification Reviews" tile on the Okta Dashboard.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image11.png?raw=true")

Then on the Reviewers Dashboard, select the assigned campaign. The detail page should contain four pending reviews.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image12.png?raw=true")

Next, click the "Approve" or "Revoke" button next to each user. You will be prompted to optionally enter in a reason for approving or revoking the users access.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image13.png?raw=true")

Its recommended to add a justification as this will be included in the audit report. Ensure that you revoke the access for at least one user.

Now go back to the browser window where you are logged onto the Okta Administration console. Go to  **Directory > Groups**  and select the group titled "SampleAppOne". The users that had their access revoked should no longer be members of this group, and as a result, will have lost access the the application titled "Sample App One".

### Step 3 - End Access Campaign
In the Okta Administration console, go to  **Identity Governance > Access Certifications**  and under the "Active" tab, there will be the current active campaign. The campaign will automatically end after the configured duration has elapsed. Rather than waiting, we are going to end the campaign now. Select the campaign and go to the "Actions" drop down box in the top right corner and select "End". Then in the popup, select "End Campaign".
  
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image14.png?raw=true")

On the "Access certification campaigns" summary page, select the "Closed" tab and select the campaign that justed ended. On the campaign detail page, select a user that had their access revoked. The users "Review Details" will open on the right of the screen. Under the "History", you will see who revoked the users access with the justification.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/010/image15.png?raw=true")

**Note:** This detail is also available in the Identity Governance reports under  **Reports > Reports > Access Certification Campaigns**. It will take a few minutes for the reports to be available once the respective campaign has ended.

*That completes this challenge.*
