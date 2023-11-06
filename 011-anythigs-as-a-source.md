## Use Case Overview
Okta customers, particularly in the Workforce Identity space, are looking to model and, where possible, automate the IT processes associated with individuals joining, moving within, or leaving their organization. These processes are driven by changes to data in an organization’s source of truth for identity information. The driving forces behind automation include improving IT efficiency, security and end user productivity while also reducing costs.

Okta provides Out of the Box integrations with directories (AD, LDAP and CSV) and a limited number of HR systems. So if your source of truth falls within these boundaries, on-boarding users into Okta is simple and easy with Okta’s Lifecycle Management solution. But what if your HR system is not pre-integrated into Okta?

Up until recently, the choices were the following:

-   **Integration Middleware** – Customers could purchase an intermediary solution such as an API-driven middleware provider to develop integrations.
-   **Custom Code and Scripts**  – Customers could also write and maintain their own custom code and scripts which automate the IT components associated with their organization’s joiner/mover/leaver processes.

These two options are very expensive to build, test and maintain as well as time consuming. Additionally, these solutions leveraged the Users API (or SCIM Server), which pushes integration responsibility to the client. The client must map data and user states, have the right triggers to push updates and handle data synchronization. This is complex to orchestrate, which further increases the build, test and maintain cycle.

Introducing  **Anything-as-a-Source (XaaS)**. This new feature provides the following:

1.  Makes it easier for integrators to connect any source of truth to Okta without requiring them to rebuild basic ETL functionality already provided by Okta’s import pipeline. A brand new XaaS API replaces the need to use the Users API. This now moves key integration responsibilities from the client to Okta, which greatly reduces the complexity on the client side.
2.  Within Workflows, the Okta connector has been updated to include the new XaaS API operations. This enables Okta customers to quickly and easily implement the client side logic within workflows, thus eliminating the need for Integration Middleware or Custom Code and Scripts.

Anything-as-a-Source allows you to integrate any source of truth with Okta, and realize the benefits of HR-driven provisioning from any source of truth. XaaS gives customers the flexibility to define the terms of synchronization between Okta and the source of truth.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image1.png?raw=true)

**Key Benefits**

-   Get increased choice and customization for triggers, filtering, attributes, and more
-   Simplify architecture by directly implementing connectors and do the work using fewer resources
-   Get the benefits of Okta Lifecycle Management automations without custom code or middleware

## The Challenge
In this challenge, we are going to use Okta Workflows to implement Anything as a Source against an external database table of users. Using the Anything as a Source functionality, we are going to:
 1. Synchronize the external table of user's into your Okta tenant as new user's.
 2. Apply some profile updates to the external database table and then synchronize those changes with the respective user's Okta profile.
 3. Deactivate some user's in the external database table and then synchronize that status change with Okta.

## Resources
This challenge uses a sample workflow that can be downloaded from here: [Sample Workflow](https://minhaskamal.github.io/DownGit/#/home?url=https://github.com/iamse-blog/workflows-templates/tree/main/anything-as-a-source)

We will also be using the following endpoint to synchronize users from the external database store: https://4b0xg2leu8.execute-api.us-west-2.amazonaws.com/xaas/items

## Pre-Requirements
To complete this challenge, your workflow instance will require the following connectors pre-configured:
1. Okta Connector
2. API Connector - Auth Type of None

**Let’s get started!**

## Solution
### Step 1 – Configure Custom Identity Source App
In this step, we are going to add the  **Custom Identity Source** app to Okta and configure the mapping from the incoming user profile to the Okta profile.

See the official Okta documentation on the Custom Identity Source application here:  [Use Anything-as-a-Source](https://help.okta.com/oie/en-us/Content/Topics/users-groups-profiles/usgp-use-anything-as-a-source.htm?cshid=ext-use-xaas)

#### Add Application

In your Okta Administration console, go to  **Applications > Applications**  and then click on  **Browse App Catalog**. Then search for  _**Custom Identity Source**_.

Add the following application to your Okta tenant:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image2.png?raw=true)

#### Configure Application

Once you have added the app to your Okta tenant, give the app a meaningful name. In my example, I have named it  _**My HR Database Table User**._

Next, go to the  **Provisioning tab**  and select the  **Integration**  option on the left menu. Click  **Edit**  and check the box for  _**Enable API Integration**_. In this example, we will not be importing Groups, so you can un-check the box for  **Import Groups**. Then click  **Save**. See the screenshot below:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image3.png?raw=true)

Next, under the  **Provisioning tab**, select the  **To Okta**  option on the left menu. In the section titled  **User Creation & Matching**, click  **Edit**  and check the boxes for the following settings:

1.  Auto-confirm exact matches
2.  Auto-confirm new users
3.  Auto-activate new users

Then click  **Save**. If these settings are not enabled, the administrator will have to manually confirm and activate the imports. See the screenshot below:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image4.png?raw=true)

In the section titled Profile & Lifecycle Sourcing, click Edit and check the box for Allow Custom Identity Source to source Okta users. You can also optionally check the boxes for Reactivate suspended Okta users and Reactivate deactivated Okta users. Then click Save. See the screenshot below:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image5.png?raw=true)

#### Update Identity Source Profile
The default profile for the Custom Identity Source Application contains the following attributes; Username, First Name, Last Name, Email, Second Email and Mobile Phone. If you are using the AWS DynamoDB example, then we need to add some additional custom attributes to the profile.

In your Okta Administration console, go to Directory > Profile Editor and select the profile for the Custom Identity Source Application added in the previous section. Then add the following custom attributes:

| **Display Name** | **Variable Name** | **Data Type** | **Attribute Type** |
|--|--|--|--|
|Title |title |string |Custom|
|Display Name |displayName |string |Custom|
|Primary Phone |primaryPhone |string |Custom|
|Zip Code |zipCode |string |Custom|
|Street Address |streetAddress |string |Custom|
|City |city |string |Custom|
|State |state |string |Custom|
|Country Code |countryCode |string |Custom|
|Department |department |string |Custom|

Once complete, click the **Mappings** button to bring up the profile mappings from the **Custom Identity Source App** profile to the Okta profile. Ensure all the default and custom attributes are mapped from source to destination. The mapping for the first three attributes is displayed in the screenshot below:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image6.png?raw=true)

### Step 2 – Configure the Sample Workflow

#### Add Required Scopes
The additional XaaS operations that have been added to the workflow Okta connector, require some additional scopes. In your Okta Administration console, go to  **Applications > Applications** and select the application titled  **Okta Workflows OAuth**. Open the **Okta API Scopes** tab and grant consent for the following two scopes:
1.  okta.identitySources.manage
2.  okta.identitySources.read

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image7.png?raw=true)

In order for the  **Okta Connector** to be able to access these additional scopes, it needs to be re-authorized. Open your workflow console and select Connections at the top of the page. Within your exiting connections, select the  **Okta connector.**  Click on the reauthorize icon on the right, and enter in your Domain, Client ID and Client Secret from the  **Okta Workflows OAuth** app.

#### Import Sample Flows

The contents of the downloaded zip file is as follows:

1.  anythingAsASource.folder – The workflow folder import
2.  readme.md – Install Instructions
3.  sampleUsers.csv – Sample user data (Not used as the external database table has already been populated and made available)

Open your workflow console and create a new folder and give it a meaningful name. In my workflow instance, I called my folder  **Anything-as-a-Source**. Click the three dots at the end of the folder name and select  **Import**. Import the file titled anythingAsASource.folder

Once the import is complete, the folder will contain the following flows:

1.  **[main] Populate DynamoDB Table** – This flow is used to synchronize the sample users supplied in the workflow table, with the AWS DynamoDB table. (Not used as the external database table has already been populated and made available)
2.  **[util] Upload Sample User** – This flow is called by [main] Populate DynamoDB Table and uploads an individual user to the AWS DynamoDB table. (Not used as the external database table has already been populated and made available)
3.  **[main] Scheduled Import Active Users** – This flow orchestrates a bulk upload of active users from the DynamoDB table into Okta.
4.  **[main] Scheduled Import Deactive Users** – This flow orchestrates a bulk upload of de-active users from the DynamoDB table into Okta.
5.  **[util] Profile Map to Okta Format** – This flow takes the user data from the DynamoDB user record and formats it into the expected format for the XaaS upload API.
6.  **[util] Delete Target Import Session** – This flow is used to clean up any old import sessions.
7.  **[util] Filter Active Users** – This flow will return a value of True if the user is in a active state.
8.  **[util] Filter De-active Users** – This flow will return a value of True if the user is in a de-active state.

#### Update Flow Configuration

***Okta Connector***
The following flows have  **Okta Connector**  cards that require updating:

1.  [main] Scheduled Import Active Users
2.  [main] Scheduled Import De-active Users
3.  [util] Delete Target Import Session

Within each flow, select the respective  **Okta Connector** card and update the connector to use your local connector.

***API Connector***
The following flows have  **API Connector** cards that require updating:

1.  [main] Scheduled Import Active Users
2.  [main] Scheduled Import De-active Users
3.  [util] Upload Sample User

Within each flow, select the respective  **API Connector**  card and update the connector to use your local connector. The  **Auth Type** of your  **API Connector**  should be a type of  **None**. As this is just an example Custom Identity Source, there is no security required.

***Custom Identity Source URL***

The following flows have  **API URL’s**  that require updating:

1.  [main] Scheduled Import Active Users
2.  [main] Scheduled Import De-active Users

Within each flow, select the respective  **API Connector** card and update the URL to point to the supplied endpoint.

***XaaS Workflow Cards***

The XaaS workflow cards need to point to your  **Custom Identity Source**  application created in Step 1. Open flow  **[main] Scheduled Import Active Users**  and select the  **Options**  for each of the following XaaS cards:

1.  List Import Sessions
2.  Create an Import Session
3.  Bulk User Import
4.  Trigger Import Session

Then under the  **Application**  option, select your  **Custom Identity Source**  application from the drop down menu and then select save. Do the same for flow  **[main] Scheduled Import De-active Users**.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image8.png?raw=true)

Once the workflow configuration has been updated, turn each flow **On**. (Ignore the flows titled [main] Populate DynamoDB Table and [util] Upload Sample User, as we will not be using these)

### Step 3 – Import Active Users
In this step, we are going to run the sample flow and see how the workflows can synchronize users from the external repository into Okta. The two scheduled flows that will synchronize users are:
1. _**[main] Scheduled Import Active Users**_  - Create and Update
2. _**[main] Scheduled Import Deactive Users**_ - Deactivate

In production, these flows will run at the desired schedule to synchronize new users, user profile updates and user status updates.

In the Workflow console, open the flow titled **[main] Scheduled Import Active Users** and click the Run button on the top right. The view will switch to the Execution History screen show the cards being executed. When the flow has completed, all the cards should have green ticks, indicating that they successfully executed.

Oce the flow has completed, in the Administration console, go to **Reports > Import Monitoring**. You should see a new import session has started. 

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image9.png?raw=true)

After a few minutes, the import session should complete and indicate that exactly 100 new users have been created in your tenant.
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image10.png?raw=true)

### Step 4 – Synchronize User Profile Update
In this step we are going to synchronize a user profile update. The workshop instructor will update at least one user profile in the external database table and advise you of the changes they have made. Once this has been completed, in the Workflow console, run the flow titled **[main] Scheduled Import Active Users** again. As well as creating users in Okta, this flow will also update users.

Once the import session has completed, the Import Monitoring report should indicate that at least one user profile has been updated, while the remainder of the users have been left unchanged.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image11.png?raw=true)

In the Administration console, go to **Directory > People** and search for the respective user that had a profile update. The users profile should now reflect the new values.

### Step 5 – Synchronize User Status Update
In this step we are going to synchronize a user profile status change by deactivating a user. The workshop instructor will deactivate at least one user in the external database table and advise you of the username. Once this has been completed, in the Workflow console, run the flow titled **[main] Scheduled Import Deactive Users**. 

Once the import session has completed, the Import Monitoring report should indicate that at least one user profile has been removed (Deactivated).

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/011/image12.png?raw=true)

In the Administration console, go to **Directory > People** and search for the respective user that was deactivated. The users status should now be deactivated.

*That is the completion of this challenge.*
