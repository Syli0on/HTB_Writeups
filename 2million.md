# TwoMillion Writeup — HackTheBox

- Difficulty : Easy
- OS : Linux
- Date : June 9th 2026

## Recogntion

As always when I start a new box I started by doing a Nmap of ports et services on it. 
\
\
<img width="740" height="210" alt="nmap" src="https://github.com/user-attachments/assets/99607cc4-cbf8-4ac2-aa66-5b5fa1cb975d" />


Two ports are open : 22 (SSH) and 80 (HTTP). I tried to find 
subdomains with Ffuf but found nothing. I then visited the website.
\
\
<img width="1920" height="933" alt="site" src="https://github.com/user-attachments/assets/3f5c546f-97ff-4175-8d8f-ff1fcc18112f" />
\
\
The page shows a login link and a "Join Us" button. Without 
credentials I clicked "Join Us" but I needed an invite code.
\
\
<img width="1892" height="77" alt="jsobfusqué" src="https://github.com/user-attachments/assets/bda9d390-1257-4344-9ba7-79a4fd682c67" />

## Exploitation
### Finding the invite code
I noticed the "How to generate" in the keywords so I decided to try it.
\
\
<img width="1109" height="71" alt="chiffré" src="https://github.com/user-attachments/assets/be24f138-566c-41ca-af42-79fc66026ce4" />
\
\
Bingo we have something went on dcode and as it said ROT13  an tada it clearly say that I have to go on api/v1/invite/generate. "Curling" it send me a code in base64 so I decoded it thanks to an echo and a base64 -d (d for decode). So I registered myself and explored this new interface.
\
\
<img width="1922" height="936" alt="dashboard_discover" src="https://github.com/user-attachments/assets/cde2c497-33a5-4622-8d6e-b5919a8ac571" />


### API Enumeration
\
\
Once logged in I enumerated the API endpoints with curl /api/v1.
\
\
<img width="1178" height="140" alt="endpoint_auth" src="https://github.com/user-attachments/assets/0c900288-d0da-4c10-82d4-4fd06fa08b36" />
\
\
I found three interesting admin endpoints. I used admin/settings/update to escalate my privileges to admin.
\
\
<img width="637" height="102" alt="admin" src="https://github.com/user-attachments/assets/c011eb28-02a3-48f1-b5ef-345d76857544" />
\
\
Which gave me conditions and when fully satisfied I went on admin/auth to see if I were trully admin and it confirmed it. Naturally I generated an admin vpn in order to see if there was a vulnerability in it and yes there was one.
\
\
<img width="863" height="197" alt="remarque_user" src="https://github.com/user-attachments/assets/1e971897-528f-414f-9203-143d0b663c5f" />
### Command injection
\
\
As admin I generated a VPN and noticed the username parameter was reflected in the output.
\
\
<img width="546" height="154" alt="creds" src="https://github.com/user-attachments/assets/6742cabe-f671-46fe-9362-df6e5316e35a" />
\
\
I injected commands through the username field. A simple ls -la revealed a .env file with hardcoded credentials.
\
\
<img width="403" height="81" alt="image" src="https://github.com/user-attachments/assets/35984e5b-572c-4a28-b7e0-43333ca15748" />
### Privilege Escalation
\
\
Running sudo -l showed no results. I searched for files 
named "admin" with find / -name "admin" and found a mail 
sent to the admin user.
\
\
<img width="504" height="51" alt="admin_machine" src="https://github.com/user-attachments/assets/e35cdf39-a826-4b71-9413-686937e6c04b" />
\
\

The mail mentioned a vulnerability in the Linux kernel's 
OverlayFS subsystem — CVE-2023-0386. This flaw allows an 
unprivileged user to gain root by copying a setuid capable 
file from a nosuid mount into another mount.
\
\
I downloaded a public PoC on my machine and transferred 
it to the target via scp. After running it I got a root shell.
\
\
<img width="650" height="514" alt="rooting" src="https://github.com/user-attachments/assets/ed932e9a-1596-4e17-8199-af830257ff28" />
\
\
Finally I am root !!! 
\
\
If I'm root I can now go get the root flag and FINALLY I SEE IT 
\
\
<img width="279" height="50" alt="root_flag" src="https://github.com/user-attachments/assets/04c2ef37-2183-4ff1-b3c6-fabaf8dd327e" />


## What do i learned ?

- How to enumerate all API endpoints with a single curl command
- How to identify and exploit command injection in a misconfigured 
VPN generator
- How CVE-2023-0386 works and how OverlayFS can be abused 
for privilege escalation  
