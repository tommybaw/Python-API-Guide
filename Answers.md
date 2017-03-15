# Answer to Exercises

----
----

## Homework 1

```
import requests
import json
import zipfile
```

### Part one:

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

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;*Reference the file.write command above*

### Part two:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1. Export a specific report (in RJMetrics); save the contents to a .txt file**

*Answer in Terminal*
```
curl -H "X-RJM-API-Key: *your_key*" https://api.rjmetrics.com/0.1/figure/*figure_id*/export -o "part2.txt"
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2. Export a .csv through a raw data export (try doing this in both the terminal and python)**

*Answer in Python*
``` 
exportid = 'INSERT EXPORT ID HERE'
apikey = 'INSERT API KEY HERE'

url1 = 'https://api.rjmetrics.com/0.1/export/' + exportid

h = {'X-RJM-API-Key': apikey}
response1 = requests.get(url1, headers=h)
print response1      

with open("example.zip", 'w') as f:
    f.write(response1.content)
zip = zipfile.ZipFile('example.zip')
zip.extractall()
```

*Answer in Terminal*
```
curl -H "X-RJM-API-Key: *your_key*" https://api.rjmetrics.com/0.1/export/*table_id* > "part2-1.zip"
unzip exercise2.zip
```

----
## Homework 2: Import API Exercise

```
clientid = 'INSERT CLIENT ID HERE'
tableid = 'INSERT TABLE ID HERE'
apikey = 'INSERT API KEY HERE'
```

```
url = 'https://connect.rjmetrics.com/v2/client/' + clientid + '/table/' + tableid + '/data?apikey=' + apikey

h = {'Content-type': 'application/json'}
response1 = requests.post(url, headers = h, json=data)
print response1.content 
```

----
## Homework 3: Importing multiple records

```
import requests
import json
```

```
clientid = 'INSERT CLIENT ID HERE'
tableid = 'INSERT TABLE ID HERE'
apikey = 'INSERT API KEY HERE'
```

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

url = 'https://connect.rjmetrics.com/v2/client/' + clientid + '/table/' + tableid + '/data?apikey=' + apikey

h = {'Content-type': 'application/json'}

for i in data1:
    i.update({"keys": ["id"]})
    response = requests.post(url, headers = h, json=i)
    print response.content
print data1
```

----
## Homework 4: Importing indegoBike data into RJMetrics

```
import requests
import json
```

### Part One
```
# Requesting indego data
url = 'https://www.rideindego.com/stations/json/'

headers = requests.utils.default_headers()

headers.update(
    {
        'User-Agent': 'Random User',
    }
)
response = requests.get(url, headers=headers)
mydata = json.loads(response.text)

# Parsing out only the Properties category, appending list to d1
d1 = []
for x in mydata['features']:
    d1.append((x['properties']))
```

### Part Two && Three
```
# Specifying import location
clientid = 'ENTER CLIENT ID HERE'
akey = 'ENTER API KEY HERE'
tablename = 'ENTER TABLE NAME HERE'


rjurl = 'https://connect.rjmetrics.com/v2/client/' +  clientid + '/table/' + tablename + '/data?apikey=' + akey

h = {'Content-type': 'application/json'}


# Importing bike data into RJMetrics
for i in d1:
    i.update({"timestamp": datetime.now().strftime('%Y-%m-%d %H:%M:%S')}) # Adding timestamp column
    i.update({"keys": ["kioskId","timestamp"]}) # This is the primary key
    response = requests.post(rjurl, headers = h, json=i)
    print response.content # Prints data for each bike station (to confirm loop works correctly)

print json.dumps(d1, indent = 4) # Prints in json format
```

----
## Homework 5: Adding a cron job


----
## Homework 6: Generating fresh reports

```
import requests
import json
import zipfile
import time
```

### Part One && Two
```
tableid = 'INSERT TABLE ID HERE'
exportname = 'INSERT EXPORT NAME HERE'
apikey = 'INSERT API KEY HERE'

# Send POST request to existing Raw Data Export
# Generates new Export with new Table ID
# Prints new Table ID
url = 'https://api.rjmetrics.com/0.1/export/' + tableid + '/copy'
h = {'X-RJM-API-Key': apikey}
data = {'name': exportname}

response2 = requests.post(url, headers=h, data = data)
```

### Part Three
```
newid = json.loads(response2.content)

print "New Export ID: " + str(newid['export_id'])
```

### Answer to Note
```
# Loop to wait for new Export ID to generate
status = ''
while status != 'Completed':
	print 'Waiting for Raw Data Export to load.'
	time.sleep(5)
	info = requests.get('https://api.rjmetrics.com/0.1/export/' + str(newid['export_id']) + '/info', headers = h)
	status = json.loads(info.content)['status']
```

### Part Four
```
# Requesting new information and saving zip
url3 = 'https://api.rjmetrics.com/0.1/export/' + str(newid['export_id'])

h = {'X-RJM-API-Key': apikey}
response1 = requests.get(url3, headers=h)
print "Response Code: " + str(response1.status_code)

with open("log.zip", 'w') as f:
    f.write(response1.content)
zip = zipfile.ZipFile('log.zip')
zip.extractall()
```

----
## Homework 7: Continuation of Homework 6

```
import requests
import json
import csv
```

```
clientid = 'INSERT CLIENT ID HERE'
apikey = 'INSERT API KEY HERE'
exportname = 'INSERT EXPORT NAME HERE'
primarykey = 'ENTER THE PRIMARY KEY OF THE TABLE HERE'
tablename = 'ENTER NAME OF THE NEW TABLE NAME'
```

### Import csv into python
```
arr = []

with open(exportname + '.csv') as f:
    reader = csv.DictReader(f)
    for row in reader:
        arr.append(row)
     
        
jsonText = json.dumps(arr, indent = 4)
print jsonText # To make sure the data is in the correct JSON format
```

### Sending the data into an API. We use RJMetrics here as an example.
```
posturl = 'https://connect.rjmetrics.com/v2/client/' + clientid + '/table/' + tablename + '/data?apikey=' + apikey

h = {'Content-type': 'application/json'}

for i in arr:
    i.update({"keys": [clientid]})
    response = requests.post(posturl, headers = h, json=i)
    print response.content
```

### Answer to Bonus:
```
for i in arr:
    i.update({"keys": [primarykey]}) # Adds PK
group = [arr[i:i+100] for i in range(0, len(arr), 100)] # Groups data in sets of 100

for i in group: # Send POST request in batches
    response = requests.post(posturl, headers = h, json=i)
    print response.content
```

----
## Homework 8: Parsing data

```
import csv
import requests
import json
import zipfile
import time
```

### Refresh raw export, download and save to csv
```
exportid = '119999'
apikey = '049c7b7ced91878038365cca434fe22e'
exportname = 'revenuereport'

h = {'X-RJM-API-Key': apikey}
data = {'name': exportname}

url = 'https://api.rjmetrics.com/0.1/export/' + exportid + '/copy'
response2 = requests.post(url, headers=h, data = data)
newid = json.loads(response2.content)
print "New Export ID: " + str(newid['export_id'])

# 5s Delay Loop
status = ''
while status != 'Completed':
	print 'Waiting for Raw Data Export to load.'
	time.sleep(5)
	info = requests.get('https://api.rjmetrics.com/0.1/export/' + str(newid['export_id']) + '/info', headers = h)
	status = json.loads(info.content)['status']

# Extracting zip/csv
url3 = 'https://api.rjmetrics.com/0.1/export/' + str(newid['export_id'])
response1 = requests.get(url3, headers=h)
print "Response Code: " + str(response1.status_code)

with open("revenue.zip", 'w') as f:
    f.write(response1.content)
zip = zipfile.ZipFile('revenue.zip')
zip.extractall()
print 'Complete'
```

### Load back into python
```
arr = []

with open(exportname + '.csv') as f:
    reader = csv.DictReader(f)
    for row in reader:
        arr.append(row)
```

### Parsing data for specific columns
```
columns = ['date', 'Customer\'s lifetime revenue']

d0 = []
for i in range(0, len(arr)):
    d0.append(dict((k, arr[i][k]) for k in (columns) if k in arr[i]))
```







