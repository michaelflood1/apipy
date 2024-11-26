# API

## HTTP and REST API

Representational state transfer

used in most of the modern apis on the web

Used to support single page web apps

API = Application Programming interface

ReST API: API to store/manage/retrieve data

uses HTTP (protocol for web comms)

### Resources and actions 

An API allows users to access "resources" (= data)

A resource is represented by a URI (Uniform Resource Identifier)

Resources are nouns. For example: students .

Example: https://my_api.com/students/2 is the URI that allows to manipulate data about the resource "student
#2"

Actions can be performed on resources.

The HTTP protocol defines several actions when accessing resources.

The 4 most important ones are:

|Action|HTTP verb|
|--|--|
|Retrieve resource| GET|
|Create new resource|PUT|
|Update Existing resource|POST|
|Delete resource|DELETE|

### HTTP response codes

The server returns a status code with the response. The status code contains information about the request and
whether it was successfully processed

|code|type|comment|
|--|--|--|
|1xx|informational|request recieved continuing process|
|2xx|successful|the request was successfully recieved understood and accepted|
|3xx|redirection|further action needs to be taken in order to complete the request|
|4xx|client error|the request contains bad syntax or cannot be fulfilled|
|5xx|server error|the server failed to fulfill an apparently valid request|

### HTTP status codes

|number|alpha resp|comment|
|--|--|--|
|200|OK|Everything is fine|
|204|OK| Everything is fine(empty response)|
|400|Bad Request|the client did something wrong|
|403|Forbidden|Access is forbidden tot he resource|
|404|resource not found|resource does not exist|
|405|Method not allowed| the resource cannot be manipulated with that method|
|500|internal server error|YOU SHOULD BE WORRIED BUG - IN APPLICATION|

### mAKING http REQUESTS WITH THE REQUESTS LIBRARY

Install it in the virtual environment: pip install requests

You can make GET, POST, PUT, DELETE requests with the relevant methods.

The following are examples using a demo API that gives you info about the request just made

```py
>>> response = requests.get("https://httpbin.org/get")
>>> response.text
'{\n "args": {}, \n "headers": {\n "Accept": "*/*", \n "Accept-Encoding": "gzip, deflate", \n
"Host": "httpbin.org", \n "User-Agent": "python-requests/2.24.0", \n
"X-Amzn-Trace-Id": "Root=1-5f948529-1af85529337c3efc7189b2f3"\n }, \n
"origin": "172.103.147.11", \n "url": "https://httpbin.org/get"\n}\n'
>>> response.json()
{'args': {},
'headers': {'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate',
'Host': 'httpbin.org', 'User-Agent': 'python-requests/2.24.0',
'X-Amzn-Trace-Id': 'Root=1-5f948529-1af85529337c3efc7189b2f3'},
'origin': '172.103.147.11', 'url': 'https://httpbin.org/get'
}
```

^Get

```py
>>> response = requests.put("https://httpbin.org/put", json={"hello": "world", "something": 12345})
>>> response.json()
{'args': {},
'data': '{"hello": "world", "something": 12345}',
'files': {}, 'form': {},
'headers': {
'Accept': '*/*', 'Accept-Encoding': 'gzip, deflate',
'Content-Length': '38', 'Content-Type': 'application/json',
'Host': 'httpbin.org', 'User-Agent': 'python-requests/2.24.0',
'X-Amzn-Trace-Id': 'Root=1-5f948572-6f73cf4c41406d523a3eb244'
},
'json': {'hello': 'world', 'something': 12345},
'origin': '172.103.147.11', 'url': 'https://httpbin.org/put'
}
```
^PUT
example status code

    >>> response = requests.get("https://httpbin.org/status/404")
    >>> response.status_code
    404
    >>> response = requests.get("https://httpbin.org/status/503")
    >>> response.status_code
    503

### making HTTP requests with httpie

install with pip install httpie

make requests through command line using http command

    > http get https://httpbin.org/get
    HTTP/1.1 200 OK
    Access-Control-Allow-Credentials: true
    Access-Control-Allow-Origin: *
    Connection: keep-alive
    Content-Length: 298
    Content-Type: application/json
    Date: Sat, 24 Oct 2020 19:55:09 GMT
    Server: gunicorn/19.9.0
    {
    "args": {},
    "headers": {
    "Accept": "*/*",
    "Accept-Encoding": "gzip, deflate",
    "Host": "httpbin.org",
    "User-Agent": "HTTPie/2.2.0",
    "X-Amzn-Trace-Id": "Root=1-5f94869d-187ce78308c9cb23422797dc"
    },
    "origin": "172.103.147.11",
    "url": "https://httpbin.org/get"
    }

    > http --json put https://httpbin.org/put name=Tim score=1234
    HTTP/1.1 200 OK
    Content-Type: application/json
    Server: gunicorn/19.9.0
    {
    [...]
    "data": "{\"name\": \"Tim\", \"score\": \"1234\" ",
    [...]
    "json": {
    "name": "Tim",
    "score": "1234"
    },
    }

## Rest API with flask

Return JSON with FLASK

```py
@bp.route("/api/list/")
def show_list():
list_of_dicts = [
{
"name": "Tim",
"city": "Vancouver",
},
{
"name": "John",
"city": "Toronto",
}
]
return jsonify(list_of_dicts)
```

### serialize deserialize

some data types cannot be serialized natively into JSON

you must serialize them first

typically using dictionaries attr/value

```py
@bp.route("/api/list/")
def show_list():
manager = ObjectManager()
list_of_objects = [
obj.to_dict() for obj in manager.get_objects()
]
return jsonify(list_of_objects)
```

### Process arguments and parameters


```py
@bp.route("/api/detail/<int:number>/")
def show_detail(number):
# Return data for that specific number
pass
```

```py
from flask import request
@bp.route("/api/list/")
def show_list():
order = request.args.get("order", False)
# Will match when the request is /api/list?order=yes
if order == 'yes':
list_of_objects.sort()
return jsonify(list_of_objects)
```