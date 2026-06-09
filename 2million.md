TwoMillion Writeup — HackTheBox

- Difficulty : Easy
- OS : Linux
- Date : June 9th 2025

## Recongntion

As always when I start a new box I started by doing a Nmap of ports et services on it. 

<img width="740" height="210" alt="nmap" src="https://github.com/user-attachments/assets/99607cc4-cbf8-4ac2-aa66-5b5fa1cb975d" />

We can see that two ports are opened the 22(SSH) and 80(HTTP). To go further in the phase I decided to search for sub-domains thanks to Fuff but it didn't find anything. So I connected to the website host by the machine. I arrvied on this page <img width="1920" height="933" alt="site" src="https://github.com/user-attachments/assets/3f5c546f-97ff-4175-8d8f-ff1fcc18112f" />.
  
Au premier coup d'oeil on voit un login cependant comme je n'ai trouvé aucun 
At first glance we can see a login link and a Join Us button as I didn't find any other thing usefull I went first to the log in page and found as I expected a login form but i didn't have any credentials so I then have cliked on Join US. Unfortunately I didn't had a code so i decided to watch in the cource code and found a suspicious JS that might verfy the code so I made a curl on it to see what it can teach me about and found this 
<img width="1892" height="77" alt="jsobfusqué" src="https://github.com/user-attachments/assets/bda9d390-1257-4344-9ba7-79a4fd682c67" />
.I noticed the "How to generate" in the keywords so i decided to try it <img width="1109" height="71" alt="chiffré" src="https://github.com/user-attachments/assets/be24f138-566c-41ca-af42-79fc66026ce4" /> bingo we have something went on dcode and as it said rotated it by 13 an tada it clearly say that i have to go on api/v1/invite/generate


## Exploitation
[comment t'as obtenu le foothold]

## Privilège Escalation
[comment t'as obtenu root/system]

## Lessons Learned
[ce que t'as appris]
