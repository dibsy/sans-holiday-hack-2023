## Objective
Visit Ginger Breddie in Santa's Shack on Christmas Island to help him with some basic Linux tasks. It's in the southwest corner of Frosty's Beach.

## Solution

- The North Pole 🎁 Present Maker:
All the presents on this system have been stolen by trolls. Capture trolls by following instructions here and 🎁's will appear in the green bar below. Run the command "hintme" to receive a hint.
```
yes
```

- Perform a directory listing of your home directory to find a troll and retrieve a present!
```
ls
```

- Now find the troll inside the troll.
```
cat troll_19315479765589239 | grep -i "troll"
```

- Great, now remove the troll in your home directory.
```
rm troll_19315479765589239
```

- Print the present working directory using a command.
```
pwd
```

- Good job but it looks like another troll hid itself in your home directory. Find the hidden troll!
```
ls -la
```

- Excellent, now find the troll in your command history.
```
history
```

- Find the troll in your environment variables.
```
env
```

- Next, head into the workshop.
```
cd workshop
```

- A troll is hiding in one of the workshop toolboxes. Use "grep" while ignoring case to find which toolbox the troll is in. ```grep -ir  "troll*" .```

- A troll is blocking the present_engine from starting. Run the present_engine binary to retrieve this troll.
```
ls -la | grep present
chmod +x  present_engine
```

- Trolls have blown the fuses in /home/elf/workshop/electrical. cd into electrical and rename blown_fuse0 to fuse0.
```
/home/elf/workshop/electrical
mv blown_fuse0 fuse0
```

- Now, make a symbolic link (symlink) named fuse1 that points to fuse0 ```ln -s fuse0 fuse1```

- Make a copy of fuse1 named fuse2. ```cp fuse1 fuse2```

- We need to make sure trolls don't come back. Add the characters "TROLL_REPELLENT" into the file fuse2.
```
echo -n TROLL_REPELLENT > fuse2
```

- Find the troll somewhere in /opt/troll_den.
```
cd /opt/troll_den
find . -iname *troll*
```

- Find the file somewhere in /opt/troll_den that is owned by the user troll.
```
find . -type f -user troll
```

- Find the file created by trolls that is greater than 108 kilobytes and less than 110 kilobytes located somewhere in /opt/troll_den.
```
find /opt/troll_den/ -type f -size +108k -size -110k
```

- List running processes to find another troll.
```
 ps -aux
```

- The 14516_troll process is listening on a TCP port. Use a command to have the only listening port display to the screen.
```
netstat -antp
```

- The service listening on port 54321 is an HTTP server. Interact with this server to retrieve the last troll.
```
curl http://localhost:54321
```

- Your final task is to stop the 14516_troll process to collect the remaining presents.
```
ps -aux
kill -9 15362
```


