# 1. First things first: Lets set up your Bluemix accounts!

1. http://ace.ng.bluemix.net  
This will take care of your _Sign Up_ and _Log In_ into the Bluemix portal.  

# 2. Get to know our App

### What is our App?
We want to try to figure out the popularity of the Candidates - "Hillary Clinton" and "Jeb Bush", based on the sentiments of the *live* tweets concerning them. All this with minimal _(almost none)_ code and with a drag-drop flow diagram feature that is up and running within minutes.  

### The app requires Node-RED boilerplate and SQLDB service from Bluemix.
A [quick introduction to Node-RED](http://nodered.org) would be handy. Nevertheless, we can pick the skills on the go.

# 3. Lets get started!

####Set up the Node-RED boilerplate and SQLDB service
* Login to your Bluemix account.  
What you see here is the list of your *Applications* and *Services* in Bluemix. We are going to create a Node-RED boilerplate application and *bind* a SQLDB service to this application.  
  
*Now, a Boilerplate can be imagined as a bundled Application with relevant bound Services for a particular targeted usage to get up and running quickly.*  
* In the upper menu bar, go to **CATALOG**  
  
*This page lists all kinds of different boilerplates, runtimes and services that are offered by Bluemix. Browsing through this page can give you an idea about all the exciting features offered by Bluemix.*   
* From the Boilerplates section, select the **Node-RED starter** boilerplate.  
  
*Here you can see the SDK for Node.JS and the two services - Cloudant and Monitoring & Analytics that come as a part of the Node-RED boilerplate. For our app, we are not going to use these bundled services. Instead we will bind our own SQLDB service.*  
* Provide a **unique name** to the app on the right side of the page and click **CREATE**. A non-unique name would notify and urge you to think of a more creative name.  
* In a couple of minutes, your application will be staging and ready to run. 

*You will land up on the 'Start coding with Node-RED' page. The instructions on this page are to get familiar with Cloud Foundry command line interface to control your application. You can go through the steps and try them out at leisure. We won't be needing that for our app right now.*  
* On the left panel, click on **Overview**. This is the overview of your Application showing the link to go to the App, status of the app, instances, memory and the services bound to it.  
* We want to bind the SQLDB service to the App. Lets click on the **ADD A SERVICE OR API** button.
* Search for SQL Database in the search tab or select the **SQL Database** service in the Data Management section.
* In the right hand side panel, the App name would be pre-filled and the service name would be randomly generated. The selected plan should be defaulted to Free plan. Click **CREATE**.   
* The App would be required to **RESTAGE**. Your SQLDB service is now bound with the Node-RED App and it should show up on the App Overview page.  
* For authentication purposes for the SQLDB service further on, you would require the username and password at several places. This can found by clicking on the **Show Credentials** text on the left bottom part of the SQL Database block. In the textbox that shows up, make a note of the **username** and **password** among the various attributes. You can always refer back to this whenever required.  

####We are done with the setup of the Node-RED app and the SQLDB service. That was pretty easy.

# 4. Lets create our Node-RED flow!

#### A little homework on the SQLDB side

We need to create two tables in our SQL Database for storing the data for our App. Lets get that done quickly.

* On the overview page, click on the SQL-Database service block in the services section. Further, click on the **LAUNCH** button on the upper right corner of the page. This will open up the SQLDB console in a new tab.  
* Click on **Work with Tables**.  
* In the left panel, click on the small little circular **+** symbol. This will enable you to create a new table.  
* In the right side textbox paste the following DDL statement and click **Run DDL**

	``` sql
	CREATE TABLE HILLARYCLINTON   
	(
	  LOCATION VARCHAR(100),
	  SCREENNAME VARCHAR(50),
	  TWEET VARCHAR(300),
	  SENTIMENT DECIMAL(5,2),
	  TS TIMESTAMP  
	);
	```
* Repeat the above process for another table JEBBUSH.

	``` sql
	CREATE TABLE JEBBUSH   
	(
	  LOCATION VARCHAR(100),
	  SCREENNAME VARCHAR(50),
	  TWEET VARCHAR(300),
	  SENTIMENT DECIMAL(5,2),
	  TS TIMESTAMP  
	);
	```
* You can see two new tables showing up in the left panel. We are done with our work here. Lets jump to the interesting Node-RED part now!	

#### The Node-RED flow

* From the App overview page, click on the link next to **Routes** right beneath the App name.
* This will land you on the Node-RED in BlueMix page. Click on the red button **Go to your Node-RED flow editor**
  
*The left panel of this page includes all the different nodes available for direct use to plug and play within your flow structures. The main area called the 'sheet' is where you pull in the nodes and connect them to each other to make a functional app. On the right panel, 'info' displays the information about a particular node when selected and the 'debug' tab is the space where we validate and visualize the state of data flowing at any time within the flow.*

* On the top right corner, click the icon:
![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/CandidatesApp/images/%245A2C55CE3129CFC8.bmp)
 and select **Import** > **Clipboard**  
* In the textbox, paste the content of the file **CandidatesApp_Import_Sheet1.txt** in the folder **node-RED import files** of this project. Click **Ok**  

* ####Set the Twitter credentials  
Double click on the blue Twitter node "Hillary Clinton" and setup the "Login as" using the icon next to it. You will need to have a Twitter account to authenticate to the Twitter service.
![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/CandidatesApp/images/edit_twitter_in_node.bmp)  
Repeat the same process for the other Twitter node "Jeb Bush".


* This imports the ready-to-use flow structure that you can use for this app. On the top right, click **Deploy**.  
* In the **debug** panel on the right, you would start seeing the live tweets that are going to be saved in the SQLDB tables that we set up before. 
* Go back to the SQLDB console and validate the records being inserted into the tables we created.  
* Now, click the icon on the top right corner of the middle panel to create a new sheet in the flow editor.
* From the **Import** > **Clipboard** textbox as earlier, import the contents of the file **CandidatesApp_Import_Sheet2.txt**.
* Click **Deploy**

### We have created the Node-RED flow required for the App. Time to see the results. 

* Open a new tab. Log on to **"Your App name".mybluemix.net/bar?q=bush**
* You can view the Bar graph of the Sentiment Spread of tweets about Jeb Bush. Click on any of the bar and you can view the tweets pertaining to that Sentiment rank.
* Try the same with **"Your App name".mybluemix.net/bar?q=clinton**

# Within a few minutes, you have a Sentiment Graph about the Candidates for US elections based on the live Twitter feed!!!

  
 
 


 






