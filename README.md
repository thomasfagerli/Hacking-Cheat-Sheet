# Hacking-Cheat-Sheet
Collection of tips and tricks when doing penetration tests personalized for me
This collection will be strictured into different sections updated once I explore new topics.

## Enumeration

### FFUF

Ffuff can be used to scan for directories, files, extensions, vhosts, parameter names and values. 

Assign keyword to fuzz to wordlist:<br>
`-w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ`

Fuzzing for web directories:<br>
`ffuf -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ`

Extension fuzzing: <br>
`ffuf -w /usr/share/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/known_directory/indexFUZZ`

Page fuzzing (knowing that extension is .php): <br>
`ffuf -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/known_directory/FUZZ.php`

Recursive fuzzing: <br>
`ffuf -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ -recursion -recursion-depth 1 -e .php -v`

Public sub-domain fuzzing: <br>
`ffuf -w /usr/share/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://FUZZ.inlanefreight.com/`

Vhost fuzzing (sub domain scan on same ip):<br>
`ffuf -w /usr/share/SecLists/Discovery/DNS/subdomains-top1million-5000.txt:FUZZ -u http://academy.htb:PORT/ -H 'Host: FUZZ.academy.htb'`

Filtering options:
```
  -fc              Filter HTTP status codes from response. Comma separated list of codes and ranges
  -fl              Filter by amount of lines in response. Comma separated list of line counts and ranges
  -fr              Filter regexp
  -fs              Filter HTTP response size. Comma separated list of sizes and ranges
  -fw              Filter by amount of words in response. Comma separated list of word counts and ranges
```
Parameter GET fuzzing:<br>
`ffuf -w /usr/share/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php?FUZZ=key -fs xxx`

Parameter POST fuzzing:<br>
`ffuf -w /usr/share/SecLists/Discovery/Web-Content/burp-parameter-names.txt:FUZZ -u http://admin.academy.htb:PORT/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx`

Value fuzzing (Using custom wordlist with numbers 1-1000): <br>
`ffuf -w ids.txt:FUZZ -u http://admin.academy.htb:30333/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded'`

###NMAP

Nmap offers host discovery, port scanning, service enumeration, service detection, os detection and scriptable interaction with target. 

Syntax:<br>
`nmap <scan types> <options> <target>`

Scanning options:
```
10.10.10.0/24 	Target network range.
-sn 	Disables port scanning.
-Pn 	Disables ICMP Echo Requests
-n 	Disables DNS Resolution.
-PE 	Performs the ping scan by using ICMP Echo Requests against the target.
--packet-trace 	Shows all packets sent and received.
--reason 	Displays the reason for a specific result.
--disable-arp-ping 	Disables ARP Ping Requests.
--top-ports=<num> 	Scans the specified top ports that have been defined as most frequent.
-p- 	Scan all ports.
-p22-110 	Scan all ports between 22 and 110.
-p22,25 	Scans only the specified ports 22 and 25.
-F 	Scans top 100 ports.
-sS 	Performs an TCP SYN-Scan.
-sA 	Performs an TCP ACK-Scan.
-sU 	Performs an UDP Scan.
-sV 	Scans the discovered services for their versions.
-sC 	Perform a Script Scan with scripts that are categorized as "default".
--script <script> 	Performs a Script Scan by using the specified scripts.
-O 	Performs an OS Detection Scan to determine the OS of the target.
-A 	Performs OS Detection, Service Detection, and traceroute scans.
-D RND:5 	Sets the number of random Decoys that will be used to scan the target.
-e 	Specifies the network interface that is used for the scan.
-S 10.10.10.200 	Specifies the source IP address for the scan.
-g 	Specifies the source port for the scan.
--dns-server <ns> 	DNS resolution is performed by using a specified name server.
```
Output Options:
```
Nmap Option 	Description
-oA filename 	Stores the results in all available formats starting with the name of "filename".
-oN filename 	Stores the results in normal format with the name "filename".
-oG filename 	Stores the results in "grepable" format with the name of "filename".
-oX filename 	Stores the results in XML format with the name of "filename".
```
Performance Options:
```
Nmap Option 	Description
--max-retries <num> 	Sets the number of retries for scans of specific ports.
--stats-every=5s 	Displays scan's status every 5 seconds.
-v/-vv 	Displays verbose output during the scan.
--initial-rtt-timeout 50ms 	Sets the specified time value as initial RTT timeout.
--max-rtt-timeout 100ms 	Sets the specified time value as maximum RTT timeout.
--min-rate 300 	Sets the number of packets that will be sent simultaneously.
-T <0-5> 	Specifies the specific timing template.
```




