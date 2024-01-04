## Objective
Noel Boetie used ChatNPT to write a pentest report. Go to Christmas Island and help him clean it up.

## Solution

#### The Unintended Way : Bruteforce

Although the intended method is to review the report and mark findings as false positives or valid, a bruteforce approach was successfully executed. This method involved systematically trying all possible combinations of inputs, leveraging the limited input space of 0 or 1.

- Utilized the ClusterBomb option in Burp Intruder with 9 wordlists, each containing 0 and 1.
- Modified the HTTP POST request to the /check endpoint with varying input combinations.
  
```http
POST /check HTTP/2
Host: hhc23-reportinator-dot-holidayhack2023.ue.r.appspot.com
Cookie: ReportinatorCookieYum=eyJ1c2VyaWQiOiI4MWU3MGM0NC1hZWQ5LTQ4ZDItYTQ4YS00MGQ2OWJlOTc3ZjMifQ.ZZNU5A.dSBjAvqiMqpuEarTGq5yh55lPFw
Content-Length: 89
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.71 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://hhc23-reportinator-dot-holidayhack2023.ue.r.appspot.com

Referer: https://hhc23-reportinator-dot-holidayhack2023.ue.r.appspot.com/?&challenge=reportinator&username=dibsyhex&id=81e70c44-aed9-48d2-a48a-40d69be977f3&area=ci-rudolphsrest&location=33,28&tokens=reportinator&dna=ATATATTAATATATATATATGCATATATATATTATAGCCGATATATATATATTAATATATATATATATATATATATTAGCATATATATATATGCGCATATATATATATATATATATTATA
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9

input-1=0&input-2=0&input-3=0&input-4=0&input-5=0&input-6=0&input-7=0&input-8=0&input-9=0
```

Invalid Resoponse ( Length : 274 or 278 )
```
{"error":"FAILURE"}
```

#### Correct Payload
```http
POST /check HTTP/2
Host: hhc23-reportinator-dot-holidayhack2023.ue.r.appspot.com
Cookie: ReportinatorCookieYum=eyJ1c2VyaWQiOiI4MWU3MGM0NC1hZWQ5LTQ4ZDItYTQ4YS00MGQ2OWJlOTc3ZjMifQ.ZZNU5A.dSBjAvqiMqpuEarTGq5yh55lPFw
Content-Length: 89
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.71 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Accept: */*
Origin: https://hhc23-reportinator-dot-holidayhack2023.ue.r.appspot.com
Referer: https://hhc23-reportinator-dot-holidayhack2023.ue.r.appspot.com/?&challenge=reportinator&username=dibsyhex&id=81e70c44-aed9-48d2-a48a-40d69be977f3&area=ci-rudolphsrest&location=33,28&tokens=reportinator&dna=ATATATTAATATATATATATGCATATATATATTATAGCCGATATATATATATTAATATATATATATATATATATATTAGCATATATATATATGCGCATATATATATATATATATATTATA
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Priority: u=1, i
Connection: keep-alive

input-1=0&input-2=0&input-3=1&input-4=0&input-5=0&input-6=1&input-7=0&input-8=0&input-9=1
```

Valid Response
```json
{"hash":"7fe1681630ab878a61b03421b14a0e34fe726cb726e860672bc6e3a6e4d51e0f","resourceId":"81e70c44-aed9-48d2-a48a-40d69be977f3"}
```

#### Intented Way - Wrong Pentest Findings 

3. ```By intercepting HTTP request traffic on 88555/TCP, malicious a...``` - Not a valid port 
6. The XSS payload is wrong
7. ```When given an HTTP 7.4.33 request,...``` - There is no such HTTP Version
#### Success

Report Validation Complete
Great work! You've successfully navigated through the intricate maze of data, distinguishing the authentic findings from the AI hallucinations. Your diligence in validating the penetration test report is commendable.

Your contributions to ensuring the accuracy and integrity of our cybersecurity efforts are invaluable. The shadows of uncertainty have been dispelled, leaving clarity and truth in their wake. The findings you have authenticated will play a crucial role in fortifying our digital defenses.

We appreciate your expertise and keen analytical skills in this crucial task. You are a true asset to the team. Keep up the excellent work!
