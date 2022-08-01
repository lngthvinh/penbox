# Methodology

## Proxies
- ☐ Abusing hop-by-hop headers
```bash
for HEADER in $(cat /usr/share/seclists/Discovery/Web-Content/BurpSuite-ParamMiner/lowercase-headers); do python3 hbh-header-abuse-test.py -u <url> -x "$HEADER" -v; :'sleep 1'; done
```
- ☐ Cache Poisoning/Cache Deception
```bash
go install -v github.com/Hackmanit/Web-Cache-Vulnerability-Scanner@latest
cp go/Web-Cache-Vulnerability-Scanner tools/Web-Cache-Vulnerability-Scanner/wcvs
cd tools/Web-Cache-Vulnerability-Scanner/
./wcvs -u <url> -hw wordlists/headers -pw wordlists/parameters -f
```
- ☐ HTTP Request Smuggling 
```bash
Burp - HTTP Request Smuggler
- Right click on a request and click Extensions > HTTP HTTP Request Smuggler > Smuggle Probe.

wget https://raw.githubusercontent.com/gwen001/pentest-tools/master/smuggler.py
python3 smuggler.py -u <url> -v 3
```
- ☐ H2C Smuggling
```
python3 h2csmuggler.py -x https://southtelecom.vn/ -t --threads 5 -v
```
- ☐ Server Side Inclusion/Edge Side Inclusion (Detected by Burp Scanner)
- ☐ Uncovering Cloudflare
- ☐ XSLT Server Side Injection

## User input
### Reflected Values
- ☐ Client Side Template Injection (Like SSTI)
- ☐ Command Injection
- ☐ CRLF
- ☐ Dangling Markup
- ☐ File Inclusion/Path Traversal
- ☐ Open Redirect
- ☐ Prototype Pollution to XSS
- ☐ Server Side Inclusion/Edge Side Inclusion
- ☐ Server Side Request Forgery
- ☐ Server Side Template Injection
- ☐ Reverse Tab Nabbing
- ☐ XSLT Server Side Injection
- ☐ XSS
- ☐ XSSI
- ☐ XS-Search

