#uni
It stands for (HyperText Transfer Protocol)
two type of http messages:
- *request*
- *response*
the message format is **ASCII**
After a timeout the connection is closed automatically
TCP connection consume memory and leave them open leaves the data structures allocated wasting memory
# Status Codes
there are multiple status codes for http responses
- 200 -> Ok
	- request succeeded
- 301 -> Move Permanently
	- requested object is moved, new location is specified later in the message with the tag *Location* 
- 400 -> Bad Request
	- request msg not understood by the server
- 404 -> Not Found 
	- requested document not found on this server
- 505 -> HTTP version not supported
# Telnet
`telnet <host> <port>` cli command open a tcp connection to the specified url and port
	type `GET <url>` HTTP/1.1 to make a get request then press enter
	type  `Host: <host>` and press enter twice so have the response

# Cookies
Used by some browsers to maintain some state between transactions
Four Components:
- **Cookie Header Line** of http response message
- **Cookie Header Line** of http request message
- **Cookie File** kept on the user's end system
- **Back-End Database** at the Web site

The client make a simple http request to a server
The server creates and ID for the user and store it in a database

This means that the next time you make a http request the server know who you are
This is how website can keep you logged in even when the website is closed

They have various implementations:
- Authorizations
- Shopping Carts
- Recommendations
- User session state (web, e-mail)

The question is *How do we keep the state?*
	protocol endpoints: maintain state at sender/receiver over multiple transactions
	cookies: HTTP messages carry state

The Cookies permits the sites to learn about you on their site
Third party persistent cookie allow common identity to be tracked across multiple web sites

# Web Caches
Otherwise known as **Proxy Servers**
*Goal*: satisfy client request without involving origin server
*Why*: to alleviate the load on the origin servers and reduce the response time.
The user configures the browser to point to web cache
The browser send all http request to cache
- *if* the object is in cache -> cache returns it to the client
- *else* cache request the object from the origin server, caches it  and then returns it to the client
Web caches act both as client and server and they re typically installed by ISP
Using web caches is less expensive than upping the access link bit-rate
# Conditional GET
*Goal*: don't send object if cache has up-to-date cached version
*Why*: cached object can be no longer up-to-date and the client could receive and obsolete object
- no object transmission delay
- lower link utili[]()zation
*cache*: specify date of cached copy in http request
- `if-modified-since: <date>`
*server*: response contains no object is cached copy is up-to-date
- `HTTP/1.0 304 Not Modified`
# HTTP Versions
## HTTP/1.1
introduce *multiple, pipelined GETs* over single TCP connection
*problem*: the used algorithm is called **FCFS**(First Come First Served). The smaller (faster) objects could have to wait for bigger (slower) objects to be served. The bigger objects are called **HOL**(Head Of Lineblocking).
Also loss recovery (retransmitting lost TCP segment) stalls object transmission.
## HTTP/2
*Key goal*: decreasing delay in multi-object HTTP requests
It increased the flexibility for server when sending objects to clients: objects are divided into frames and the frame transmission is interleaved
*problem*: recovery from packet loss still stalls all objects transmissions
- as in HTTP/1.1, browser have incentive to open multiple parallel TCPs connections to reduce stalling, increasing overall throughput
- no security over vanilla TCP
`RFC 7540, 2015`
## HTTP/3
It is just like the previous ones but adds:[]()
- security
- per object error (and congestions) control (more pipelining) over UDP
