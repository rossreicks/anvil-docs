## Prerequisites

* Twilio account with a verified Toll Free phone number
* Anvil Messenger Application
* Integration User in Salesforce
* Permissions sets assigned to each user that will use Anvil Messenger
* Logged into a Salesforce user that has the permissions to create a connected app and is assigned the permission set "Anvil_Messenger_Admin"

![[Screenshot 2024-06-20 at 9.02.42 AM.png]]
*Validate that an integration user exists with Minimum Access*

![[Screenshot 2024-06-20 at 10.17.33 AM.png]]
*The Integration User needs to have the permission set "Anvil Messenger User"*

## Step 1: Create a connected app and generate credentials

![[Screenshot 2024-06-20 at 9.18.38 AM.png]]
*In Setup, search App Manager*

![[Screenshot 2024-06-20 at 9.18.56 AM.png]]
*Click "New Connected App"*
![[Screenshot 2024-06-20 at 11.06.01 AM.png]]
*Create a new connected App with OAuth settings, any valid callback url (this won't be used), api permissions, and enable client credentials flow*

![[Screenshot 2024-06-20 at 9.20.40 AM.png]]
*After your app has been created press "Manage Consumer Details"*

![[Screenshot 2024-06-20 at 10.03.44 AM.png]]
*Salesforce will Email you a code to verify your identity*

![[Screenshot 2024-06-20 at 10.04.35 AM.png]]
*Copy these keys and save them for a future step*

![[Screenshot 2024-06-20 at 9.20.40 AM.png]]
*Back on the connected app tab, press "Manage"*

![[Screenshot 2024-06-20 at 10.00.09 AM.png]]
*Press "Edit Policies"*

![[Screenshot 2024-06-20 at 10.00.23 AM.png]]
*Press the search icon in the "Run As" in the Client Credentials Flow section*

![[Screenshot 2024-06-20 at 10.00.46 AM.png]]
*Select your integration user you are using for this connection and then press save on the edit policies screen*

## Step 2: Twilio Forwarder Function

![[Screenshot 2024-06-20 at 11.23.01 AM.png]]
*Before leaving Salesforce, copy the instance url from your browser. This is just the domain name like `https://my-demo-org.lightning.force.com`*

![[Screenshot 2024-06-20 at 9.10.32 AM.png]]
*Over in the Twilio Console, search for "Functions"*

![[Screenshot 2024-06-20 at 9.11.06 AM.png]]
*Click on "Services" in the left hand sidebar*

![[Screenshot 2024-06-20 at 9.11.34 AM.png]]
*Create a Service and give it a name*

![[Screenshot 2024-06-20 at 9.11.44 AM.png]]
*Press the "Create your function" button and press enter*

![[Screenshot 2024-06-20 at 9.12.41 AM.png]]
*Paste the following code to this window: `exports.handler = require('twilio-anvil-forwarder');` this will handle forwarding the messages from Twilio to Salesforce*

![[Screenshot 2024-06-20 at 9.12.56 AM.png]]
*Click "Environment Variables" in the bottom left Settings menu*

![[Screenshot 2024-06-20 at 9.14.02 AM.png]]
*Create 3 variables, `SF_INSTANCE_URL`, `SF_CONSUMER_KEY`, `SF_CONSUMER_SECRET` and set their values to the 3 values we copied earlier*

![[Screenshot 2024-06-20 at 9.14.26 AM.png]]
*Click "Dependencies" in the bottom left settings section and enter `twilio-anvil-forwarder` in the Module input and press "Add"*

![[Screenshot 2024-06-20 at 9.14.34 AM.png]]
*Press the "Deploy All" blue button in the bottom left corner*

![[Screenshot 2024-06-20 at 9.14.47 AM.png]]
*Once it has deployed, it should let you know it was successful*

![[Screenshot 2024-06-20 at 9.16.04 AM.png]]
*Press the 3 dots next to the function name and press "Copy URL" we will use that in the next step*

## Step 3: Create the Twilio Messaging Service


![[Screenshot 2024-06-20 at 9.05.23 AM.png]]
*Back to the Search bar, Search for "Messaging Services"*

![[Screenshot 2024-06-20 at 9.06.00 AM.png]]
*Click Services in the left hand side menu and press the "Create Messaging Service" button*


![[Screenshot 2024-06-20 at 9.06.44 AM.png]]
*Give your service and Friendly name and press "Create Messaging Service"*

![[Screenshot 2024-06-20 at 9.06.55 AM.png]]
*We need to assign your Toll Free number to this service. Press "Add Senders"*

![[Screenshot 2024-06-20 at 9.07.02 AM.png]]
*Press "Continue"*

![[Screenshot 2024-06-20 at 9.07.14 AM.png]]
*Select your toll free number and press "Add Phone Numbers"*

![[Screenshot 2024-06-20 at 9.07.42 AM.png]]
*Press "Step 3: Set up integration"*

![[Screenshot 2024-06-20 at 9.17.08 AM.png]]
*Select "Send a webhook" option and paste your copied URL into the 3 input boxes for URLs*

![[Screenshot 2024-06-20 at 9.17.34 AM.png]]
*Press "Complete Messaging Service Setup"*

## Step 4: Set Twilio Settings in Salesforce


![[Screenshot 2024-06-20 at 9.04.28 AM.png]]
*Before leaving Twilio, go to the home page, you should see your Account Info. You will need this Account SID and Auth Token*

![[Screenshot 2024-06-20 at 10.04.59 AM.png]]
*Back in Salesforce, in the Waffle Menu, Search "Setup" and click the Setup option*

![[Screenshot 2024-06-20 at 10.05.09 AM.png]]
*This is the Anvil Messenger Setup page, here we will enter our twilio credentials*

![[Screenshot 2024-06-20 at 10.05.35 AM.png]]
*After your validate your credentials, select your Default Messaging Service from the dropdown*

![[Screenshot 2024-06-20 at 10.06.32 AM.png]]
*A User Assignments sections should appear, we need to assign each user to a messaging service, select your user's from the search menu to assign them to this messaging service*

![[Screenshot 2024-06-20 at 10.06.47 AM.png]]
*Next, enable outreach sending so we can allow our users to send scheduled marketing messages to our customers*

## Step 5: Validate

![[Screenshot 2024-06-20 at 10.07.39 AM.png]]
*Create a new Contact to test with, enter a valid phone number that you can test with*

![[Screenshot 2024-06-20 at 10.10.12 AM.png]]
*On the contact screen, you should have a message window on the right, try to send a message!*

![[Screenshot 2024-06-20 at 10.17.54 AM.png]]
*Reply to the message and you should see that appear!*

