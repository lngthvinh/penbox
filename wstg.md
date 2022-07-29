# Testing Checklist

The following is the list of items to test during the assessment:

## Information Gathering

| Test ID           | Test Name                                                                  | How to Test                |
|:------------------|:---------------------------------------------------------------------------|:---------------------------|
| **WSTG-INFO**     | **Information Gathering**                                                  |                            |
| WSTG-INFO-01      | Conduct Search Engine Discovery and Reconnaissance for Information Leakage | Google Dorking, DuckDuckGo |
| WSTG-INFO-02      | Fingerprint Web Server                                                     | Wappalyzer, nmap           |
| WSTG-INFO-03      | Review Webserver Metafiles for Information Leakage                         | robots.txt                 |
| WSTG-INFO-04      | Enumerate Applications on Webserver                                        | nuclei, nmap, dirsearch    |
| WSTG-INFO-05      | Review Webpage Content for Information Leakage                             | Burp Suite, Vega, Zaproxy  |
| WSTG-INFO-06      | Identify Application Entry Points                                          | Burp Suite, Vega, Zaproxy  |
| WSTG-INFO-07      | Map Execution Paths Through Application                                    | Burp Suite, Vega, Zaproxy  |
| WSTG-INFO-08      | Fingerprint Web Application Framework                                      | Wappalyzer                 |
| WSTG-INFO-09      | Fingerprint Web Application                                                |                            |
| WSTG-INFO-10      | Map Application Architecture                                               | Database, Authen, Proxy..  |

---
**TODO**
```
// Subdomain
https://dnsdumpster.com/
knockpy <domain> --no-http-code 404 500 530 -th 100 -o knockpy (long time)
./sudomy -d <domain> [-b] -dP -eP -tO -wS -cF -pS -rS -sC -nT --httpx --dnsprobe -aI webanalyze --slack --html -sS

//*//
B=<domain>;subfinder -d $B | httprobe | tee sub-domain/$B-output.txt | nuclei -t nuclei-templates/ -o nuclei/$B-output.txt

// Information Disclosure
Google Dorking
bash degoogle_hunter.sh <domain>
python3 JSFinderPlus.py -u <url>
python2 SimplyEmail.py -all -e <domain/ip>

// Determine the version and type of web server
Wappalyzer
B=<ip>;nuclei -t nuclei-templates/ -u $B -o nuclei/$B-output.txt
B=<ip>;nmap -sC -sV $B -vv -oN nmap/$B-output.txt -p- --min-rate 5000 -T5

// Identify hidden or obfuscated information
robots.txt
sitemap.xml
.well-known/security.txt
humans.txt
B=<domain/ip>;dirsearch -u http://$B -t 100 -r -i 200,301,302,401,403 -o ${PWD}/dirsearch/$B-output.txt

// Fingerprint
HTTP headers
Cookies
HTML source code
Specific files and folders
File extensions
Error messages

// Scan
Burp Suite + Vega + Zaproxy + Nikto (B=<domain/ip>;nikto -h $B -p 80,443 -o nikto/$B-output.txt)
```
---

## Configuration and Deploy Management Testing

| Test ID           | Test Name                                                                  | How to Test               |
|:------------------|:---------------------------------------------------------------------------|:--------------------------|
| **WSTG-CONF**     | **Configuration and Deploy Management Testing**                            |                           |
| WSTG-CONF-01      | Test Network Infrastructure Configuration                                  | nmap                      |
| WSTG-CONF-02      | Test Application Platform Configuration                                    | nuclei, dirsearch         |
| WSTG-CONF-03      | Test File Extensions Handling for Sensitive Information                    | nikto                     |
| WSTG-CONF-04      | Review Old Backup and Unreferenced Files for Sensitive Information         | dirsearch, nikto          |
| WSTG-CONF-05      | Enumerate Infrastructure and Application Admin Interfaces                  | dirsearch, hydra, Zaproxy |
| WSTG-CONF-06      | Test HTTP Methods                                                          | nmap                      |
| WSTG-CONF-07      | Test HTTP Strict Transport Security                                        | curl                      |
| WSTG-CONF-08      | Test RIA Cross Domain Policy                                               | Vega, Zaproxy, corsy      |
| WSTG-CONF-09      | Test File Permission                                                       |                           |
| WSTG-CONF-10      | Test for Subdomain Takeover                                                | sudomy                    |
| WSTG-CONF-11      | Test Cloud Storage                                                         |                           |
| WSTG-CONF-12      | Testing for Content Security Policy                                        | Google CSP Evaluator      |

---
**TODO**
```
// Test Net
nmap -vv --script http-methods <target>
nmap -vv -p 445 --script=smb-* --script-args=unsafe=1 <target>
nmap --script=ftp-anon,ftp-bounce,ftp-libopie,ftp-proftpd-backdoor,ftp-vsftpd-backdoor,ftp-vuln-cve2010-4221,tftp-enum -p 21 <target>
python2 shocker.py -H <target> --command "/bin/cat /etc/passwd" -c /cgi-bin/status --verbose

// Test HTTP Strict Transport Security
curl -s -D- <url> | grep -i strict

// Test Cross Domain Policy
python3 corsy.py -u <url>

// Testing Content Security Policy
https://csp-evaluator.withgoogle.com/
```
---

## Identity Management Testing

| Test ID           | Test Name                                                                  | How to Test               |
|:------------------|:---------------------------------------------------------------------------|:--------------------------|
| **WSTG-IDNT**     | **Identity Management Testing**                                            |                           |
| WSTG-IDNT-01      | Test Role Definitions                                                      | Burp's Autorize extension |
| WSTG-IDNT-02      | Test User Registration Process                                             |                           |
| WSTG-IDNT-03      | Test Account Provisioning Process                                          |                           |
| WSTG-IDNT-04      | Testing for Account Enumeration and Guessable User Account                 |                           |
| WSTG-IDNT-05      | Testing for Weak or Unenforced Username Policy                             |                           |

## Authentication Testing

| Test ID           | Test Name                                                                  | How to Test            |
|:------------------|:---------------------------------------------------------------------------|:-----------------------|
| **WSTG-ATHN**     | **Authentication Testing**                                                 |                        |
| WSTG-ATHN-01      | Testing for Credentials Transported over an Encrypted Channel              |                        |
| WSTG-ATHN-02      | Testing for Default Credentials                                            | Burp Intruder, Hydra   |
| WSTG-ATHN-03      | Testing for Weak Lock Out Mechanism                                        | Burp Intruder, Hydra   |
| WSTG-ATHN-04      | Testing for Bypassing Authentication Schema                                | Burp Intruder, Zaproxy |
| WSTG-ATHN-05      | Testing for Vulnerable Remember Password                                   |                        |
| WSTG-ATHN-06      | Testing for Browser Cache Weakness                                         |                        |
| WSTG-ATHN-07      | Testing for Weak Password Policy                                           |                        |
| WSTG-ATHN-08      | Testing for Weak Security Question Answer                                  |                        |
| WSTG-ATHN-09      | Testing for Weak Password Change or Reset Functionalities                  |                        |
| WSTG-ATHN-10      | Testing for Weaker Authentication in Alternative Channel                   |                        |

---
**TODO**
```
hydra -l <username> -P <wordlist> ftp://MACHINE_IP
hydra -l <username> -P <wordlist> MACHINE_IP -t 4 ssh
hydra -l <username> -P <wordlist> MACHINE_IP http-post-form "/login:username=^USER^&password=^PASS^:F=incorrect" -V
```
---

## Authorization Testing

| Test ID           | Test Name                                                                  | How to Test          |
|:------------------|:---------------------------------------------------------------------------|:---------------------|
| **WSTG-ATHZ**     | **Authorization Testing**                                                  |                      |
| WSTG-ATHZ-01      | Testing Directory Traversal File Include                                   | DirTraversal/LFI/RFI |
| WSTG-ATHZ-02      | Testing for Bypassing Authorization Schema                                 |                      |
| WSTG-ATHZ-03      | Testing for Privilege Escalation                                           |                      |
| WSTG-ATHZ-04      | Testing for Insecure Direct Object References                              |                      |

---
**TODO**
- [PayloadsAllTheThings - Directory Traversal](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Directory%20Traversal)
- [PayloadsAllTheThings - File Inclusion](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/File%20Inclusion)
---

## Session Management Testing

| Test ID           | Test Name                                                                  | How to Test |
|:------------------|:---------------------------------------------------------------------------|:------------|
| **WSTG-SESS**     | **Session Management Testing**                                             |             |
| WSTG-SESS-01      | Testing for Session Management Schema                                      | Burp Suite  |
| WSTG-SESS-02      | Testing for Cookies Attributes                                             | Burp Suite  |
| WSTG-SESS-03      | Testing for Session Fixation                                               | Burp Suite  |
| WSTG-SESS-04      | Testing for Exposed Session Variables                                      | Burp Suite  |
| WSTG-SESS-05      | Testing for Cross Site Request Forgery                                     | Burp Suite  |
| WSTG-SESS-06      | Testing for Logout Functionality                                           | Burp Suite  |
| WSTG-SESS-07      | Testing Session Timeout                                                    | Burp Suite  |
| WSTG-SESS-08      | Testing for Session Puzzling                                               | Burp Suite  |
| WSTG-SESS-09      | Testing for Session Hijacking                                              | Burp Suite  |
| WSTG-SESS-10      | Testing JSON Web Tokens                                                    | Burp Suite  |

## Session Management Testing

| Test ID           | Test Name                                                                  | How to Test |
|:------------------|:---------------------------------------------------------------------------|:------------|
| **WSTG-INPV**     | **Input Validation Testing**                                               |             |
| WSTG-INPV-01      | Testing for Reflected Cross Site Scripting                                 |             |
| WSTG-INPV-02      | Testing for Stored Cross Site Scripting                                    |             |
| WSTG-INPV-03      | Testing for HTTP Verb Tampering                                            |             |
| WSTG-INPV-04      | Testing for HTTP Parameter pollution                                       |             |
| WSTG-INPV-05      | Testing for SQL Injection                                                  |             |
| WSTG-INPV-06      | Testing for LDAP Injection                                                 |             |
| WSTG-INPV-07      | Testing for XML Injection                                                  |             |
| WSTG-INPV-08      | Testing for SSI Injection                                                  |             |
| WSTG-INPV-09      | Testing for XPath Injection                                                |             |
| WSTG-INPV-10      | Testing for IMAP SMTP Injection                                            |             |
| WSTG-INPV-11      | Testing for Code Injection                                                 |             |
| WSTG-INPV-12      | Testing for Command Injection                                              |             |
| WSTG-INPV-13      | Testing for Format String Injection                                        |             |
| WSTG-INPV-14      | Testing for Incubated Vulnerabilities                                      |             |
| WSTG-INPV-15      | Testing for HTTP Splitting Smuggling                                       |             |
| WSTG-INPV-16      | Testing for HTTP Incoming Requests                                         |             |
| WSTG-INPV-17      | Testing for Host Header Injection                                          |             |
| WSTG-INPV-18      | Testing for Server-Side Template Injection                                 |             |
| WSTG-INPV-19      | Testing for Server-Side Request Forgery                                    |             |

---
**TODO**
- [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
---

## Error Handling

| Test ID           | Test Name                                                                  | How to Test |
|:------------------|:---------------------------------------------------------------------------|:------------|
| **WSTG-ERRH**     | **Error Handling**                                                         |             |
| WSTG-ERRH-01      | Testing for Improper Error Handling                                        |             |
| WSTG-ERRH-02      | Testing for Stack Traces                                                   |             |

## Cryptography

| Test ID           | Test Name                                                                  | How to Test |
|:------------------|:---------------------------------------------------------------------------|:------------|
| **WSTG-CRYP**     | **Cryptography**                                                           |             |
| WSTG-CRYP-01      | Testing for Weak Transport Layer Security                                  |             |
| WSTG-CRYP-02      | Testing for Padding Oracle                                                 |             |
| WSTG-CRYP-03      | Testing for Sensitive Information Sent Via Unencrypted Channels            |             |
| WSTG-CRYP-04      | Testing for Weak Encryption                                                |             |

---
**TODO**
```
https://www.ssllabs.com/ssltest/analyze.html
nmap --script ssl* -p 443 <target>
```
---

| Test ID           | Test Name                                                                  | How to Test |
|:------------------|:---------------------------------------------------------------------------|:------------|
| **WSTG-BUSLOGIC** | **Business Logic Testing**                                                 |        |       |
| WSTG-BUSL-01      | Test Business Logic Data Validation                                        |        |       |
| WSTG-BUSL-02      | Test Ability to Forge Requests                                             |        |       |
| WSTG-BUSL-03      | Test Integrity Checks                                                      |        |       |
| WSTG-BUSL-04      | Test for Process Timing                                                    |        |       |
| WSTG-BUSL-05      | Test Number of Times a Function Can Be Used Limits                         |        |       |
| WSTG-BUSL-06      | Testing for the Circumvention of Work Flows                                |        |       |
| WSTG-BUSL-07      | Test Defenses Against Application Misuse                                   |        |       |
| WSTG-BUSL-08      | Test Upload of Unexpected File Types                                       |        |       |
| WSTG-BUSL-09      | Test Upload of Malicious Files                                             |        |       |
| **WSTG-CLIENT**   | **Client-side Testing**                                                    |        |       |
| WSTG-CLNT-01      | Testing for DOM Based Cross Site Scripting                                 |        |       |
| WSTG-CLNT-02      | Testing for JavaScript Execution                                           |        |       |
| WSTG-CLNT-03      | Testing for HTML Injection                                                 |        |       |
| WSTG-CLNT-04      | Testing for Client-Side URL Redirect                                       |        |       |
| WSTG-CLNT-05      | Testing for CSS Injection                                                  |        |       |
| WSTG-CLNT-06      | Testing for Client-Side Resource Manipulation                              |        |       |
| WSTG-CLNT-07      | Test Cross Origin Resource Sharing                                         |        |       |
| WSTG-CLNT-08      | Testing for Cross Site Flashing                                            |        |       |
| WSTG-CLNT-09      | Testing for Clickjacking                                                   |        |       |
| WSTG-CLNT-10      | Testing WebSockets                                                         |        |       |
| WSTG-CLNT-11      | Test Web Messaging                                                         |        |       |
| WSTG-CLNT-12      | Test Browser Storage                                                       |        |       |
| WSTG-CLNT-13      | Testing for Cross Site Script Inclusion                                    |        |       |
| **WSTG-APIT**     | **API Testing**                                                            |        |       |
| WSTG-APIT-01      | Testing GraphQL                                                            |        |       |
