# Answers to Homework

----

## Homework 1

### Part one:

**1. Make a single request to the API**
```
import requests
import json
import zipfile

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

*Reference the file.write command above*

### Part two:

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**1. Export a specific report (in RJMetrics); save the contents to a .txt file**

```
curl -H "X-RJM-API-Key: *your_key*" https://api.rjmetrics.com/0.1/figure/*figure_id*/export -o "part2.txt"
```

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**2. Export a .csv through a raw data export (try doing this in both the terminal and python)**

<details>
<summary>Answer in Python </summary>

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

</details>


<details> 
<summary> Answer in Terminal </summary>

curl -H "X-RJM-API-Key: *your_key*" https://api.rjmetrics.com/0.1/export/*table_id* > "part2-1.zip"
unzip exercise2.zip

</details>



