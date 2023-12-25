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
