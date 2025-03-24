## Nmap

#Attacking_Machine 
- Enumerate the open ports on the entry host.

```
nmap --script http-enum -sV 10.200.112.105
```

![[Enumeration-20250324210218682.webp]]

After trying port 80 you won't get anything.

- Visit http://10.200.112.105:8002.

![[Enumeration-20250324203215898.webp]]


Here, you can see there's a Login page.


## Gobuster

Gobuster is a **directory, DNS, and subdomain brute-forcing tool** commonly used for web penetration testing.

```
gobuster dir -u http://10.200.112.105:8002 -w /usr/share/wordlists/dirb/common.txt --exclude-length 3302 -t 50 -o results.txt
```

![[Enumeration-20250324210522027.webp]]


There are several routes you have to check out.

