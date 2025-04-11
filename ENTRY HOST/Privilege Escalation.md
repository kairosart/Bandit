### SSH connection

Connect via SSH with the credentials you found.

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

## Flag

If you list the files, you'll find the flag.txt file.

The flag is: 

THM{ALL_THIS_ESCAPING_MAKES_ME_TIRED_AM_I_DONE?}

