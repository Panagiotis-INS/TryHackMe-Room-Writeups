Machine IP:  10.10.187.60

We have an assume breach scenario

Domain: `lunar.eruca.com`
user: `thm`
pass: `Password1@`


# Nmap Scanning:

```bash
nmap -Pn -sV -sC 10.10.187.60 -oN ./Nmap/nmap.init
```

![[./Images/Pasted image 20230928225700.png]]


![[./Images/Pasted image 20230928225731.png]]



# RDP:

We have RDP access with the breached credentials


