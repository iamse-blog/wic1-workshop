# Mover Flow

## Use Case Overview

In the mover flow portion of this use case, we are going to use workflows to automatically change a users group membership, based on a change of value within an attribute on the users profile. The common way to appropriate a certain level of access in Okta is via group membership. Based on a users groups, they will be provisioned access to certain applications and a certain level of access within each application.

**Note:** This same process of allocating group membership from a attribute value, can also be orchestrated by Okta's Group Rules. But with Okta Workflows, we can perform a more customised provisioning and deprovisioning process. In this case, we are going to record a users change of access via a workflow table.

**Let’s get started!**

## Solution
### Step 1 - Create Workflow Table

We are going to start by creating a workflow table to record a user profile update. Within your workflow folder, select the Tables tab at the top of the page.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image1.png?raw=true")

Click the "New Table" button and create a table. Give it a meaningful name like  _**User Profile Update**_  and add the following text columns:

1.  userProfileChanged
2.  userId

### Step 2 - Create Flow

Next, select the "Flows" tab at the top of the page and create a new flow. Give the new flow a meaningful name like  _**Mover Flow**_  and ensure  _**Save all data that passes through the Flow**_  has been ticked. Then click the "Add event" button on the left and select your Okta connector.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image2.png?raw=true")

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image3.png?raw=true")

Then within the Okta connector, choose the event titled  _**User Okta Profile Updated**_. Next we would like to populate the workflow table that you just created. To the right of the Okta connector card, click on the "Function" button and the function popup will open. Then scroll down and select "Tables" on the left. Then within the tables function, click "Create Row".

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image4.png?raw=true")

Then click "Choose Table" and then browse and select the table you have previously created. Note that you will need to select the parent folder of you flows first. Then click "Save". By default all the columns will be available for input.

Now drag the "Changed Attributes" value from the Okta event to the "userProfileChanged" column in your table. Then drag the Okta User "ID" to the "userId" column in your table. The result should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image5.png?raw=true")

Now save the flow and turn the flow ON by using the toggle switch at the top of the flow.

At this point, we should test the flow to ensure it runs as expected and populates the table with a users attribute changes. Go to the Administration console and go to  **Directory > People**. Then select one of your test users. Go to the users profile tab and click Edit. Then update any attribute on the users profile. For example, update the users "Title" to a specific value. Then click save at the bottom of the page.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image6.png?raw=true")

Now go back to the workflow console and open your workflow table. It should contain a record indicating the user Id and attribute name of the modified attribute.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image7.png?raw=true")

If your table has not updated, open your flow and select "Flow History" at the top left. This should help indicate what went wrong.

### Step 3 - Add Template

Okta Workflows comes with “Templates” - you can see these as accelerators for your flows or tutorials of how cards are used in combinations to accomplish different scenarios. Let us use one popular template of managing group memberships based on user profile attributes. In our example, we will use the "Title" attribute - so if a person goes from  _**Engineer**_  to  _**VP**_  to  _**Engineer**_  again, his group memberships are in line with his job function. In the customer identity case this could be a “Gold” loyalty level or a “Silver loyalty level” user profile attribute that changes the user’s group membership.

To add the required template to your workflow instance, follow the steps below:
1.  Within the workflow console, click on the "Templates" link at the top of the page.
2.  Then within the search field, enter "Manage Okta Group Membership" and hit enter.
3.  Then select the template titled "Manage Okta Group Membership Based on Profile Attributes".
4.  On the right of the template, click "Add template". Add the template to your workflow instance. This will result in the creation of a new folder with four flows and one table1.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image8.png?raw=true")

We have to set the connections to Okta in the “[1.0] Fix Groups” and “[1.2] Group Addition or Removal” flows. Click on “[1.0] Fix Groups” and open the flow.

Notice this flow is created as a “helper flow” to trigger the workflow. This makes it easy to incorporate into other flows - think of this like a reusable function. Creating a flow to start as a helper flow is also very helpful to test easily.

For now, Click on “Choose Connection” and set it to your Okta connection.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image9.png?raw=true")

Then click "Save". Ensure the check box for  _**Save all data that passes through the Flow**_  has been ticked. Then turn the flow on.

This flow uses many "List" cards - so it creates a list of groups the user should be in, compares it with the list of groups the user is currently assigned to and then calculates the delta and adds the user or takes away the user as needed.

Update flow “[1.2] Group Addition or Removal” to use the Okta connector. Save this flow and turn it on.

Turn the remainder of the flows on.

Now this flow uses a workflow table as a lookup table. This is the feature that gives Okta Workflows a powerful way to manage group memberships as you will see.

The table is initially empty but it already has the columns created based on the template. Let us import data from a csv file. (The csv file will be supplied by the instructor)

Click on the "Tables" tab within the installed folder. Then click on the table titled "Group Rules". On the top right, click on "Import" and select the supplied CSV file. Once the import is complete, the table will have six records.
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image10.png?raw=true")

We should now test the installed Manage Okta Group Membership flows before we move to the next step.

Before we do this, we need to set up a test user with the correct data. Open the Administration console and go to  **Directory > People**. Open a test user, click on the "Profile" tab and ensure they have the value of "Engineer" set for the "Title" attribute:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image11.png?raw=true")

Then click on the Groups tab and ensure they are only a member of the Everyone group.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image12.png?raw=true")

The Manage Okta Group Membership flows will use the table to assign the user to the respective group. Based on a value of Engineer in the users Title, the user will be assigned to Group 3.

Now go back to the Workflow console and ensure all the flows have been turned on in the Manage Okta Group Membership Based on Profile Attributes folder.

Next, open the main flow titled "[1.0] Fix Groups".

Then click the "Test" button at the top of the flow to run the flow in the debug window. The flow will prompt you to enter a test user. Enter the username of the test user that we just configured.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image13.png?raw=true")

Then click the "Run Test" button. If successful, the user should have been added to Group 3. Here are the last two cards in the debug window:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image14.png?raw=true")

If your user was NOT added to Group 3, then follow the flow in the debug window to see where the process failed. Your instructor can assist you at this point.

To check the user is actually in Group 3, open the Administration console and go to  **Directory > Groups** and open Group 3.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image15.png?raw=true")

### Step 4 - Add Filter

In this step we are going to go back to our original flow and add a filter for the "Title" attribute. In other words, we only want the flow to continue executing if the the "Title" attribute was changed. If any other attribute has been changed, then we are not interested and there is no need to continue the flow.

In the workflow console, open your mover flow. After the "Create Row" card, click the "Add function" button and scroll down to the "Text" functions. Then on the right, select the "Find" operation. The Find operation will find a certain string within a body of text. If it exists, it will return an integer, indicating the starting position of the text. If the text does not exist, it will return -1.

We then need to drag the "Changed Attributes" from the incoming event, into the first field on the Find card (look in). In the second field of the Find card (for what), type in "title". Then give the potput a meaningful name like  _**position**_. The card should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image16.png?raw=true")

Since the Find card will return a -1 if "title" is not one of the changed attributes, we now need to just exit the flow if -1 is returned.

On the right of your flow, click "Add function" and then select "Branching". Then on the left, select "Continue If". This is a common card and is used often. Now drag the output from the "Text Find" card, to the first field on the "Continue If" card (value a). Then within the "comparision" field, from the drop down list, select "not equal". The enter -1 in the "value b" field.

In other words, this card will only continue the flow if the value is NOT -1, or if the "Text Find" card found an occurance for the attribute "title". The last two cards of the flow should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image17.png?raw=true")

### Step 5 - Complete Flow

Now we can update our flow to call the template we installed in a previous step. On the right of your flow, click "Add function" and then select "Flow control". On the right, select "Call Flow". Then click the button "Choose Flow" and select the flow titled "[1.0] Fix Groups" from the Manage Okta Group Membership folder. This flow requires just one input, "username". Drag the "Alternate ID" from under the "Okta User" in the starting event, to the input field. The result should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image18.png?raw=true")

We have completed the creation of the flow, so you can turn your flow on.

In the Administration console, go to  _**Directory > People**_, and select a test user. Go to the "Profile" tab and click "Edit". Update the users value for "Title" to either Engineer, Manager or VP. In this instance, I have changed my previous test user from Engineer to VP.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image19.png?raw=true")

Then click "Save".

Go back to your workflow console and within your flow, select "Flow History". The history should indicate that the flow ran and completed successfully. You can also do the same for the helper flow "[1.0] Fix Groups".

If you go back to the Administration console and open up your test user, they should new be a member of all the groups aligned with the respective Title.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/004/image20.png?raw=true")
You can follow the same process and ensure the workflow removes the user from certain groups if their title changes again.

*That completes part B of this challenge, the Mover flow.*
