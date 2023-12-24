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
