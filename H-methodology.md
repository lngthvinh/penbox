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

```
```
