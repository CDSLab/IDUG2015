# 1. First things first: Lets set up your Bluemix accounts!

1. http://ace.ng.bluemix.net  
This will take care of your _Sign Up_ and _Log In_ into the Bluemix portal.  

# 2. Get to know our App

### What is our App?
We want to leverage Twitter's AnomalyDetection R package to detect anomalies in any kind of data. In this case, the data will the Humidity and Temperature values sent from a device like your own cell phone. Finally we want to have an alert system to notify of the anomaly in the data through text messaging.  

### The app requires Node-RED boilerplate and DashDB service from Bluemix.
A [quick introduction to Node-RED](http://nodered.org) would be handy. Nevertheless, we can pick the skills on the go.

# 3. Lets get started!

####Set up the Node-RED boilerplate and DashDB service
* Login to your Bluemix account.  
What you see here is the list of your *Applications* and *Services* in Bluemix. We are going to create a Node-RED boilerplate application and *bind* a SQLDB service to this application.  
  
*Now, a Boilerplate can be imagined as a bundled Application with relevant bound Services for a particular targeted usage to get up and running quickly.*  
* In the upper menu bar, go to **CATALOG**  
  
*This page lists all kinds of different boilerplates, runtimes and services that are offered by Bluemix. Browsing through this page can give you an idea about all the exciting features offered by Bluemix.*   
* From the Boilerplates section, select the **Node-RED starter** boilerplate.  
  
*Here you can see the SDK for Node.JS and the two services - Cloudant and Monitoring & Analytics that come as a part of the Node-RED boilerplate. For our app, we are not going to use these bundled services. Instead we will bind our own DashDB service.*  
* Provide a **unique name** to the app on the right side of the page and click **CREATE**. A non-unique name would notify and urge you to think of a more creative name.  
* In a couple of minutes, your application will be staging and ready to run. 

*You will land up on the 'Start coding with Node-RED' page. The instructions on this page are to get familiar with Cloud Foundry command line interface to control your application. You can go through the steps and try them out at leisure. We won't be needing that for our app right now.*  
* On the left panel, click on **Overview**. This is the overview of your Application showing the link to go to the App, status of the app, instances, memory and the services bound to it.  
* We want to bind the DashDB service to the App. Lets click on the **ADD A SERVICE OR API** button.
* Search for DashDB in the search tab or select the **dashDB** service in the Big Data section.
* In the right hand side panel, the App name would be pre-filled and the service name would be randomly generated. The selected plan should be defaulted to Entry plan. Click **CREATE**.   
* The App would be required to **RESTAGE**. Your DashDB service is now bound with the Node-RED App and it should show up on the App Overview page.  
* For authentication purposes for the DashDB service further on, you would require the username and password at several places. This can found by clicking on the **Show Credentials** text on the left bottom part of the SQL Database block. In the textbox that shows up, make a note of the **username** and **password** among the various attributes. You can always refer back to this whenever required.  

####We are done with the setup of the Node-RED app and the DashDB service. That was pretty easy.

# 4. Lets create our Node-RED flow!

#### A little homework on the DashDB side

We need to create a table in our DashDB for storing the data for our App. Lets get that done quickly.

* On the overview page, click on the DashDB service block in the services section. Further, click on the **LAUNCH** button on the upper right corner of the page. This will open up the SQLDB console in a new tab.  
* Click on **Tables** on the left panel.  
* In the schema and table name panel, click on the small little circular **+** symbol. This will enable you to create a new table.  
* In the right side textbox paste the following DDL statement and click **Run DDL**

	``` sql
	CREATE TABLE IDUGIOTDEMO   
	(
	  TS TIMESTAMP,
	  HUMIDITY DECIMAL(5,2),
      TEMP DECIMAL(5,2)	  
	);
	```
* You can see the new table showing up in the left panel. We are done with our work here. Lets jump to the interesting Node-RED part now!	

#### The Node-RED flow

* From the App overview page, click on the link next to **Routes** right beneath the App name.
* This will land you on the Node-RED in BlueMix page. Click on the red button **Go to your Node-RED flow editor**
  
*The left panel of this page includes all the different nodes available for direct use to plug and play within your flow structures. The main area called the 'sheet' is where you pull in the nodes and connect them to each other to make a functional app. On the right panel, 'info' displays the information about a particular node when selected and the 'debug' tab is the space where we validate and visualize the state of data flowing at any time within the flow.*

* On the top right corner, click the icon:
![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/CandidatesApp/images/%245A2C55CE3129CFC8.bmp)
 and select **Import** > **Clipboard**  
* In the textbox, paste the content of the file **AnomalyApp_Import_Sheet1.txt** in the folder **node-RED import files** of this project. Click **Ok**  

* This imports the ready-to-use flow structure that you can use for this App. 

* Device ID setup (*You can use your cell phone too for more convenience!*) 
  * Log on to http://www.tinyurl.com/idugiot in a browser. Make note of the Device ID on the top right corner.  
  
  ![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/AnomalyApp/images/Sensor.bmp)  
  You can control the temperature and humidity data through the up and down arrow keys.  
  
  * Double click the node "Smart Transformer"  
  Enter the device ID from the browser you opened above **without colons**. Click Ok. 
  
  ![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/AnomalyApp/images/%241849374E299CB19B.bmp)  
  
  




* **Twilio setup**
  Sign up on http://www.twilio.com and fetch the Account SID and Auth Token from your account.  
  Double click the Twilio node and enter the information required.  
  ![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/AnomalyApp/images/Add_Twilio.bmp)  
  ![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/AnomalyApp/images/Add_Twilio_2.bmp)

* Open up the node *"Login"* and update the DashDB username and password selecting *Basic Authentication*.  
![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/AnomalyApp/images/Login_R_REST_Call.bmp)  
  

* Open up the node *"R Script Payload"* and update the schema with your DashDB username.  
![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/AnomalyApp/images/R_script_payload.bmp)  



On the top right, click **Deploy**.  
* In the **debug** panel on the right, you would start seeing the temperature and humidity data coming from your phone. 
* Go back to the DashDB console and validate the records being inserted into the table we created.  

* Click **Deploy**

* The debug console shows the Anomaly Detection output from the R package running on the data captured from your phone. Try to vary the data being sent from your phone so as to create an anomaly in the data and it should be reflected in the output and sent to you as a text message on your phone.

### We have created the Node-RED flow required for the App. Time to see the results. 

# Within a few minutes, you have a Data Anomaly Detection App that is able to send alerts to your phone!!!

  
 
 


 






