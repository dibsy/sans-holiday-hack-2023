## Objective
Help Sparkle Redberry with some Azure command line skills. Find the elf and the terminal on Christmas Island.

## Solution
```
az help | less
```
```
az account show
{
  "environmentName": "AzureCloud",
  "id": "2b0942f3-9bca-484b-a508-abdae2db5e64",
  "isDefault": true,
  "name": "northpole-sub",
  "state": "Enabled",
  "tenantId": "90a38eda-4006-4dd5-924c-6ca55cacc14d",
  "user": {
    "name": "northpole@northpole.invalid",
    "type": "user"
  }
}
```
```
elf@944d88b2171c:~$ az group list --output table
[
  {
    "id": "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/northpole-rg1",
    "location": "eastus",
    "managedBy": null,
    "name": "northpole-rg1",
    "properties": {
      "provisioningState": "Succeeded"
    },
    "tags": {}
  },
  {
    "id": "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/northpole-rg2",
    "location": "westus",
    "managedBy": null,
    "name": "northpole-rg2",
    "properties": {
      "provisioningState": "Succeeded"
    },
    "tags": {}
  }
]

```

```
az functionapp function list --resource-group northpole-rg1
```

```
 az vm list --resource-group northpole-rg2

[
  {
    "id": "/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/northpole-rg2/providers/Microsoft.Compute/virtualMachines/NP-VM1",
    "location": "eastus",
    "name": "NP-VM1",
    "properties": {
      "hardwareProfile": {
        "vmSize": "Standard_D2s_v3"
      },
      "provisioningState": "Succeeded",
      "storageProfile": {
        "imageReference": {
          "offer": "UbuntuServer",
          "publisher": "Canonical",
          "sku": "16.04-LTS",
          "version": "latest"
        },
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage",
          "managedDisk": {
            "storageAccountType": "Standard_LRS"
          },
          "name": "VM1_OsDisk_1"
        }
      },
      "vmId": "e5f16214-18be-4a31-9ebb-2be3a55cfcf7"
    },
    "resourceGroup": "northpole-rg2",
    "tags": {}
  }
]
```
```
elf@944d88b2171c:~$ az vm run-command invoke -g northpole-rg2 -n NP-VM1 --command-id RunShellScript --scripts "ls"
{
  "value": [
    {
      "code": "ComponentStatus/StdOut/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": "bin\netc\nhome\njinglebells\nlib\nlib64\nusr\n",
      "time": 1703361311
    },
    {
      "code": "ComponentStatus/StdErr/succeeded",
      "displayStatus": "Provisioning succeeded",
      "level": "Info",
      "message": "",
      "time": 1703361311
    }
  ]
}
```
