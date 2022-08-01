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
- ☐ Server Side Inclusion/Edge Side Inclusion
- ☐ Uncovering Cloudflare
- ☐ XSLT Server Side Injection
