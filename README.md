# Python API Guide (and how it applies to RJMetrics)
A repository containing scripts &amp; materials regarding APIs and the interaction with Python

----
## 1. First, Learning about the Terminal and APIs
### Learning the terminal
The following video provides basic terminal usage and how to use the command line
https://www.youtube.com/watch?v=jDINUSK7rXE

Once completed, try completing the following quizzes. You may need to use Google to find some of the answers.

[Quiz 1](http://www.ch.embnet.org/CoursEMBnet/Exercises/Quiz/quix1.html)

[Quiz 2](http://www.ch.embnet.org/CoursEMBnet/Exercises/Quiz/quix2.html)

[Quiz 3](http://www.ch.embnet.org/CoursEMBnet/Exercises/Quiz/quix3.html)

### An API Overview

What is an API and what can you do with it? Review the following videos and guides to get a better understanding.

[What is REST?](http://www.restapitutorial.com/lessons/whatisrest.html)

[REST API concepts and examples (youtube)](https://www.youtube.com/watch?v=7YcW25PHnAA)

[HTTP Methods: GET vs. POST](https://www.w3schools.com/tags/ref_httpmethods.asp)

### Making cURL requests

Now that you've learned about APIs, find out how to transfer data using a cURL command

[How to use the cURL command (youtube)](https://www.youtube.com/watch?v=WxUVU0b95Oc)

[An intro to cURL](https://gist.github.com/joyrexus/85bf6b02979d8a7b0308)

[cURL examples](https://www.rosehosting.com/blog/curl-command-examples/)

### Making API requests

[Pythong Guide on Requests](http://docs.python-requests.org/en/master/)

[Coding with Python, API basics to grab data (youtube)](https://www.youtube.com/watch?v=pxofwuWTs7c)

[Python library: urllib2](https://docs.python.org/2/library/urllib2.html)

----
## 2. Time for Exercises

### Homework 1

**Part one:**
Check out the Open Philly Indego Bikes API. 
Get a sense of what it does. 

Then try the following:

```
import requests
import json
import urllib2
import pandas as pd
import zipfile
from StringIO import StringIO
```

**1. Make a single request to the API**

```
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

**2. Save the output to a .txt file**

```
mydata = json.loads(response.text)
file = open("indego.txt", "w")  #to create an empty file?? "w" = writing
file.write(json.dumps(mydata, indent=4)) #json formatting
file.close()
```

**3. Format the output in a pretty json format (You should be able to do this with one line of code)**
Reference the file.write command above

**Part two:**
Check out the export API [support docs](https://support.rjmetrics.com/hc/en-us/articles/204674465-Automating-data-retrieval-with-the-Data-Export-API) for RJMetrics.
     
**1. Export a specific report (in RJMetrics); save the contents to a .txt file**

**In Terminal**
```
curl -H "X-RJM-API-Key: *your_key*" https://api.rjmetrics.com/0.1/figure/*figure_id*/export -o "part2.txt"
```

**2. Export a .csv through a raw data export**

**In Python**
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

**In Terminal**
```
curl -H "X-RJM-API-Key: *your_key*" https://api.rjmetrics.com/0.1/export/*table_id* > "part2-1.zip"
unzip part2-1.zip
```


Note: You'll need to create a report and a raw data export to be able to do this. You cannot use a tabular report.

This will require some googling to understand. I please ask that you try to understand what you're doing along the way, and not just copy and paste indiscriminately.

Good luck 

### Homework 2

**Part one:**
Send multiple records to the RJMetrics API using a for loop.
For example, something like this:

> [{
>  "keys": ["id"],
>  "id": 1,
>  "email": "joe@schmo.com",
>  "status": "pending",
>  "created_at": "2012-08-01 14:22:32"
> },{
>  "keys": ["id"],
>  "id": 2,
>  "email": "anne@schmo.com",
>  "status": "pending",
>  "created_at": "2012-08-03 23:12:30"
> },{
>  "keys": ["id"],
>  "id": 1,
>  "email": "joe@schmo.com",
>  "status": "complete",
>  "created_at": "2012-08-05 04:51:02"
> }]
