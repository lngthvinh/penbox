# Port Scan - Service discovery

```
B=<ip>;nmap -vv -sV -sC -O -p- -n -Pn --min-rate 5000 -T5 -oN nmap/$B-output.txt $B
```

# Searching service version exploits

```
// Browser
Search in "google" or others: <service_name> [version] exploit

// Searchsploit
searchsploit "linux Kernel" #Example
searchsploit -m 7618 #Paste the exploit in current directory

// MSF-Search
msf> search platform:windows port:135 target:XP type:exploit
```

# Pentesting services

### Tech Detect
```
Wappalyzer
B=<host>;nuclei -t nuclei-templates/ -u http://$B -o nuclei/$B-output.txt
```

### Check if any WAF
```
nmap -p80 --script http-waf-detect <ip>
```

### Source code review
If the **source code** of the application is available in **github**, apart of performing by **your own a White box test** of the application there is **some information** that could be **useful** for the current **Black-Box testing**:
- Is there a **Change-log or Readme or Version** file or anything with **version info accessible** via web?
- How and where are saved the **credentials**? Is there any (accessible?) **file** with credentials (usernames or passwords)?
- Are **passwords** in **plain text**, **encrypted** or which **hashing algorithm** is used?
- Is it using any **master key** for encrypting something? Which **algorithm** is used?
- Can you **access any of these files** exploiting some vulnerability?
- Is there any **interesting information in the github** (solved and not solved) **issues**? Or in **commit history** (maybe some **password introduced inside an old commit**)?

### Fingerprint
```
/robots.txt
/sitemap.xml
/crossdomain.xml
/clientaccesspolicy.xml
/.well-known/
Check also comments in the main and secondary pages.

// Check information disclosure
bash degoogle_hunter.sh <domain>
python3 JSFinderPlus.py -u <url>
python2 SimplyEmail.py -all -e <domain/ip>
```

### SSL/TLS vulnerabilites
```
nmap --script ssl* -p 443 <target>
https://www.ssllabs.com/ssltest/analyze.html
testssl <url>
```

### Brute Force directories and files
```
B=<host>;dirsearch -u http://$B -t 100 -r -i 200,301,302,401,403 -o ${PWD}/dirsearch/$B-output.txt
```

### Automatic scanners
```
Vega
Burp Suite
Zaproxy
```

