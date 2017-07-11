![STSA Logo](/images/STSALogo.png)
# Workshop 2 Watson Conversation Service

## Architecture
Below is the architecture overview of workshop 2, the Watson Conversation Service (WCS). This architecture
is consistent with the reference implementations of WCS for cloud native applications using microservices.
![Architecture Overview](/images/wk2-arch-overview.png)


## Application Overview
Workshop 2 is intended to help you understand the basics of the Watson Conversation Service (WCS)
as part of the Watson API's. WCS is a question and answer system that focuses on providing
a dialog type of experience between the user and the conversation system. This style of interaction
is commonly called a bot. The intent of this lab is to take the base work model from workshop 1
and extend the lab to leverage the WCS capabilities. We will enable through a dialog approach
WCS interacting with the data from the IoT device, asking questions specific to the data that is
captured. The lab will also allow for commands to be sent to the IoT device, directed from the
response of the WCS dialog flow. Though the example is simple, it will provide you with a
solid understanding of the core pieces of WCS.

### Terminology
WCS has a couple of terms that need to be understood. This will allow for an easier time with
creating an application (i.e. Dialog) in WCS

**Intent:** An intent is the intention of the command or question given by the user to WCS.
	It is common to think of intents as the verbs or actions that need to occur. An Example of
	an Intent is "Tell me the temperature" or "I want to know the current temperature". In both
	cases, though the sentences are different they both as asking WCS for temperature information.
	It should be noted that WCS can only support a single Intent per interaction with WCS. So
	asking questions with multiple intents will produce unreliable ordering of answers to the question.

**Entities:** An entity is the object that intents use to help narrow the scope of the request.
	It is common to think of entities as nouns or objects. The nice thing about entities, compared to
	intents, is that you can have multiple entities per interaction. This is very helpful when trying
	to narrow down the answers to a question.

**Dialog:** The conversation that is created within WCS, is called a dialog. A dialog is
	composed of creating a flow between intents and entities. The combination of flows and
	subFlows allows WCS to provide mult-layered conversation based on multiple interactions,
	instead of a single question and answer.


## Create Watson Conversation Service

1. Logon to BlueMix and go to your dashboard.
2. Click on the catalog button at the top navigation on the right
3. Select "Watson" on the left navigation under the Services menu
4. Click on "Conversation"

![Architecture Overview](/images/wk2-catalog-wcs.png)	 

5. In the service name type STSA-WK2-WCS-xxx (where xxx is your team number). You can leave credential name as is, if you like.
6. Click on the create in the lower right corner. You should now see the following

![Architecture Overview](/images/wk2-wcs-manage.png)

7. You will need to cut and paste your service credentials. Click on "Service Credentials"
on the left navigation, right under "Manage". Once the page has loaded, click on "View Credentials" on the right side of the screen.
A drop down will show with your specific credentials

![Architecture Overview](/images/wk2-wcs-credentials.png)
Your values for username and password will be different than shown.

8. Click the copy icon to copy the values to your clipboard. Paste the values in a text file for later use. Make sure you get both:

- username
- password

9. Go back to the manage page, by clicking on the manage link on the left side navigation. This will take you back to the manage page.

You have now completed the first step in creating the WCS service instance.

## Launch WCS tooling
1. Click on the "Launch tool" icon to take you to the WCS workspace

![Architecture Overview](/images/wk2-wcs-manage-launch.png)

2. This will open another browser window and you should now be on the Watson Conversation Workspace page

![Architecture Overview](/images/wk2-wcs-workspaces.png)

3. You are going to create a new workspace. Click on the ***"Create a new workspace"*** tile.
4. Enter a workspace name i.e. STSA-WK2-xxx (where xxx is your team number).

![Architecture Overview](/images/wk2-wcs-workspaces-create-popup.png)


5. Click the last icon (Back to workspaces) on the left navigation. It should look like

![Architecture Overview](/images/wk2-conversation-click-workspaces-small.png)

or

![Architecture Overview](/images/wk2-conversation-click-workspaces-big.png)

You should now see:

![Architecture Overview](/images/wk2-conversatons-workspaces-dash.png)

5. Click on the **three green buttons** in the upper right corner of your workspace tile.

![Architecture Overview](/images/wk2-conversation-workspace-tile.png)

You should now see

![Architecture Overview](/images/wk2-conversation-workspace-tile-submenu.png)

5. Click **View Details**. You should now see something like the following

![Architecture Overview](/images/wk2-conversation-workspace-tile-viewdetails.png)

5. Copy the **Workplace ID** and paste it somewhere for use later.

5. Click on the **White Back Arrow** in the upper corner of the tile.

![Architecture Overview](/images/wk2-conversation-workspace-detail-convid.png)

5. Click **Get Started** or anywhere in the tile, to get started.

![Architecture Overview](/images/wk2-conversation-workspace-tile-getstarted.png)

## Create Intents
You are now ready to create some **Intents**. Click on the Intents link. You should now see the following:

![Architecture Overview](/images/wks-wcs-workspaces-create-intents.png)

Click the **"Create New"** button in the middle of the page. You should now see the following:

![Architecture Overview](/images/wk2-wcs-workspaces-intents-add.png)

1. Add a new Intent name of **Information**.

![Architecture Overview](/images/wk2-conversation-intent-ex-information.png)


You then need to provide some examples. Copy the samples provided, one at a time, paste each into the tool.

```
What is the temperature
Can you explain
What is a
I need information about
What is the current temperature
I need some information on the temperature
Can you tell me the temperature
I need to know the temperature

```
It should look like the following:

![Architecture Overview](/images/wk2-wcs-workspaces-intents-examples.png)

Click **Done** in the upper right corner when finished.

![Architecture Overview](/images/wk2-wcs-workspaces-intents-done.png)

2. Add another intent, by clicking "Create new" button. Use the new Intent name of **Greeting**. Copy the samples and look at the image below.

```
good evening
Hello
Hi
Howdy
Good morning
Good afternoon
Yo Yo Yo
Sup dude
```
![Architecture Overview](/images/wk2-wcs-workspaces-intents-greeting.png)
Click **Done** when finished.

3. Add another intent **GoodBye**. Add the following examples also.

```
good-bye
see you later
talk to you later
goodbye
later
```
![Architecture Overview](/images/wk2-wcs-workspaces-intents-goodbye.png)
Click **Done** when finished.

4. Add one last intent **ChangeColor** with the following examples:

```
I need to change the color of the sensor
Please change the sensor color
Change the color of the sensor
make the sensors color green
change the sensor color to green

```

![Architecture Overview](/images/wk2-wcs-workspaces-intents-changecolor.png)
Click **Done** when finished.

You have now finished creating all of the intents. (Nice Job!)

## Create Entities
Next we want to create some **Entities**. Click on the "Entities" link at the top of the page.

![Architecture Overview](/images/wk2-wcs-workspaces-entities-add.png)

1. Click the **"Create New"** button.
1. Type **"Temperature"** as the new Entity name.

![Architecture Overview](/images/wk2-wcs-workspaces-entities-examples.png)

Now notice, you need to add examples of new "Temperature" entities. So add **"Current Temperature"** as a new entity
but also you need to provide some synonyms to help identify variations of the entity value. Now add **"Temperature now"** and **"Current Temp"**
as synonyms. You now need to add **"Average Temperature"** and **"Temperature"** with the corresponding
 synonyms, as shown in the image above. After you are finished with each set of synonyms click the **Plus** icon to add them.

```
Current temperature		current temp	temperature now
Average temperature		avg temp		avg temperature
Temperature 			Temp

```
Click **Done** when finished.

2. Create another entity called **"degree"** also add the associated synonyms.

```
Celsius			celcius		degrees in c	degrees C
Fahrenheit		Degrees F	degrees in f


```

![Architecture Overview](/images/wk2-wcs-workspaces-entities-degree.png)

Click **Done** when finished.

3. Add one last entity called **Colors** and the associated values.

```
Yellow
Blue
White
Green
Red
Black	off
```

![Architecture Overview](/images/wk2-wcs-workspaces-entities-colors.png)
Click **Done** when finished.

Great you now completed the adding of Entities. (Well done!).

## Create a Dialog
We are now ready to create a dialog. Click on the Dialog link at the top of the page.

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-add.png)

1. Click the **"Create"** button to start to create a dialog. You should now see the following:

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-start.png)

What you see are two "Dialog Nodes". The first is the standard "Welcome" message and the other
is a catch-all "Anything else" . Remember the way a conversation dialog works is by scanning from the top of the tree
and evaluating every node until a condition is met that satisfies the question being asked. So when the conversation starts
WCS will respond with the "Welcome" Node. If you click on the "Welcome" node you will see that the standard Watson response
is "Hello. How can I help you?"
![Architecture Overview](/images/wk2-wcs-workspaces-dialog-click-welcome.png)

2. We now want to create our own node, based on the "Intents" we create earlier. To add a new node in the tree, click on the "Welcome"
node,  and then click on the "Add Node" icon.

![Architecture Overview](/images/wk2-wcs-workspace-dialog-node-add.png)

You should now see the following:

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-add-input.png)

3. Type **Greetings** as the name of the node. The trigger should be **"#Greetings"**. The Response condition should be set to "True" and you can provide any response text you like. It should look similar to the following:

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-greeting.png)

4. Again create another root node by clicking on the **"Greetings"** node and then clicking **"Add node"**. This will add another node. This node's values are as follows:

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-goodbye.png)

5. Create a new node under the **"Goodbye"** node, called **Color Change** with the following information:

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-colorchange.png)

In this node, your response needs to have a condition set, prior to responding. This is done by clicking on the **Add response condition** link.

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-colorchange-response-condition.png)

Take special note of the extra information in the first response. Make sure to copy it properly.   

`I just changed the color to <? entities.Colors.literal ?>`   

This is an expression language snippet of code. If you look at the information in the node, the intent is to change color and the first condition for a response is "@Colors", which
signals to Watson, if a known color is provided in the user's input then use that color in the response.   

Now click on the **Add another response** to add another condition. Make the second response condition **True**.
If an unknown color is typed (i.e. Violet) in the user's input the processing will hit the "True" condition. This is because "Violet" is not in our "@Color" entity definition.

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-colorchange-response-condition-true.png)

6. Create a node under "Color Change" called **WhatElse**, with the following information:

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-whatelse.png)

7. We are now on the last node we are going to create. Insert this node between the "Goodbye" and "Color Change" node. This is done by clicking on the
goodbye node and then clicking the **"Add Node"**.

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-aftergoodbye.png)

8. Enter **"Information"** as the name of the node. In the "If bot recognizes" type the word "Information" you will see a type ahead pop up. Select the "#Information" entry. **Do not provide any responses.**

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-information.png)

9. Click the large green X at the top to close the dialog editor.
10. Now you want to create a sub-dialog. This time make sure the **"Information"** node is selected and then click on the **"Add child node"** button.

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-information-plussign.png)

11. Enter "Temperature" as the name of the node.
12. In the "If bot recognizes" field enter "@Temperature:(Current Temperature)". This is the entity defined earlier.

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-information-temperature.png)

13. In the Response section, for the first response, add a response condition. Type "@degree:Celsius" in the response condition field.
Then add the Watson response of "The current temperature in celsius is"
14. Add another response condition of "@degree:Fahrenheit" with the response message of "The current temperature in fahrenheit is".
15. Add one last response condition of "True". this is a catch all condition for this node. Add the Watson response as "The current temperature in fahrenheit is"
The reason for this last response to have a response in the event Watson doesn't knows what type of temperature you are looking for. For example,  Watson knows you are looking for information about the temperature, but isn't sure if you mean in celsius or fahrenheit.
Your completed node should look the like the following:

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-temperature.png)

Close the node editor, we have one last sub-node to create.

16. Under the "Temperature" node, create a new node. The is done by clicking on the "Temperature" node and then clicking on **Add node** button.

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-temperature-addnode.png)   

Call it **Unknown** Add the information like below:

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-information-unknown.png)

17. The next step is to go back to the "Information" node, by clicking on the node itself. Next click on the **Three green buttons** This will open a small menu like the following:   

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-information-submenu.png)

18. Click on the **Jump to** menu item and then click on the **Temperature** node. You should now see the following:   

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-information-temperature-submenu.png)

19. Click on the **If Bot recognized condition** menu item. You screen should now look like the following:   

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-information-temperature-jump.png)

20. Next we want to select the three circles on the "Temperature" node. Like with the "Information" node, select the  **Jump to** menu item.
Then select the **"WhatElse"** node. This time select the **"Respond"** option. The sub-flow should now look like the following:

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-information-temperature-jump-respond.png)

21. Next we want to select the three circles on the "Color Change" node. Like with the "Temperature" node, select the  **Jump to** menu item.
Then select the **"WhatElse"** node. This time select the **"Respond"** option. The sub-flow should now look like the following:

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-colorchange-jump.png)

22. Go back to the "Information" node and click on it to open the node editor. Click the three bubbles in the upper right.

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-temperature-json.png)

Then click on the **Open JSON Editor** menu item.

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-temperature-json-pulldown.png)

23. We are going to add a single line of code to the JSON. The purpose of this code, is to send a signal to the NodeRed application that some special processing needs to occur. So for Watson to respond to the **degree:Celsius** entity,request we need to have a new attribute on the JSON object. Type **"action": "CurrentTempCelsius"** make sure you put the **"comma"** right after the curly bracket, but before the new code you just added.

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-temperature-json-celsius.png)

24. You now need to do the same thing for the each of the other responses on the temperature node.
For the **degree:Fahrenheit** response add **"action": "CurrentTempFahrenheit"**

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-temperature-json-fahrenheit.png)

25. Now do it for the **True** condition as well. We will have the default response be in fahrenheit so add **"action": "CurrentTempFahrenheit"**

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-temperature-json-default.png)

26. The last step is to update the **ChangeColor** dialog node, with a similar action command. but this time set the command value to **"action":"changeColor"**

![Architecture Overview](/images/wk2-wcs-workspaces-dialog-node-changecolor-json.png)

We are now finished with the entire Dialog. Congratulations. We will be coming back here to make a couple of minor updates. Now is time to test your code.

## NodeRed Flow
What might become obvious is that the responses from WCS are static. There is nothing personalized in the responses.
Because of this we need to add a layer in front of WCS to take the responses and make them personalized. We are going to use NodeRed, like we did for the IoT lab.
Below are the steps needed for adding a new flow to your existing **BlueMix NodeRed** application.

1. Open your browser and go to the BlubMix NodeRed URL. It should be something like STSAWorkshops-xx.mybluemix.net. You can also get to it from your BlueMix Dashboard.
2. Login to your NodeRed Editor

	![Architecture Overview](/images/wk2-nodered-logon.png)
3. You should now see something like the following, which is your flow for IoT

	![Iot Flow](/images/wk2-nodered-flow-iot.png)
5. Click [WCS NodeRed Flow](https://github.com/jdcalus/STSA-Workshop-2-WCS/blob/master/wk2-wcs-workspace.json) link. This will take you to the NodeRed flow JSON file which has a pre-developed flow for talking to WCS. You should now see:

	![Iot Flow](/images/wk2-github-flowjson-small.png)

6. Highlight all of the JSON text (Triple click on the text) and copy it to your clipboard. (ctrl+c on your keyboard).
7. Go back to your NodeRed editor and in the upper right corner is the cake layer icon, click on it. Then click import and then click clipboard

	![Iot Flow](/images/wk2-nodered-import.png)

8. You should now see the following dialog window:

	![Iot Flow](/images/wk2-nodered-import-paste.png)
	click in the center of the dialog and paste the information from your clipboard. (ctrl+v). Make sure the **new flow** button is pressed before you import.
	Your screen should now look something like the following, which a new tab called **WCS Flow**

	![Iot Flow](/images/wk2-nodered-flow-wcs.png)

### Update NodeRed Flow

Now we need to update a couple of the nodes, with information that was provided earlier in IoT Workshop.

1. Make sure you have the credentials information from your DashDB service. This can be access from your **BlueMix dashboard**, by clicking on the service:

![Iot Flow](/images/wk2-bluemix-dashboard-dashdb.png)

You should see the manage page for that instance of the dashDB service

![Iot Flow](/images/wk2-bluemix-dashdb-manage.png)

Now click on the Service Credentials link on the left side. Once the page loads, there is an option for **View Credentials**.  You will need to copy the **HostName, Username and Password** in the next step.

![Iot Flow](/images/wk2-bluemix-dashdb-credentials.png)

2. Now go back to NodeRed and make sure you are in the **WCS flow**.

Click on the **Current Temp** node. This is a DashDB node, so we need to make sure the setting are configured properly. You should see something like:

![Iot Flow](/images/wk2-nodered-flow-wcs-dashdb-config.png)   

If you see the "Server" information from the IoT lab. Select it. If you don't see an option for the "Server" information from the IoT Lab, you will need to add the information. You do this by clicking on the **pencil** icon to set up the connection information. You should see something like

![Iot Flow](/images/wk2-nodered-flow-wcs-dashdb-blank-security.png)

Once you fill in the values it should look similar to:

![Iot Flow](/images/wk2-nodered-flow-wcs-dashdb-security.png)

Click the **update** button then click on the **Done** button. Your dashDB node should now have the proper information to connect to the service instance.

3. Now double click on the **IoT Change Color** node.

![Iot Flow](/images/wk2-nodered-flow-wcs-iotdb.png)

We need to make sure the connection information is correct. If you are unsure, go to your flow from **Lab 1** and look at the **IoT: Command** node.

![Iot Flow](/images/wk2-node-red-iotcommand-node.png)

4. The final step is to double click on the **STSA-CONV** node.

![Iot Flow](/images/wk2-nodered-flow-wcs-node.png)

This is the Watson Conversation node, that connects to the conversation you just created above.

![Iot Flow](/images/wk2-nodered-flow-wcs-node-wcs-config.png)

Provide the following from above

- Username
- Password
- Workspace


5. Click **Done** when completed. You are done updating the NodeRed Flows for WCS
6. Click the **DEPLOY** button at the top of the screen.

### Watson Conversation Web Client

The next step is to create a sample application that connects to your NodeRed flow. When you click on the button below, you will be taken to bluemix DevOps page. This will automatically create a new application for you, via the DevOps ToolChain. If you are not logged on to BlueMix you will need to logon.

1. Click the Deploy to BlueMix button below to provision the web client application.

	[![Deploy to BlueMix](https://bluemix.net/deploy/button.png)](https://bluemix.net/deploy?repository=https://github.com/jdcalus/STSA-WCS-WebProxy.git)

	You will see a screen like the following:

	![Iot Flow](/images/wk2-devops-autocreate.png)

2. Click the deploy button, after about 1 minute or so, but may be longer if BlueMix is slow :-) , your screen will change to something like the following:

	![Iot Flow](/images/wk2-devops-toolchain-autocreate.png)

3. Click on the **Deliver** tile

	![Iot Flow](/images/wk2-devops-toolchain-deliver-icon.png)

	Then you should see something like the following:

	![Iot Flow](/images/wk2-devops-toolchain-deliver-home.png)

4. Once the message in the deploy tile has changed to finished from Deploy running, click on the **Rocket** icon under **Last Executed Results**

	![Iot Flow](/images/wk2-devops-toolchain-deliver-deploy-tile.png)

	This will take you to the newly created applications. Your screen should now look like:

	![Iot Flow](/images/wk2-bluemix-wcs-webclient.png)

5. Click on the **Runtime** link on the left and your screen should change, then click on environment tab in the middle of the screen. It should look like the following:

	![Iot Flow](/images/wk2-bluemix-wcs-webclient-runtime.png)

6. In the **CONVERSATION_URL** value change the "xxxxxx" to the hostname of your **NodeRed** application. This comes from the NodeRed url that where you are creating your NodeRed flow. So something like "STSAWorkshops-xx.mybluemix.net", do not include "/red/#

7. Click the **Save** button. The application will restart.

8. Click on the **Visit App URL**. This is at the top of the page. **You will get an error message**. That is okay. You need to add **webclient** to the end of the browsers URL.

	![Iot Flow](/images/wk2-bluemix-wcs-webclient-url.png)

	Your screen should now look like the following:

	![Iot Flow](/images/wk2-bluemix-wcs-webclient-webpage.png)


### Ask Watson questions
You can now ask Watson questions about the temperature.

1. What is the current temperature in Celsius
2. What is the temperature
3. What is the current temperature in fahrenheit
4. Change the background color to blue

Nice Job you are now a Jedi Knight in Watson Conversation Service!

### Jedi Knights and Jedi Masters
If you have time, you can start to add more Dialog Nodes to ask other questions and you can provide the appropriate responses. Play with the ordering of the nodes in the dialog tree. Create new child nodes and see how they react to the flow. Also you can always add new entities and intents and then create even more dialog nodes. It is really endless as to what you can do.

If you have a hard time thinking of what else you can do, don't worry, in the Hybrid cloud and Blockchain labs you will have more opportunities to add more dialog nodes.
