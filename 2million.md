TwoMillion Writeup — HackTheBox

- Difficulty : Easy
- OS : Linux
- Date : June 9th 2025

## Recogntion

As always when I start a new box I started by doing a Nmap of ports et services on it. 

<img width="740" height="210" alt="nmap" src="https://github.com/user-attachments/assets/99607cc4-cbf8-4ac2-aa66-5b5fa1cb975d" />

We can see that two ports are opened the 22(SSH) and 80(HTTP). To go further in the phase I decided to search for sub-domains thanks to Fuff but it didn't find anything. So I connected to the website host by the machine. I arrvied on this page <img width="1920" height="933" alt="site" src="https://github.com/user-attachments/assets/3f5c546f-97ff-4175-8d8f-ff1fcc18112f" />.

At first glance we can see a login link and a Join Us button as I didn't find any other thing usefull I went first to the log in page and found as I expected a login form but i didn't have any credentials so I then have cliked on Join US. Unfortunately I didn't had a code so i decided to watch in the cource code and found a suspicious JS that might verfy the code so I made a curl on it to see what it can teach me about and found this 
<img width="1892" height="77" alt="jsobfusqué" src="https://github.com/user-attachments/assets/bda9d390-1257-4344-9ba7-79a4fd682c67" />
.I noticed the "How to generate" in the keywords so i decided to try it <img width="1109" height="71" alt="chiffré" src="https://github.com/user-attachments/assets/be24f138-566c-41ca-af42-79fc66026ce4" /> bingo we have something went on dcode and as it said rotated it by 13 an tada it clearly say that i have to go on api/v1/invite/generate. "Curling" it send me a code in base64 so I decoded it thanks to an echo and a base64 -d (d for decode). So I registered myself and explored this new interface <img width="1922" height="936" alt="dashboard_discover" src="https://github.com/user-attachments/assets/cde2c497-33a5-4622-8d6e-b5919a8ac571" />. I checked admin enpoint by doing another curl on the /api/v1 <img width="1178" height="140" alt="endpoint_auth" src="https://github.com/user-attachments/assets/0c900288-d0da-4c10-82d4-4fd06fa08b36" /> where we can see three interesting admin endpoints. first i decided to escalate my privilegies by adding myself in admin first I went on admin/settings/update <img width="637" height="102" alt="admin" src="https://github.com/user-attachments/assets/c011eb28-02a3-48f1-b5ef-345d76857544" /> which gave me conditions and when fully satisfied I went on admin.auth to see if I were trully admin and it confirmed it. Naturally I generated an admin vpn in order to see if there was a vulnerability in it and yes there was one <img width="863" height="197" alt="remarque_user" src="https://github.com/user-attachments/assets/1e971897-528f-414f-9203-143d0b663c5f" /> we can see that there's the username test so it means that we can inject commands i firstly did a ls -la in order to see what can be interesting in it and found a gold mine the .env where the credential where hardcoded. <img width="546" height="154" alt="creds" src="https://github.com/user-attachments/assets/6742cabe-f671-46fe-9362-df6e5316e35a" />

thanks to those credentials i logged in it and cat the user.txt ![flag_user](https://github.com/user-attachments/assets/3dba3b7a-4b01-4075-9bb9-d72e2e113ff4)

i started doing the privilege escalation as i saw in the previous a simple sudo -l did worked so i did a find / -name "admin" in order to know all the files with admin in there name and found two of them one is his directory and the second one is a mail sent to him<img width="504" height="51" alt="admin_machine" src="https://github.com/user-attachments/assets/e35cdf39-a826-4b71-9413-686937e6c04b" /> I opened it and found explicitly found in it an exchange about a vulenrablity on the servers especially the kerberos of where unauthorized access to the execution of the setuid file with capabilities was found in the Linux kernel's OverlayFS subsystem in how a user copies a capable file from a nosuid mount into another mount. I downloaded a POC to inject it. firstly i downloaded on my machine and then thanks to npc i send it to the machine I runned it <img width="650" height="514" alt="rooting" src="https://github.com/user-attachments/assets/ed932e9a-1596-4e17-8199-af830257ff28" />
Finally I am root !!! ![root_flag](https://github.com/user-attachments/assets/a8adbdc4-33cb-492b-bf38-c6a6c8badeaf) if I'm root i can now go get the root flag and FINALLY I SEE IT ![root_flag](https://github.com/user-attachments/assets/22006c4e-4499-45ee-a92c-b74235dd2003)

## What do i learned ?

i learned how to find all the endpoint on a website in one command thanks to curl. I also learned about the possiblity to inject commands in a miconfigured VPN.   
