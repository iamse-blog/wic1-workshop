

# Joiner Flow

## Use Case

A new user joins your organization and we would like to provide a more personalised welcome message, plus convey some essential information like the users Employee Number. To do this we are going to use Okta Workflows to send a welcome message to a Slack Channel. Additionally we need to read the user’s profile and retrieve their Employee Number and then format and send a personalised email to the respective user, notifying them of their number.

**Let’s get started!**

## Solution

### Step 1 - Create Flow

Create a new flow and give it a meaningful name like Joiner Flow. Ensure you have enabled the property  Save all data that passes through the Flow?

![](https://media.graphassets.com/tnsLYUkQUigidZGwLzMw "f6f710d9-ffb2-41c6-815d-1190ab99dd03.png")

### Step 2 - Add Okta Event

To start the flow, click Add event and select the Okta connector. Then select "User Activated" as the event.

![](https://media.graphassets.com/hPyyLRxQJ8oZLJcM0jkQ "Snip20230711_63.png")

![](https://media.graphassets.com/zZaArsZCShKn7P7CwbfC "Snip20230711_64.png")

### Step 3 - Compose Slack Message

Add a text "Compose" card and enter a welcome message. Include the respective users name in the message by dragging the "Display Name" from the "User Activated" event card on the left. Be sure to select the Display Name from under the Okta User in the event. Then give the output from the compose card a meaningful name.

![](https://media.graphassets.com/aWP770fHToWwtvNhYrSq "3c95b6dd-030a-4975-9b83-952308fd232a.png")

### Step 4 - Send Message to Channel

To the right of the "Compose" card, select the Slack Connector and select "Send Message to Channel". Then select a channel from the drop down list and optionally populate the remainder of the input fields. Select a Message Type of "Plain Text". Once you have saved the configuration, drag the output from the text "Compose" card to the "Text" message input for the "Slack" card.

![](https://media.graphassets.com/4axZVwU6Rh2bRvqxfCm9 "077e4ea5-c814-4390-b2d3-d0fb70d6d385.png")

### Step 5 - Test Your Flow
Save your flow and activate it by using the ON/OFF toggle switch at the top of the flow.

At this point we are going to test the flow before we progress any further. Now go to the Administration console and activate an existing test user or alternatively you can create a new test user at this point. Once the user has been activated, the flow will be executed. You can check the execution by going to the "Flow History" of your flow in the Workflow console.

![](https://media.graphassets.com/iZpOaAg4S8amD4QwxkQx "704096ee-1c04-407f-8ce7-2fa570ab9d4f.png")

This will result in your message going to the Slack channel.

![](https://media.graphassets.com/CgZSBwqlT5CvS2J4cUXt "79b044c8-c296-4937-810b-a4be49c7357d.png")

### Step 6 - Read User Profile

We now need to read the users profile so we can get their Employee Number. On the right of your flow, click "Add app action" and select the Okta connector. Then select "Read User". You then need to select what outputs you would like to retrieve from the users profile. Select the following:

1.  Username
2.  First name
3.  Last name
4.  Primary email
5.  Employee number

Then click Save to complete the configuration. Now drag the user "ID" from the incoming User Activated event card to the input for the Read User card.

![](https://media.graphassets.com/tMBcqimYSACGJeu6p76D "8f788ff9-fa18-466c-9e0e-835136ac2b3c.png")

### Step 7 - Compose Email Message

We now need to compose a message containing the users Employee Number. On the right of your flow, click Add function. Then select a text "Compose" card. Compose a message to the user containing the user's Employee Number which was retrieved from the previous Read User card.

![](https://media.graphassets.com/35mqsMl6TsiM1CGB7Cs3 "9f11b0af-0074-43de-9a3c-e62c6e777ea0.png")

### Step 8 - Send Email
Next, click Add app action on the right of your flow. Select the Gmail connector. Then select "Send Email". Drag the users "Primary email" from the Okta Read User card to the Send Email card. Enter a value for Subject and drag the composed message to the input for "Body". Now save the flow.

![](https://media.graphassets.com/js5oT2FXTYS2k4ldictu "d6fa96b9-2963-4193-b714-e66d89b546a9.png")

### Step 9 - Test Your Flow

Now the flow is complete, we can now test the flow. Within the Administration console, examine the properties for your test user to ensure it contains a value for Employee Number.

![](https://media.graphassets.com/BmgeuoxiTZCwtHF1gnof "f1dc7331-cc6f-41e4-bc79-e96561a825bf.png")

Next, set the users status to Deactivated. Then Reactivate the user. This will generate another event which will cause your flow to execute. Within the Workflow console, check the Flow History of your flow. The flow history should be a new execution. Check the execution to ensure all cards executed successfully.

Now check the email inbox for your user. There should be a message from your flow.

![](https://media.graphassets.com/mI2sfx1QQu97zZyOX1cJ "Snip20230711_65.png")

That completes Part A of this challenge, the Joiner flow.
