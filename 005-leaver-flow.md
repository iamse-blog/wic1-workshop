

# Leaver Flow

## Use Case

The last part of this use case is the Leaver flow. In other words, we need to deprovision access or notify an external system, when a user leaves the organisation. We are going to simulate the notification of an external system by calling a web service endpoint when a user is deactivated.

**Let’s get started!**

## Solution

### Step 1 - Create Flow and Add Event

Select the "Flows" tab at the top of the page and create a new flow. Give the new flow a meaningful name like _**Leaver Flow**_ and ensure _**Save all data that passes through the Flow**_ has been ticked. On the left of the flow, click "Add event" and select your Okta connector. Then search for and select the event "User Deactivated".

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/005/image1.png?raw=true")

### Step 2 - Create API Connector

Before we can call an external service, we need to create an API Connector we can use. At the top of the page, select "Connections". Then click "New Connection". From the list, select "API Connector". This is a generic connector that can be used to call a web service endpoint. From the "Auth Type" drop down list, select a authentication type of "None". In practice, you would select the required authentication type that the service endpoint requires. For our test, we are just going to call an endpoint with no authentication.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/005/image2.png?raw=true")

### Step 3 - Add API Connector to Flow

In the workflow console, select "Flows" at the top of the page. Then open your Leaver flow. Click the "Add function" button on the right of the flow and scroll down and select "API Connector (HTTP)". Then on the right, select the "Get" operation.

At the top of the API Connector card, select the API Connector that you created in the previous step.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/005/image3.png?raw=true")

Then in the URL field, enter the following:  [http://petstore-demo-endpoint.execute-api.com/petstore/pets](http://petstore-demo-endpoint.execute-api.com/petstore/pets)

Your flow should now match the following:
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/005/image4.png?raw=true")

Now using the toggle switch, turn the flow on. Then click the "Test" button at the top and run your flow. As we are not using any of the data from the incoming User Deactivated event in the flow, you can leave all the input fields blank. When the flow runs, the API Connector will perform a Get operation on the configured endpoint.

The result should be the following:
![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/005/image5.png?raw=true")

### Step 4 - Parse JSON Response

Now that we have a JSON response from the web service call, lets use Okta Workflows to parse and extract the results.

Click the "Add function" button on the right of the flow and scroll down and select "JSON" and then on the right, select the "Parse" operation. Then drag the "Body" from the API Connector response to the input of the Parse card. On the output for the Parse card, select the drop down on the right hand side of the field and check the "List" indicator. This will parse the response into a list that workflows can understand.

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/005/image6.png?raw=true")

Note that once the output has been defined as a list, three orange bars appear on the field. This indicates that it can be used as input for any cards that receive a list.

As an example of how we can work with lists, we are going to get the first item from the list. Click the "Add function" button on the right of the flow and scroll down and select "List" and then on the right, select the "Get First Item" operation. Now drag the output from the Parse card to the list input on the "Get First Item" card.

Your flow should now match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/005/image7.png?raw=true")

Now that we have the first item in the list, we are going to extract the values from the JSON object. Click the "Add function" button on the right of the flow and scroll down and select "Object". Then on the right, select the "Get Multiple" operation. Then drag the output from the "Get First Item" card onto the input for the "Get Multiple" card. Now enter the name of each JSON element in the input box titled "Click or drag to create". The element names are:

1.  id
2.  type
3.  price

The flow is now complete and should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/005/image8.png?raw=true")

Ensure the flow is turned on and click the "Test" button at the top to run the flow. If the flow runs successfully, the last two cards in the flow execution, should match the following:

![](https://github.com/iamse-blog/wic1-workshop/blob/main/images/005/image9.png?raw=true")

That is the completion of Part C of this challenge.

