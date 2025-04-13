
IP: 10.200.112.10

After getting root  on the ENTRY HOST you can explore the users on the machine and look for something interesting to connect to a Windows Machine.

You'll see there are two users: `safeadmin`, `ubuntu`.

## Adding hostname to /etc/hosts

#Reverse_shell
1. Go to ubuntu's directory and you'll find a file called `hostresolver.py`.

2. Take a look a that file.
```
#!/usr/bin/python3

import os
import socket

OCTECT_WINDOWS = (".10")
HOSTNAME_WINDOWS = "bandit.corp"
hostname = socket.gethostname()
ip_address = socket.gethostbyname(hostname)

octets = ip_address.split(".")

WindowsIPaddr = octets[0] + "." + octets[1] + "." + octets[2] + OCTECT_WINDOWS
etchostsr = open("/etc/hosts", "r")
if not HOSTNAME_WINDOWS in etchostsr.read():
    etchostsw = open("/etc/hosts", "a")
    etchostsw.write( WindowsIPaddr + " " + HOSTNAME_WINDOWS + " \n")
    etchostsw.close()
etchostsr.close()

```

The file  is a convenient alternative to manually adding an entry to `/etc/hosts`.

3. Run the file.
```
python3 hostresolver.py 
```

Look at the hosts file, it shows that the hostname resolves to the Windows target that is provided with this room:

```
cat /etc/hosts
```

![[Connecting to Windows Machine-20250412204127536.webp]]

4. Ping the new hostname.
```
ping bandit.corp
```

![[Connecting to Windows Machine-20250412204327533.webp]]

You have access to the Windows machine.

## PWSH

`pwsh` stands for **PowerShell**, specifically the **cross-platform** version of PowerShell (formerly known as "PowerShell Core").

### What is `pwsh`?

- It's the **command-line shell and scripting language** from Microsoft.
    
- Unlike Windows PowerShell (`powershell.exe`), which runs only on Windows, `pwsh` runs on:
    
    - ✅ Linux
    
    - ✅ macOS
    
    - ✅ Windows
    

---

### Why use `pwsh`?

- Powerful object-based scripting (not just text like Bash).

- Can interact with .NET, REST APIs, and more.

- Great for automation, DevOps, and cloud scripting (especially with Azure).


---

### How to install `pwsh` on Linux:

```bash
sudo apt install -y wget apt-transport-https software-properties-common
wget -q "https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/packages-microsoft-prod.deb"
sudo dpkg -i packages-microsoft-prod.deb
sudo apt update
sudo apt install -y powershell
```

Then run it with:

```bash
pwsh
```

---
## PowerShell history

Theoretically, there should be a file with the PowerShell history containing the data needed for further progress. Unfortunately, there’s a good chance that the file won’t be generated (this happens when you receive a “lost connection” message during registration). I also encountered this problem and after a long search and falling into various rabbit holes, I eventually just used a writeup.

```
$ClearPassword = "Passw0rd"

$SecurePass = ConvertTo-SecureString $ClearPassword -AsPlainText -Force

$credential = New-Object System.Management.Automation.PSCredential("safeuserHelpDesk", $SecurePass)

Enter-PSSession -ComputerName bandit.corp -Credential $credential -ConfigurationName testHelpDesksafe -Authentication Negotiate
```

## PowerShell session

#Reverse_shell 
1. Run pwsh.
2. Run one after another the previous commands.

	![[Connecting to Windows Machine-20250413193019825.webp]]

>[!Success]
>You have a connection with the Windows machine.

