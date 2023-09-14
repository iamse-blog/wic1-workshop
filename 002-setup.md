# Workshop Tenant Setup

## Okta Tenant
On enrolling in this lab, an Okta tenant has been auto-generated for you. You should have received an email to activate your account. When you log in for the first time, you will be prompted to set your own password.
Your tenant has been pre-configured with the following resources:

### Pre-Configured User Accounts
The following test users have been created in your Okta tenant:
* bart.simpson#@mailinator.com
* dean.simpson#@mailinator.com
* homer.simpson#@mailinator.com
* barney.gumble#@mailinator.com
* chief.wiggum#@mailinator.com

**Note:**  The # will be replaced by an assigned number. This will help identify your test users when this lab is performed in a workshop environment.

### Pre-Configured Groups
The following test groups have been created in your Okta tenant:
-   Group 1
-   Group 2
-   Group 3
-   Staff
-   O365Guest
-   SampleAppOne
-   SampleAppTwo

### Pre-Configured Applications
The following test applications have been created in your Okta tenant:
-   Sample App One
-   Sample App Two

## Okta Workflows Setup
Before we commence the challenges in this lab, we need to configure a number of workflow connectors. These will be used throughout the workshop. Where the connectors need specific account information, the workshop instructor will provide the relevant endpoints and credentials. 

The following connectors are required:
1. **Okta Connector** - This will connect to the underlying Okta tenant.
2. **Slack Connector** - The labs will involve sending messages to Slack channels.
3. **API Connector** - This is a generic Web Service connector used to connect to any endpoint.
4. **Mail Connector** - Either Office 365 or GMail
5. **Office 365 Admin Connector** - This is required when completing the Office 365 Guest User lab.

**Lets set up those connectors now.**

Start by logging into your Okta tenant and within the Admin console, go to **Workflow > Workflows console**. This will open the workflow console in a separate tab. The workshop instructor will provide an overview of the console.

Select **Connections** at the top of the workflows console.

### Okta Connector Setup
Click **New Connection** and select the Okta connector.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/002/image1.png?raw=true")
Next we need to provide a meaningful name. As we will only be using one Okta connector, its OK to leave the default as "Okta".

Next we need your Okta tenants domain name. This needs to be the URL without "https://" or "admin" or "workflows". You can get the correct URL by going to the end users application dashboard. There will be a link in the top right of the Admin console.
EG: 
`demo-green-silkworm-44581.okta.com`

We also need a **Client Id** and **Client Secret**. These come from the workflow application pre-created in your Okta tenant. Go back to the Admin console for your Okta tenant and select **Applications > Applications**. The application that is created for the Okta connector is titled "Okta Workflows OAuth". Select this application and go to the **Sign On** tab. This is where we can copy the Client Id and Client Secret.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/002/image2.png?raw=true")

Your Okta connector should now look like the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/002/image3.png?raw=true")

Next, click the **Create** button. At this point, the configured settings will be validated and if correct, the connector will be created. If successful, your Okta connector should have a green tick next to it, indicating that it is active.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/002/image4.png?raw=true")

### Slack Connector
Click **New Connection** and select the





