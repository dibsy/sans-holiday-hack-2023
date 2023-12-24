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
