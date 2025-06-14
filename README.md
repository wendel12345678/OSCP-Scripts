These scripts were developed to streamline common tasks during the OSCP exam and are provided as-is. Users are responsible for managing their own dependencies. The scripts primarily rely on a default Kali Linux environment and standard Python 3 libraries. This project does not represent professional-grade software and is intended solely for personal use and educational purposes. Users are strongly encouraged to explore and understand all options available in the source code of these scripts. Doing so not only ensures effective use of these specific tools, but also helps build the essential skill of reading and analyzing Python code, an important ability for success in the OSCP exam.

[![Video Tutorial](https://img.youtube.com/vi/1VTjEL_21as/maxresdefault.jpg)](https://youtu.be/1VTjEL_21as)

# Installation

The `install` script installs the scripts into `/opt/oscp-scripts` and updates the user's and sudo's path.

```
chmod +x install
sudo ./install
```
# scripts
## clock-sync

Syncs with Kerberos server. 

Usage:
```
sudo clock-sync <Kerberos IP>
```

To reset system time to normal:
```
sudo clock-sync
```

## enum-AD

Enumerates domain users into a file called users, checks the password policy, runs SharpHound extractor and saves output into the file bloodhound.zip, then tries to AS-REP roast and Kerberoast users. 

Usage:
```
enum-AD -i <IP> -u <domain user> -p|-H <password|hash>
```

This script depends on the scripts as-rep-roast, bloodhound-collect, enum-AD, get-domain, and get-domain-users.

## kdbx-crawl

Crawls a kdbx file automatically.

Usage:
```
kdbx-crawl -f <kdbx file> -p|-H <password|hash>
```

## proxy

Automatically sets up a command to run with a dynamic proxychains config.

Usage:
```
proxy <IP>:<port> <command with spaces>
```

HTTP proxy:
```
proxy --http <IP>:<port> <command with spaces>
```

## psexec

Generates a Windows msfvenom reverse shell, uploads it with impacket-psexec, then executes it. This requires administrator privileges (run with --debug to check authentication). If there seems to be a weird I/O issue, hit enter a bunch of times, and it should be fixed. 

Usage:
```
psexec -i <IP> -u <user> -p|-H <password|hash> [--debug]
```

## ps-payload

Generates an encoded powershell reverse shell.

Usage:
```
ps-payload <IP> <port>
```

## scan-network

Scans an IP range for hosts and runs a fast and slow scan on TCP and UDP for each in its own directory. The `--internal` flag was made for scans tunneled through Ligolo-NG.

Usage:
```
sudo scan-network <IP range> [--internal]
```

## upload-server

Similar to the command `python -m http.server <port>`, it creates a simple HTTP server, but supports upload as well. It dynamically grabs the tun0 interface and prints the Windows and Linux commands to remotely upload a file.

Usage:
```
upload-server <port>
```

# windows

## reboot.c
Occasionally, a user would have SeShutdownPrivilege, but I would be unable to reboot the host. Uploading the compiled program and running it fixed the problem for me in all cases.

This program can be cross-compiled from Kali with the following command:

For 64-bit (x86_64):
```
x86_64-w64-mingw32-gcc reboot.c -o reboot.exe -ladvapi32 -luser32
```

For 32-bit (x86):
```
i686-w64-mingw32-gcc reboot.c -o reboot32.exe -ladvapi32 -luser32
```
