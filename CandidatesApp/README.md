# 1. First things first: Lets set up your Bluemix accounts!

1. http://ace.ng.bluemix.net  
This will take care of your _Sign Up_ and _Log In_ into the Bluemix portal.  

# 2. Get to know our App

### What is our App?
We want to try to figure out the popularity of the Candidates - "Hillary Clinton" and "Jeb Bush", based on the sentiments of the *live* tweets concerning them. All this with minimal _(almost none)_ code and with a drag-drop flow diagram feature that is up and running within minutes.  

Here is a screenshot of the app that you will be making and hosting on the cloud, powered by the SQLDB cloud database:
![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/CandidatesApp/images/bar_chart_bush.bmp)

Your results will look different, as this is based on live twitter data. You can see that on the particular day when this screenshot was taken, tweets about Jeb Bush were trending negative.

In this tutorial we will show you how to define an application flow using Node Red and how to store the tweets in a SQLDB service.  Then using SQL queries we create a REST API which
provides the data that we need to render the bar chart in HTML / javascript.

### The app requires Node-RED boilerplate and SQLDB service from Bluemix.
A [quick introduction to Node-RED](http://nodered.org) would be handy. Nevertheless, we can pick the skills on the go.

# 3. Let's get started!

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
* The App will now **RESTAGE**, which should take a minute. When done, your SQLDB service is now bound with the Node-RED App and should show up on the App Overview page.  

####We are done with the setup of the Node-RED app and the SQLDB service. That was pretty easy.

# 4. Let's create our Node-RED flow!

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

	CREATE TABLE JEBBUSH   
	(
	  LOCATION VARCHAR(100),
	  SCREENNAME VARCHAR(50),
	  TWEET VARCHAR(300),
	  SENTIMENT DECIMAL(5,2),
	  TS TIMESTAMP  
	);
	```
* You can see two new tables showing up in the left panel. 
![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/CandidatesApp/images/created_tables_sqldb.bmp)  
We are done with our work here. Lets jump to the interesting Node-RED part now!	

#### The Node-RED flow

* From the App overview page, click on the link next to **Routes** right beneath the App name.
* This will land you on the Node-RED in BlueMix page. Click on the red button **Go to your Node-RED flow editor**
  
*The left panel of this page includes all the different nodes available for direct use to plug and play within your flow structures. The main area called the 'sheet' is where you pull in the nodes and connect them to each other to make a functional app. On the right panel, 'info' displays the information about a particular node when selected and the 'debug' tab is the space where we validate and visualize the state of data flowing at any time within the flow.*

* On the top right corner, click the icon:
![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/CandidatesApp/images/%245A2C55CE3129CFC8.bmp)
 and select **Import** > **Clipboard**  
* In the textbox, paste the content of the file **CandidatesApp_Import_Sheet1.txt** in the folder **files** of this project. Click **Ok**  
![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/CandidatesApp/images/node_flow.bmp)

* ####Set the Twitter credentials  
Double click on the blue Twitter node "Hillary Clinton" and setup the "Login as" using the icon next to it. You will need to have a Twitter account to authenticate to the Twitter service.
![alt text](https://raw.githubusercontent.com/CDSLab/IDUG2015/master/CandidatesApp/images/edit_twitter_in_node.bmp)  
Repeat the same process for the other Twitter node "Jeb Bush".

* ####Point the SQLDB nodes to your service.  Since you have created a new SQLDB service and bound it to your application, you need to tell each of the 3 SQLDB nodes in your flow
the name of the service.  Open each SQLDB node by double clicking and select the service name from the dropdown and click OK.

* This imports the ready-to-use flow structure that you can use for this app. On the top right, click **Deploy**.  
* In the **debug** panel on the right, you would start seeing the live tweets that are going to be saved in the SQLDB tables that we set up before. 
* Go back to the SQLDB console and validate the records being inserted into the tables we created.  
* Now, click the icon on the top right corner of the middle panel to create a new sheet in the flow editor.
* From the **Import** > **Clipboard** textbox as earlier, import the contents of the file **CandidatesApp_Import_Sheet2.txt**.
* Click **Deploy**

### We have created the Node-RED flow required for the App. 

*Let's take a closer look.*  The cool thing here is that we are able to quickly and easily build our application logic for a cloud-driven data application without actually writing much code.
First let's look at sheet 1.  Each of the twitter nodes (one for each candidate) flow into a sentiment node where a sentiment score is assigned to the tweet based on the positive or negative emotion of the words in the tweet.  Then we format the data (mapping JSON paths in the msg object to columns in a SQL table) and insert into our SQLDB database.  Tweets about Hillary Clinton are stored in the HILLARYCLINTON table along with their sentiment score and some metadata (location, timestamp, screenname), and tweets about Jeb Bush go into the JEBBUSH table.

You might notice at the bottom of the Bush flow that we have some extra logic.  This is an alerting system that sends a notification when a top tweet for the day is received about Jeb Bush.  How does it do this?  Well, there are 2 types of SQLDB nodes that you can work with in Node Red, output nodes (which we used to write to the HILLARYCLINTON and JEBBUSH tables), and query nodes.  After the sentiment analysis is done for Jeb Bush, we fork the flow - one end goes to the database, and the other is fed into the query node.  The query node takes the incoming sentiment score from the flow and compares it against all existing sentiment scores in the database.  It does this with the following SQL query: "select count(*) as bettertweets from jebbush  where sentiment >= ?".  This query basically says: give me the count of any tweets existing in the database timestamped today that are better than the one I have coming in.  Then the result of that query flows into a filter node which checks if that count is 0 or not.  If the count is not 0, then the flow ends there.  If the count is 0, then you know there are no better tweets and this newly added tweet must be the highest sentiment score of the day.  In such a case, you might want to send an alert as an SMS text message (using the twilio node), or send an email, or even tweet about it -- all of these are options available using nodes in Node Red.  For now, the flow will print to the debug console.  

Next let's inspect sheet 2.  Using the SQLDB query nodes, we implement a simple REST API.  Each REST API has a http intput node on the left feeding into the SQLDB query node, and an http response node on the right.  A get request comes in which triggers a query on the database which is then returned in JSON format as the response.  To see the queries that power the REST API, simply double click on the SQLDB node and you can see the query that is issued against your SQLDB cloud database.  You can see the output of the REST API in JSON format by going to these URL's:  
http://idugdemo.mybluemix.net/bushspread  
http://idugdemo.mybluemix.net/clintonspread  
This is the data that is being used to create the UI.  The HTML / javascript that is used to render the UI is served up inside a template node labeled "HTML" -- it is also wrapped inside a HTML request/response flow.

### Time to see the results. 

* Open a new tab. Log on to **"Your App name".mybluemix.net/bar?q=bush**
* You can view the Bar graph of the Sentiment Spread of tweets about Jeb Bush. Click on any of the bar and you can view a sample of the tweets pertaining to that sentiment score.
* Try the same with **"Your App name".mybluemix.net/bar?q=clinton**  

# Within a few minutes, you have created a Sentiment Graph about the Candidates for US elections based on the live Twitter feed!

  
 
 


 






