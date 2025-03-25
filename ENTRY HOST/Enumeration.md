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

## Web

Visit http://10.200.112.105:8002/.

![[Enumeration-20250325203952383.webp]]

There's not much here, but a Search field.

### Reflected XSS

Reflected XSS is a type of **Cross-Site Scripting (XSS)** attack where an attacker injects malicious scripts into a web application. The injected script is **reflected** off the web server and executed in the victim's browser.

**How It Works**

1. **Attacker crafts a malicious URL** containing a script.
2. **Victim clicks the link** (via phishing, comments, etc.).
3. **Web server reflects the input** without sanitization.
4. **Victim’s browser executes the script** (stealing cookies, session hijacking, etc.).


So, insert the following code on the search field.

```
"><script>alert('1');</script>
```

After click on the Search button you'll see a pop up window like this:

![[Enumeration-20250325205126744.webp]]

>[!Success]
>The pagee is vulnerable to Reflected XSS.

