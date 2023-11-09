## Use Case Overview
Account takeover is a major security risk that can result in malicious actors gaining access to resources and information by “stealing” a user’s account. This can happen through social engineering, purchasing stolen credentials on the dark web, phishing, or other means. 

Okta Workflows can be used to detect and react to potential account takeover (ATO) attempts by identifying certain risky behavior and taking action, such as notifying a security team or terminating a user’s sessions and even suspending the user account until further investigation can occur. 

## The Challenge
In this challenge, we are going to use Okta Workflows to detect account takeover attempts from multiple MFA resets. Using the supplied Workflow sample, we are going to monitor factor resets for a user during a monitoring window. If multiple resets are detected within that time window, action is taken:
1. A detailed alert is sent to a specified recipient
2. The user’s sessions are terminated
3. The user account is suspended

## Resources
This challenge uses a sample workflow that can be downloaded from here: [Sample Workflow](https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/iamse-blog/workflows-templates/tree/main/detect-potential-ato)

## Pre-Requirements
To complete this challenge, your workflow instance will require the following connectors pre-configured:
1. Okta Connector
2. Slack Connector

**Let’s get started!**

## Solution
### Step 1 – Enrol Users in MFA
In order to test the workflow, we will need some test users that have enrolled in at least two additional factors other than email and password. 
In the Administration console, go to **Security > Authenticators** and add some additional authenticators to your Okta tenant. For example, add Google Authenticator and Security Question.
Once added, your authenticator list should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/009/image1.png?raw=true)

Next, open a private/incognito browser window and log into your Okta tenant with one of your test users. On the Okta landing page, select the top right menu and select **Settings**. Then go through the setup process for at least two factors.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/009/image2.png?raw=true)
Repeat this process for a second test user.

### Step 2 – Configure the Sample Workflow

#### Import Sample Flows

Open the workflow console for your Okta tenant and select the **Flows** tab. On the left of the console, create a new folder with a meaningful name like "Detect Potential ATO".
Next, click the three dots on the folder name and then select import. Browse and select the downloaded sample file **detectPotentialAto.folder**.

Once the import is complete, the folder will contain the following flows:
1.  **[main] Process User MFA Deactivation Event** – Parent flow for this use case. Triggered by Okta - User MFA Factor Deactivated event card.
2. **[helper] Delete Table Row** - Deletes a single row from the table based on Row ID.
3. **[main] Clear Stale Records** - Scheduled flow to delete stale (outside of our monitoring window) events from the table.
4. **[helper] Check for MFA Reset Events within Time Window** - Helper flow for the List - Reduce card in "Analyze factor reset for potential ATO" flow. Constructs a notification email based on factor reset event details. Returns the notification message and count of factor resets to it's parent flow.
5. **[helper] Analyze Factor Reset for Potential ATO Attempt** - Logs factor reset event details in a table, then searches for other reset events that occurred within our monitoring window. Uses a List - Reduce to iterate over matching table rows to construct a notification email and determine how many resets have occurred for this user.

The folder will also contain the following table:
**Flagged MFA Resets** - Used to store all MFA resets so a time comparison can be made to previous resets.

#### Update Flow Configuration

***Okta Connector***
The following flows have  **Okta Connector**  cards that require updating:
1.  [main] Process User MFA Deactivation Event
2.  [helper] Analyze Factor Reset for Potential ATO Attempt

Within each flow, select the respective  **Okta Connector** card and update the connector to use your local connector.

***Slack Connector***
The following flows have  **Slack Connector**  cards that require updating:
1. [helper] Analyze Factor Reset for Potential ATO Attempt

***Add Your Slack Email***
The sample workflow will notify a user via a direct message (yourself). In production, this will likely be a security channel. Also note that the Slack connector can easily be replaced by the Teams Connector.

Open the flow titled [main] Process User MFA Deactivation Event and update the first Object Construct card to use your Slack email address.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/009/image3.png?raw=true)

Once the workflow configuration has been updated, turn each flow **On**.

### Step 3 – Test the Workflow
Now we are ready to test the workflow. Open a private/incognito browser window and log into your Okta tenant with one of your test users. On the Okta landing page, select the top right menu and select **Settings**. 
Choose one of the factors that were enrolled in Step 1 and click the corresponding Remove button.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/009/image4.png?raw=true)

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/009/image5.png?raw=true)

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/009/image6.png?raw=true)

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/009/image7.png?raw=true)

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/009/image8.png?raw=true)

*That completes this challenge.*
