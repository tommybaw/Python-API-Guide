# Python API Guide (and how it applies to RJMetrics)
This repository contains:
* Guides/videos on learning the terminal and API (GET, POST requests)
* Learning about cURL and how to send/retrieve data
* Using the terminal to interact with Python and RJMetrics
* Exercises on exporting data from APIs and an RJMetrics dashboard
* Exercises on importing data into RJMetrics using APIs


* [PENDING] cron jobs

----
## Section 1: First, Learning about the Terminal and APIs
### A. The Terminal

This [video](https://www.youtube.com/watch?v=jDINUSK7rXE) provides basic knowledge on terminal usage and how to use the command line
Once completed, try completing the following quizzes. You may need to use Google to find some of the answers.

[Quiz 1](http://www.ch.embnet.org/CoursEMBnet/Exercises/Quiz/quix1.html)

[Quiz 2](http://www.ch.embnet.org/CoursEMBnet/Exercises/Quiz/quix2.html)

[Quiz 3](http://www.ch.embnet.org/CoursEMBnet/Exercises/Quiz/quix3.html)

### B. An API Overview

What is an API and what can you do with it? Review the following videos and guides to get a better understanding.

[What is REST?](http://www.restapitutorial.com/lessons/whatisrest.html)

[REST API concepts and examples (youtube)](https://www.youtube.com/watch?v=7YcW25PHnAA)

[HTTP Methods: GET vs. POST](https://www.w3schools.com/tags/ref_httpmethods.asp)

### C. Making cURL requests

Now that you've learned about APIs, find out how to transfer data in the terminal using a cURL command

[How to use the cURL command (youtube)](https://www.youtube.com/watch?v=WxUVU0b95Oc)

[An intro to cURL](https://gist.github.com/joyrexus/85bf6b02979d8a7b0308)

[cURL examples](https://www.rosehosting.com/blog/curl-command-examples/)

### D. Making API requests

Here's a brief introduction to Python

[Python Guide on Requests](http://docs.python-requests.org/en/master/)

[Coding with Python, API basics to grab data (youtube)](https://www.youtube.com/watch?v=pxofwuWTs7c)

[Python library: urllib2](https://docs.python.org/2/library/urllib2.html)

----
## Section 2: Exercises

### Homework 1

**Part one:**
Check out the [Open Philly Indego Bikes API](https://www.opendataphilly.org/dataset/bike-share-stations). 
Get a sense of what it does. 

Then try the following:

**1. Make a single request to the API**


<details>
<summary> *Answer* </summary>
```
#Libraries used
import requests
import json
import urllib2
import pandas as pd
import zipfile
from StringIO import StringIO

url = 'https://api.phila.gov/bike-share-stations/v1'

headers = requests.utils.default_headers()

headers.update(
    {
        'User-Agent': 'Random User',
    }
)
print headers
response = requests.get(url, headers=headers)
```
</details>


**2. Save the output to a .txt file**

<details>
<summary> Answer </summary>
```
mydata = json.loads(response.text)
file = open("indego.txt", "w")  #to create an empty file?? "w" = writing
file.write(json.dumps(mydata, indent=4)) #json formatting
file.close()
```
</details>

**3. Format the output in a pretty json format (You should be able to do this with one line of code)**
Reference the file.write command above

**Part two:**
Check out the export API [support docs](https://support.rjmetrics.com/hc/en-us/articles/204674465-Automating-data-retrieval-with-the-Data-Export-API) for RJMetrics.
     
**1. Export a specific report (in RJMetrics); save the contents to a .txt file**

<details>
<summary>*Answer in Terminal*</summary>
```
curl -H "X-RJM-API-Key: *your_key*" https://api.rjmetrics.com/0.1/figure/*figure_id*/export -o "part2.txt"
```
</details>

**2. Export a .csv through a raw data export (try doing this in both the terminal and python)**

<details>
<summary>*Answer in Python* </summary>
```
url1 = 'https://api.rjmetrics.com/0.1/export/117749'

h = {'X-RJM-API-Key': '*Insert Generated Key*'}
response1 = requests.get(url1, headers=h)
print response1      

with open("example.zip", 'w') as f:
    f.write(response1.content)
zip = zipfile.ZipFile('example.zip')
zip.extractall()
```
</details>

<details> 
<summary> *Answer in Terminal* </summary>
```
curl -H "X-RJM-API-Key: *your_key*" https://api.rjmetrics.com/0.1/export/*table_id* > "part2-1.zip"
unzip exercise2.zip
```
</details>

*Note: You'll need to create a report and a raw data export to be able to do this. You cannot use a tabular report.*

This will require some googling to understand. Try to understand what you're doing along the way, and not just copy and paste indiscriminately.

Good luck!

### Homework 2: Import API Exercise


Now that you know how to export data, now try importing into RJMetrics. You can reference this [python article](http://docs.python-requests.org/en/master/user/quickstart/).

Also, read this [Developers Article](http://developers.rjmetrics.com/cloudbi/api.html) as well as this [Help Center Article](https://support.rjmetrics.com/hc/en-us/articles/204674775-Using-the-CloudBI-Import-API) for details on how to get authenticated with the Data Import API.

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

<details> 
  <summary>*Answer in Python* </summary>
```
url = 'https://connect.rjmetrics.com/v2/client/*client_id*/table/*table_name*/data?apikey=*your_key*'

h = {'Content-type': 'application/json'}
response1 = requests.post(url, headers = h, json=data)
print response1.content 
```
</details>



### Homework 3: Importing multiple records

**Part one:**
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

<details>
<summary> *Answer* </summary>
```
import requests
import json

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

url = 'https://connect.rjmetrics.com/v2/client/*client_id*/table/*table_name*/data?apikey=*your_key*'
h = {'Content-type': 'application/json'}

for i in data1:
    i.update({"keys": ["id"]})
    response = requests.post(url, headers = h, json=i)
    print response.content
print data1
```
</details>


### Homework 4: Importing indegoBike data into RJMetrics

Now that you're comfortable using a for loop to import multiple lines of data, it's time to request and import real data. 

Task: 
* Write a script which requests the [indegoBike](https://www.rideindego.com/stations/json/) data and post it to the RJMetrics API.
 * The data contains several nests. We are only looking to import data contained in **properties**.
* Amend the script to add the current timestamp to each record. e.g., "time": "2017-02-24 00:00:00"

This will take you more time than previous assignments




