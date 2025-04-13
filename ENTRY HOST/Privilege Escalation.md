### SSH connection

Connect via SSH with the following credentials you found earlier.

`safeadmin: HardcodedMeansUnguessableRight`

```
ssh safeadmin@bandit.escape
```

![[Exploitation-20250410214726331.webp]]


## Who am I?

Run:

```
id
```

![[Privilege Escalation-20250411201844847.webp]]

>[!Success]
>You are part of the sudo group which gives automatic root access.

## Flag 1

If you list the files, you'll find the flag.txt file.

>[!Success]
>The flag is: THM{ALL_THIS_ESCAPING_MAKES_ME_TIRED_AM_I_DONE?}



**Next step:** [[Connecting to Windows Machine]]
