- How many Craftperson Elf's are working from laptops?
```Kql
Employees 
| where role has "Craftsperson Elf" 
| where hostname has "LAPTOP"
| count
```
