# Home Lab Session 1 - Metasploitable2

## Setup
Ran Kali Linux and Metasploitable2 in VirtualBox on a host-only network so both machines can talk to each other but are completely cut off from my actual network. Kali was on 192.168.56.101 and Metasploitable was on 192.168.56.102.

<img width="1488" height="663" alt="image" src="https://github.com/user-attachments/assets/931d5931-79a7-46a3-90af-52759e9bfa86" />

## Recon
First thing I did was run nmap on the target to see what was open. Got back 23 open ports. The interesting ones were FTP (21), Telnet (23), Samba (139), HTTP (80) and MySQL (3306). Each open port is something you can potentially attack.

<img width="643" height="540" alt="image" src="https://github.com/user-attachments/assets/f482d94a-ee80-4a43-8236-7e187f73e8c5" />

## Anonymous FTP
FTP was running on port 21 so I tried logging in with "anonymous" as the username and no password. It just let me in. There was nothing in there but the fact that anyone can log in without a password is already a serious issue on a real server.

<img width="383" height="250" alt="image" src="https://github.com/user-attachments/assets/e1421443-6cc3-4d55-bfbb-cdfba3cc90ac" />

## Telnet
Tried connecting to Telnet on port 23 with the default login msfadmin:msfadmin and got straight in. The problem with Telnet is that everything you send including passwords goes over the network completely unencrypted so anyone watching the traffic would see the password in plain text.

<img width="630" height="684" alt="image" src="https://github.com/user-attachments/assets/bc0efd7a-b723-41c0-829e-85e77001ed02" />

## Samba Exploit
Used Metasploit to run an exploit called usermap_script against the Samba service on port 139. This gave me a root shell on the target machine - meaning full control without needing any credentials at all.

<img width="635" height="727" alt="image" src="https://github.com/user-attachments/assets/93fe4c19-3d2b-4fc7-aab4-8d808d00c9ac" />

## Password Cracking
With root access I ran cat /etc/shadow which dumped all the password hashes on the machine. Threw them into John the Ripper and it cracked msfadmin, user, postgres and service almost instantly. All of them were just using their own username as their password which is why they got cracked so fast.


<img src="https://github.com/user-attachments/assets/927ff285-9ff5-4d55-b345-d5da617bfbd4" /> <img src="https://github.com/user-attachments/assets/f201046d-b111-4ede-bd7a-a293ca72751c" />
