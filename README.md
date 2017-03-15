# Python API Guide (and how it applies to Magento BI)
----
## Table of Contents
**This repository contains:**

1. Guides/videos on learning the terminal and API (GET, POST requests)

2. Learning about cURL and how to send/retrieve data

3. Using the terminal to interact with Python and RJMetrics

4. Exercises on exporting data from APIs and an RJMetrics dashboard

5. Exercises on importing data into RJMetrics using APIs

6. Creating cron jobs to automate python scripts

7. Generating updated raw data exports & requesting new data through python

8. Load the generated report (which is a .csv) back into python and transform into a json format

9. Parse specific dimensions/columns from reports while keeping the same json format

----
## Section 1: First, Learning about the Terminal and APIs
### A. The Terminal

This [video](https://www.youtube.com/watch?v=jDINUSK7rXE) provides basic knowledge on how to navigate through the terminal  and how to use the command line.
Once completed, try completing the following quizzes. You may need to use Google to find some of the answers.

* [Quiz 1](http://www.ch.embnet.org/CoursEMBnet/Exercises/Quiz/quix1.html)

* [Quiz 2](http://www.ch.embnet.org/CoursEMBnet/Exercises/Quiz/quix2.html)

* [Quiz 3](http://www.ch.embnet.org/CoursEMBnet/Exercises/Quiz/quix3.html)

### B. An API Overview

What is an API and what can you do with it? Review the following videos and guides to get a better understanding.

* [What is REST?](http://www.restapitutorial.com/lessons/whatisrest.html)

* [REST API concepts and examples (youtube)](https://www.youtube.com/watch?v=7YcW25PHnAA)

* [HTTP Methods: GET vs. POST](https://www.w3schools.com/tags/ref_httpmethods.asp)

### C. Making cURL requests

Now that you've learned about APIs, find out how to transfer data in the terminal using a cURL command.

* [How to use the cURL command (youtube)](https://www.youtube.com/watch?v=WxUVU0b95Oc)

* [An intro to cURL](https://gist.github.com/joyrexus/85bf6b02979d8a7b0308)

* [cURL examples](https://www.rosehosting.com/blog/curl-command-examples/)

### D. Making API requests

Here's a brief introduction to Python

* [Python Guide on Requests](http://docs.python-requests.org/en/master/)

* [Coding with Python, API basics to grab data (youtube)](https://www.youtube.com/watch?v=pxofwuWTs7c)

* [More info on the Python library: urllib2](https://docs.python.org/2/library/urllib2.html)

----
# Section 2: Exercises
### Solutions are in a separate markdown file. Try not to peak without first attempting!

## Homework 1

### Part one:
Check out the [Open Philly Indego Bikes API](https://www.opendataphilly.org/dataset/bike-share-stations) and get a sense of what it does. 

Then try the following:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1. Make a single request to the API**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*Hint: Use the [requests](http://docs.python-requests.org/en/master/user/quickstart/) library*

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2. Save the output to a .txt file**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**3. Format the output in a pretty json format (You should be able to do this with one line of code)**


### Part two:
Check out the Help Center article on [export APIs](https://support.rjmetrics.com/hc/en-us/articles/204674465-Automating-data-retrieval-with-the-Data-Export-API) in RJMetrics and then try the following.
     

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1. Export a specific report (in RJMetrics); save the contents to a .txt file**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2. Export a .csv through a raw data export (try doing this in both the terminal and python)**

*Note: You'll need to create a report and a raw data export to be able to do this. You cannot use a tabular report.*

This will require some googling to understand. Try to understand what you're doing along the way, and not just copy and paste indiscriminately.

Good luck!

----
## Homework 2: Import API Exercise

Now that you know how to export data, now try importing into RJMetrics. You can reference this [python article](http://docs.python-requests.org/en/master/user/quickstart/).

Also, read this [Developers Article](http://developers.rjmetrics.com/cloudbi/api.html) as well as this [Help Center Article](https://support.rjmetrics.com/hc/en-us/articles/204674775-Using-the-CloudBI-Import-API) for details on how to get authenticated with the Data Import API.

**Task:**

For this exercise, try importing only one line of data such as:

```
data = {
  "keys": ["id"],
  "id": 1,
  "email": "joe@schmo.com",
  "status": "pending",
  "created_at": "2012-08-01 14:22:32"
}
```

----
## Homework 3: Importing multiple records

You now know how to send one data point. How do you send more than that?

**Task:**

Send multiple records to the RJMetrics API using a for loop.
For example, something like this:

```
data1 = [{
#   "keys": ["id"],
  "id": 1,
  "email": "joe@schmo.com",
  "status": "pending",
  "created_at": "2012-08-01 14:22:32"
},{
  "id": 2,
  "email": "anne@schmo.com",
  "status": "pending",
  "created_at": "2012-08-03 23:12:30"
},{
  "id": 1,
  "email": "joe@schmo.com",
  "status": "complete",
  "created_at": "2012-08-05 04:51:02"
}]
```

----
## Homework 4: Importing indegoBike data into RJMetrics

Now that you're comfortable using a for loop to import multiple lines of data, it's time to request and import real data. 

**Task:** 

* **Part One:** Write a script which requests [indegoBike](https://www.rideindego.com/stations/json/) data and post it to the RJMetrics API. *The data contains several nests. We are only looking to import data contained in **properties**.*
* **Part Two:** Import the data into RJMetrics as a new table
* **Part Three:** Amend the script to add the current timestamp to each record. e.g., "time": "2017-02-24 00:00:00"

This will take you more time than previous assignments

*Hint*

For the third bullet, you will need to set a primary key. Will any of the columns work?

----
## Homework 5: Adding a cron job

What if you wanted to complete the task of Homework 4 at each hour of the day?

This is where you will need to set up a cron job

**Task:**

Find out more about cron jobs on Google and set one up through the terminal
Have it run at each hour

*Note: This will not work if you've been using a notebook style editor such as Jupyter Notebooks. You will need to execute python files (.py) through the terminal.*

----
## Homework 6: Generating fresh reports

A raw data export is useful for clients as they can view data in RJMetrics as a .csv. The issue is that a raw export only takes a snapshot of the table based on when the export was created. 

We've already learned how to export an exisiting raw data export using the export API. Can you figure out how to generate 'fresh exports' using the export API? Use the current [Help Center article](https://support.rjmetrics.com/hc/en-us/articles/204674465-Automating-data-retrieval-with-the-Data-Export-API) for clues.

**Task:**

* **Part One:** Create an initial data export in the front end
* **Part Two:** Send a POST request to the export API to generate an updated copy of the initial data export
* **Part Three:** Obtain the updated export's export_id
* **Part Four** Send a POST request containing the new export_id and download the zip file

*Note: There may be a delay for the newly created export ID to generate. How do you account for this when trying to complete this assignment in one python file?*

*Your code should work seemlessly after entering the intial table id (i.e. not having to insert the export id each execution)*

----
## Homework 7: Continuation of Homework 6

Now that you have downloaded the new export as a zip and extracted the csv, write a script that imports the csv into python and transform it into a JSON format.

*The application here is streamlining a way for clients to extract data out of Magento BI into another integration they use.*

### Bonus:
Many integrations (RJMetrics included) place a cap on the maximum number of requests that can be sent at a time. Can you come up with a way to get around this by sending requests in batches?

----
## Homework 8: Parsing data

You should be pretty comfortable now importing and exporting data to/from APIs. Now try exporting a revenue report and extract the date and revenue values.

The final format of the extracted data should be in a list of dictionaries. 

