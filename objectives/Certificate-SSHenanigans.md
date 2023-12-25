## Objective
Go to Pixel Island and review Alabaster Snowball's new SSH certificate configuration and Azure Function App. What type of cookie cache is Alabaster planning to implement?
## Solution
#### Alabaster Snowball
Hello there! Alabaster Snowball at your service.

I could use your help with my fancy new Azure server at ssh-server-vm.santaworkshopgeeseislands.org.

ChatNPT suggested I upgrade the host to use SSH certificates, such a great idea!

It even generated ready-to-deploy code for an Azure Function App (https://northpole-ssh-certs-fa.azurewebsites.net/api/create-cert?code=candy-cane-twirl) so elves can request their own certificates. What a timesaver!

I'm a little wary though. I'd appreciate it if you could take a peek and confirm everything's secure before I deploy this configuration to all the Geese Islands servers.

Generate yourself a certificate and use the monitor account to access the host. See if you can grab my TODO list.

If you haven't heard of SSH certificates, Thomas Bouve gave an introductory talk and demo on that topic recently.

Oh, and if you need to peek at the Function App code, there's a handy Azure REST API endpoint ( https://learn.microsoft.com/en-us/rest/api/appservice/web-apps/get-source-control ) which will give you details about how the Function App is deployed.
