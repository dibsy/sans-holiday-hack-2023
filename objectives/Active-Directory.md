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
### Accessing the Vaults
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
