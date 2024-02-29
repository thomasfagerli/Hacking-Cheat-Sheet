# Hacking-Cheat-Sheet
Collection of tips and tricks when doing penetration tests personalized for me
This collection will be strictured into different sections updated once I explore new topics.

## Website enumeration

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





