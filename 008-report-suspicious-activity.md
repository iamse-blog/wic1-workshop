## Use Case Overview

Suspicious Activity Reporting provides an end user with the option to report unrecognized activity from an account activity email notification. When end users receive a security email notification, they can send a report by clicking Report Suspicious Activity. The default behaviour is for the Suspicious Activity Report to be sent to the Okta system log. Its then up to the Okta Administrators to run a report on the system log to get a list of all the users that have reported suspicious activity.

Here is an example of the default email that a user would receive:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image1.png?raw=true")

Users would receive a similar email of any other factor was reset. Note that the email contents can be fully customized as per an organizations own standards.

Okta workflows provides the ability to act on the report immediately and automatically. Workflows also provides a template for this use case. This challenge will be enabling the template in your Okta Workflows instance.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image16.png?raw=true")

**Let’s get started!**

## Solution

### Step 1 - Configure Okta

Sign onto the Okta Administration console and go to  **Security > General.**

Under  _**Security notification emails**_  there are a number of settings for different emails sent to the users. Ensure the following are enabled:

1.  Password change notification email
2.  Authenticator reset notification email
3.  Report suspicious activity via email

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image2.png?raw=true")

### Step 2 - Add Template

Sign onto the Okta Workflow console and click "Templates" at the top of the page. Then in the search criteria, type in "reported" and enter. Select the template titled "Suspicious Activity Reported".

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image3.png?raw=true")

Then click the "Add template" button.

This will create a new folder titled "Suspicious Activity Reported" containing one flow and one table. The table is titled "SuspiciousActivityReportedLog" and will be used to record each report. The flow is titled "Suspicious Activity Reported".

Open the flow and click the button labeled "Choose Connection" on the Okta event card on the left.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image4.png?raw=true")

Choose you configured Okta connector. This will automatically update all the other Okta cards in the flow to also use your configured Okta connector.

Next, scroll to the right and find the Okta "Reset Password" card. We need to complete the configuration of the options for this card. Click on the "Options" at the top of the card:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image5.png?raw=true")

Then select "Yes" for the value for "Send Email".

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image6.png?raw=true")

Then click "Save". This should update the Okta connector for all the Okta cards in your flow.

Next, we need to update the Okta tenant URL in the compose card. This is used to retrieve the report data from the tenants system log. Update thje following to match the URL of your tenant:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image7.png?raw=true")

**Note:**  Make sure the URL include the "-admin" after the tenants sub-domain name. This is used to direct the Okta Administrator to the event in the system log.

Next we need to update the connector for the Slack "Send Message to Channel" card. Click the button labeled "Choose Connection" and slect the Slack connection in your workflow instance.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image8.png?raw=true")

Once you click Save, you then need to update the cards Options. Click on the "Options" as per the screen shot below:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image9.png?raw=true")

Then choose a channel from the drop down list. Your instructor will advise which channel to choose.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image10.png?raw=true")

Then click "Save".

Now save your flow and using the toggle switch at the top, turn your flow on.

### Step 3 - Test Flow

We now need to generate a Report Suspicious Activity event to test the flow. To do this, open a private / incognito window of your browser and log into your Okta tenant as one of your test users. Once logged into Okta, on the top right of the Okta dashboard, click the drop down menu and click "Settings.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image11.png?raw=true")

Then within the setting screen, under Security Methods, click Reset next to "Password".

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image12.png?raw=true")

Complete the reset process.

If you have been using a test user with the "mailinator.com" domain, open the Mailinator web page and open the public inbox for the respective test user. There should be a new message with a subject of "Password Change". Open the email. It should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image13.png?raw=true")

Now click on the "Report Suspicious Activity" button. Then on the next screen, click on "Report Activity".

Once you have completed the report, you should see the following:

1.  Go back to the email in-box for the respective test user. You should see a new email titled "Account password reset" notifying the user that their password has been reset.
2.  Go back to the logged in session for your test user. Click refresh. The user should have been logged out.
3.  Go back to the workflow console and within your flow, click "Flow History". There should be one successful execution of the flow.
4.  On the Slack channel, there will be a message from your flow. The workshop instructor will check for you.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image14.png?raw=true")

If the Okta Administrator now clicks the link titled "Link to Okta Syslog Event", they will be sent to **Reports > System Log** in the administration console, which will display the respective event.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/008/image15.png?raw=true")

*That completes this challenge.*
