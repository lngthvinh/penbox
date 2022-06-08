    SCAN DIR
    
 ```
    dirb http://10.10.10.10/ /usr/share/SecLists/Discovery/Web-Content/common.txt
(*) dirsearch -u http://10.10.10.10/
(*) dirsearch -u http://10.10.10.10/ -w /usr/share/SecLists/Discovery/Web-Content/common.txt
    gobuster dir -u http://10.10.10.10/ -w /usr/share/SecLists/Discovery/Web-Content/common.txt
 ```

    FORENSIC
(*) # exiftool DOCUMENT.pdf

    OSINT - Wayback Machine
    https://archive.org/web/
    
    DNSDumpster
    https://dnsdumpster.com/
    
(*) nmap -Pn 10.10.10.10
(*) nmap -sV 10.10.10.10

    Enumeration
    hostname
    hostnamectl
    uname -a
    /proc/version
    /etc/issue
(*) # sudo -l
