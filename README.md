# Hacking-Cheat-Sheet
Collection of tips and tricks when doing penetration tests personalized for me
This collection will be strictured into different sections updated once I explore new topics.

## Website enumeration

### FFUF

Ffuff can be used to scan for directories, files, extensions, vhosts, parameter values. 

Assign keyword to fuzz to wordlist:<br>
`-w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ`

Fuzzing for web directories:<br>
`ffuf -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ`

Extension fuzzing: <br>
`ffuf -w /usr/share/SecLists/Discovery/Web-Content/web-extensions.txt:FUZZ -u http://SERVER_IP:PORT/known_directory/indexFUZZ`

Page fuzzing (knowing that extension is .php): <br>
`ffuf -w /usr/share/SecLists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/known_directory/FUZZ.php`







