# 0x01. Basic authentication
## The Domains/Concepts covered in this project: `Back-end` `Authentification`

This project introduced me to basic authentication, a foundational method for securing backend systems by verifying user credentials. I learned how to implement HTTP basic authentication, manage secure storage of credentials, and ensure proper handling of authentication headers in backend applications.

## Background Context

In this project, you will learn what the authentication process means and implement a **Basic Authentication** on a simple API.

In the industry, you should **not** implement your own Basic authentication system and use a module or framework that doing it for you (like in Python-Flask: [Flask-HTTPAuth](https://flask-httpauth.readthedocs.io/en/latest/)). Here, for the learning purpose, we will walk through each step of this mechanism to understand it by doing.

## Tasks :page_with_curl:

**0. Simple-basic-API**

Download and start your project from this archive.zip

In this archive, you will find a simple API with one model: User. Storage of these users is done via a serialization/deserialization in files.

Setup and start server

```
bob@dylan:~$ pip3 install -r requirements.txt
...
bob@dylan:~$
bob@dylan:~$ API_HOST=0.0.0.0 API_PORT=5000 python3 -m api.v1.app
 * Serving Flask app "app" (lazy loading)
...
bob@dylan:~$
```

**Use the API (in another tab or in your browser)**

```
bob@dylan:~$ curl "http://0.0.0.0:5000/api/v1/status" -vvv
*   Trying 0.0.0.0...
* TCP_NODELAY set
* Connected to 0.0.0.0 (127.0.0.1) port 5000 (#0)
> GET /api/v1/status HTTP/1.1
> Host: 0.0.0.0:5000
> User-Agent: curl/7.54.0
> Accept: */*
> 
* HTTP 1.0, assume close after body
< HTTP/1.0 200 OK
< Content-Type: application/json
< Content-Length: 16
< Access-Control-Allow-Origin: *
< Server: Werkzeug/1.0.1 Python/3.7.5
< Date: Mon, 18 May 2020 20:29:21 GMT
< 
{"status":"OK"}
* Closing connection 0
bob@dylan:~$
```

  * [0x01-Basic_authentication](./0x01-Basic_authentication)

**1. Error handler: Unauthorized**

What the HTTP status code for a request unauthorized? `401` of course!

Edit `api/v1/app.py`:

  * Add a new error handler for this status code, the response must be:
    * a JSON: `{"error": "Unauthorized"}`
    * status code `401`
    * you must use `jsonify` from Flask

For testing this new error handler, add a new endpoint in `api/v1/views/index.py`:

  * Route: `GET /api/v1/unauthorized`
  * This endpoint must raise a 401 error by using `abort` - [Custom Error Pages](https://flask.palletsprojects.com/en/stable/patterns/errorpages/)

By calling `abort(401)`, the error handler for 401 will be executed.

In the first terminal:

```
bob@dylan:~$ API_HOST=0.0.0.0 API_PORT=5000 python3 -m api.v1.app
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
....
```

In a second terminal:

```
bob@dylan:~$ curl "http://0.0.0.0:5000/api/v1/unauthorized"
{
  "error": "Unauthorized"
}
bob@dylan:~$
bob@dylan:~$ curl "http://0.0.0.0:5000/api/v1/unauthorized" -vvv
*   Trying 0.0.0.0...
* TCP_NODELAY set
* Connected to 0.0.0.0 (127.0.0.1) port 5000 (#0)
> GET /api/v1/unauthorized HTTP/1.1
> Host: 0.0.0.0:5000
> User-Agent: curl/7.54.0
> Accept: */*
> 
* HTTP 1.0, assume close after body
< HTTP/1.0 401 UNAUTHORIZED
< Content-Type: application/json
< Content-Length: 30
< Server: Werkzeug/0.12.1 Python/3.4.3
< Date: Sun, 24 Sep 2017 22:50:40 GMT
< 
{
  "error": "Unauthorized"
}
* Closing connection 0
bob@dylan:~$
```

  * [api/v1/app.py](./api/v1/app.py)
  * [api/v1/views/index.py](./api/v1/views/index.py)
