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

```bash
ssh monitor@ssh-server-vm.santaworkshopgeeseislands.org -i  mysshkey -i mysshkey.cert
```
```bash
monitor@ssh-server-vm:/$ echo "Management Token"
curl -s -f -H "$HEADER" "$URL/identity/oauth2/token?api-version=$API_VERSION&resource=https://management.azure.com/"
Management Token
{"access_token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjVCM25SeHRRN2ppOGVORGMzRnkwNUtmOTdaRSIsImtpZCI6IjVCM25SeHRRN2ppOGVORGMzRnkwNUtmOTdaRSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzkwYTM4ZWRhLTQwMDYtNGRkNS05MjRjLTZjYTU1Y2FjYzE0ZC8iLCJpYXQiOjE3MDM0NzU5NzMsIm5iZiI6MTcwMzQ3NTk3MywiZXhwIjoxNzAzNTYyNjczLCJhaW8iOiJFMlZnWUNnME1pbThYSC9rZFlIaE5kNnBwbTU2QUE9PSIsImFwcGlkIjoiYjg0ZTA2ZDMtYWJhMS00YmNjLTk2MjYtMmUwZDc2Y2JhMmNlIiwiYXBwaWRhY3IiOiIyIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvOTBhMzhlZGEtNDAwNi00ZGQ1LTkyNGMtNmNhNTVjYWNjMTRkLyIsImlkdHlwIjoiYXBwIiwib2lkIjoiNjAwYTNiYzgtN2UyYy00NGU1LThhMjctMThjM2ViOTYzMDYwIiwicmgiOiIwLkFGRUEybzZqa0FaQTFVMlNUR3lsWEt6QlRVWklmM2tBdXRkUHVrUGF3ZmoyTUJQUUFBQS4iLCJzdWIiOiI2MDBhM2JjOC03ZTJjLTQ0ZTUtOGEyNy0xOGMzZWI5NjMwNjAiLCJ0aWQiOiI5MGEzOGVkYS00MDA2LTRkZDUtOTI0Yy02Y2E1NWNhY2MxNGQiLCJ1dGkiOiI0R1JER3QydmswdWdtLUlKU2FhM0FnIiwidmVyIjoiMS4wIiwieG1zX2F6X3JpZCI6Ii9zdWJzY3JpcHRpb25zLzJiMDk0MmYzLTliY2EtNDg0Yi1hNTA4LWFiZGFlMmRiNWU2NC9yZXNvdXJjZWdyb3Vwcy9ub3J0aHBvbGUtcmcxL3Byb3ZpZGVycy9NaWNyb3NvZnQuQ29tcHV0ZS92aXJ0dWFsTWFjaGluZXMvc3NoLXNlcnZlci12bSIsInhtc19jYWUiOiIxIiwieG1zX21pcmlkIjoiL3N1YnNjcmlwdGlvbnMvMmIwOTQyZjMtOWJjYS00ODRiLWE1MDgtYWJkYWUyZGI1ZTY0L3Jlc291cmNlZ3JvdXBzL25vcnRocG9sZS1yZzEvcHJvdmlkZXJzL01pY3Jvc29mdC5NYW5hZ2VkSWRlbnRpdHkvdXNlckFzc2lnbmVkSWRlbnRpdGllcy9ub3J0aHBvbGUtc3NoLXNlcnZlci1pZGVudGl0eSIsInhtc190Y2R0IjoxNjk4NDE3NTU3fQ.FHt0FNZzGuH1VV-0CDSjz3dwfoXLFUQED5mcdGNQQGvjMp54cRO6RT5jN-HGkBl8JjPAlQ3SterHNfEYsBvc-ygL13F1DpGRj3Fe8guVTbqtykh4WDWgHlRhJHbNAWxYcZADdREfEvJD7I0eLdjyClK6vWb8s7dFTMKDpQNC0DgMMiShuf2fIPf4xXM61nF6-tKW9jOKzwCym3S7fwyro1D1LlFfmSg3pPoRPT0jhYb7JC8xLuvjYKywzZcrTtGfad7aXroLQVxnQF2edeRLbZIml2qZhT3ZgspPHJDD6jXo1Gyu2rZe4iRd74hhbHRaeGaWd8r0DNrWUNe6Dz-xIg","client_id":"b84e06d3-aba1-4bcc-9626-2e0d76cba2ce","expires_in":"85446","expires_on":"1703562673","ext_expires_in":"86399","not_before":"1703475973","resource":"https://management.azure.com/","token_type":"Bearer"}
```
```bash
curl -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjVCM25SeHRRN2ppOGVORGMzRnkwNUtmOTdaRSIsImtpZCI6IjVCM25SeHRRN2ppOGVORGMzRnkwNUtmOTdaRSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzkwYTM4ZWRhLTQwMDYtNGRkNS05MjRjLTZjYTU1Y2FjYzE0ZC8iLCJpYXQiOjE3MDM0NzU5NzMsIm5iZiI6MTcwMzQ3NTk3MywiZXhwIjoxNzAzNTYyNjczLCJhaW8iOiJFMlZnWUNnME1pbThYSC9rZFlIaE5kNnBwbTU2QUE9PSIsImFwcGlkIjoiYjg0ZTA2ZDMtYWJhMS00YmNjLTk2MjYtMmUwZDc2Y2JhMmNlIiwiYXBwaWRhY3IiOiIyIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvOTBhMzhlZGEtNDAwNi00ZGQ1LTkyNGMtNmNhNTVjYWNjMTRkLyIsImlkdHlwIjoiYXBwIiwib2lkIjoiNjAwYTNiYzgtN2UyYy00NGU1LThhMjctMThjM2ViOTYzMDYwIiwicmgiOiIwLkFGRUEybzZqa0FaQTFVMlNUR3lsWEt6QlRVWklmM2tBdXRkUHVrUGF3ZmoyTUJQUUFBQS4iLCJzdWIiOiI2MDBhM2JjOC03ZTJjLTQ0ZTUtOGEyNy0xOGMzZWI5NjMwNjAiLCJ0aWQiOiI5MGEzOGVkYS00MDA2LTRkZDUtOTI0Yy02Y2E1NWNhY2MxNGQiLCJ1dGkiOiI0R1JER3QydmswdWdtLUlKU2FhM0FnIiwidmVyIjoiMS4wIiwieG1zX2F6X3JpZCI6Ii9zdWJzY3JpcHRpb25zLzJiMDk0MmYzLTliY2EtNDg0Yi1hNTA4LWFiZGFlMmRiNWU2NC9yZXNvdXJjZWdyb3Vwcy9ub3J0aHBvbGUtcmcxL3Byb3ZpZGVycy9NaWNyb3NvZnQuQ29tcHV0ZS92aXJ0dWFsTWFjaGluZXMvc3NoLXNlcnZlci12bSIsInhtc19jYWUiOiIxIiwieG1zX21pcmlkIjoiL3N1YnNjcmlwdGlvbnMvMmIwOTQyZjMtOWJjYS00ODRiLWE1MDgtYWJkYWUyZGI1ZTY0L3Jlc291cmNlZ3JvdXBzL25vcnRocG9sZS1yZzEvcHJvdmlkZXJzL01pY3Jvc29mdC5NYW5hZ2VkSWRlbnRpdHkvdXNlckFzc2lnbmVkSWRlbnRpdGllcy9ub3J0aHBvbGUtc3NoLXNlcnZlci1pZGVudGl0eSIsInhtc190Y2R0IjoxNjk4NDE3NTU3fQ.FHt0FNZzGuH1VV-0CDSjz3dwfoXLFUQED5mcdGNQQGvjMp54cRO6RT5jN-HGkBl8JjPAlQ3SterHNfEYsBvc-ygL13F1DpGRj3Fe8guVTbqtykh4WDWgHlRhJHbNAWxYcZADdREfEvJD7I0eLdjyClK6vWb8s7dFTMKDpQNC0DgMMiShuf2fIPf4xXM61nF6-tKW9jOKzwCym3S7fwyro1D1LlFfmSg3pPoRPT0jhYb7JC8xLuvjYKywzZcrTtGfad7aXroLQVxnQF2edeRLbZIml2qZhT3ZgspPHJDD6jXo1Gyu2rZe4iRd74hhbHRaeGaWd8r0DNrWUNe6Dz-xIg" -X GET "https://management.azure.com/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/northpole-rg1/providers/Microsoft.Web/sites/northpole-ssh-certs-fa/functions?api-version=2021-02-01" -H "Content-Type: application/json"
```
```json
{"value":[{"id":"/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/northpole-rg1/providers/Microsoft.Web/sites/northpole-ssh-certs-fa/functions/create_cert","name":"northpole-ssh-certs-fa/create_cert","type":"Microsoft.Web/sites/functions","location":"East US","properties":{"name":"create_cert","function_app_id":null,"script_root_path_href":null,"script_href":"https://northpole-ssh-certs-fa.azurewebsites.net/admin/vfs/home/site/wwwroot/function_app.py","config_href":null,"test_data_href":"https://northpole-ssh-certs-fa.azurewebsites.net/admin/vfs/tmp/FunctionsData/create_cert.dat","secrets_file_href":null,"href":"https://northpole-ssh-certs-fa.azurewebsites.net/admin/functions/create_cert","config":{"name":"create_cert","entryPoint":"create_cert","scriptFile":"function_app.py","language":"python","functionDirectory":"/home/site/wwwroot","bindings":[{"direction":"IN","type":"httpTrigger","name":"req","methods":["GET","POST"],"authLevel":"FUNCTION","route":"create-cert"},{"direction":"OUT","type":"http","name":"$return"}]},"files":null,"test_data":"","invoke_url_template":"https://northpole-ssh-certs-fa.azurewebsites.net/api/create-cert","language":"python","isDisabled":false}}],"nextLink":null,"id":null} 
```
```bash
curl -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6IjVCM25SeHRRN2ppOGVORGMzRnkwNUtmOTdaRSIsImtpZCI6IjVCM25SeHRRN2ppOGVORGMzRnkwNUtmOTdaRSJ9.eyJhdWQiOiJodHRwczovL21hbmFnZW1lbnQuYXp1cmUuY29tLyIsImlzcyI6Imh0dHBzOi8vc3RzLndpbmRvd3MubmV0LzkwYTM4ZWRhLTQwMDYtNGRkNS05MjRjLTZjYTU1Y2FjYzE0ZC8iLCJpYXQiOjE3MDM0NzU5NzMsIm5iZiI6MTcwMzQ3NTk3MywiZXhwIjoxNzAzNTYyNjczLCJhaW8iOiJFMlZnWUNnME1pbThYSC9rZFlIaE5kNnBwbTU2QUE9PSIsImFwcGlkIjoiYjg0ZTA2ZDMtYWJhMS00YmNjLTk2MjYtMmUwZDc2Y2JhMmNlIiwiYXBwaWRhY3IiOiIyIiwiaWRwIjoiaHR0cHM6Ly9zdHMud2luZG93cy5uZXQvOTBhMzhlZGEtNDAwNi00ZGQ1LTkyNGMtNmNhNTVjYWNjMTRkLyIsImlkdHlwIjoiYXBwIiwib2lkIjoiNjAwYTNiYzgtN2UyYy00NGU1LThhMjctMThjM2ViOTYzMDYwIiwicmgiOiIwLkFGRUEybzZqa0FaQTFVMlNUR3lsWEt6QlRVWklmM2tBdXRkUHVrUGF3ZmoyTUJQUUFBQS4iLCJzdWIiOiI2MDBhM2JjOC03ZTJjLTQ0ZTUtOGEyNy0xOGMzZWI5NjMwNjAiLCJ0aWQiOiI5MGEzOGVkYS00MDA2LTRkZDUtOTI0Yy02Y2E1NWNhY2MxNGQiLCJ1dGkiOiI0R1JER3QydmswdWdtLUlKU2FhM0FnIiwidmVyIjoiMS4wIiwieG1zX2F6X3JpZCI6Ii9zdWJzY3JpcHRpb25zLzJiMDk0MmYzLTliY2EtNDg0Yi1hNTA4LWFiZGFlMmRiNWU2NC9yZXNvdXJjZWdyb3Vwcy9ub3J0aHBvbGUtcmcxL3Byb3ZpZGVycy9NaWNyb3NvZnQuQ29tcHV0ZS92aXJ0dWFsTWFjaGluZXMvc3NoLXNlcnZlci12bSIsInhtc19jYWUiOiIxIiwieG1zX21pcmlkIjoiL3N1YnNjcmlwdGlvbnMvMmIwOTQyZjMtOWJjYS00ODRiLWE1MDgtYWJkYWUyZGI1ZTY0L3Jlc291cmNlZ3JvdXBzL25vcnRocG9sZS1yZzEvcHJvdmlkZXJzL01pY3Jvc29mdC5NYW5hZ2VkSWRlbnRpdHkvdXNlckFzc2lnbmVkSWRlbnRpdGllcy9ub3J0aHBvbGUtc3NoLXNlcnZlci1pZGVudGl0eSIsInhtc190Y2R0IjoxNjk4NDE3NTU3fQ.FHt0FNZzGuH1VV-0CDSjz3dwfoXLFUQED5mcdGNQQGvjMp54cRO6RT5jN-HGkBl8JjPAlQ3SterHNfEYsBvc-ygL13F1DpGRj3Fe8guVTbqtykh4WDWgHlRhJHbNAWxYcZADdREfEvJD7I0eLdjyClK6vWb8s7dFTMKDpQNC0DgMMiShuf2fIPf4xXM61nF6-tKW9jOKzwCym3S7fwyro1D1LlFfmSg3pPoRPT0jhYb7JC8xLuvjYKywzZcrTtGfad7aXroLQVxnQF2edeRLbZIml2qZhT3ZgspPHJDD6jXo1Gyu2rZe4iRd74hhbHRaeGaWd8r0DNrWUNe6Dz-xIg" -X GET "https://management.azure.com/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/northpole-rg1/providers/Microsoft.Web/sites/northpole-ssh-certs-fa/sourcecontrols/web?api-version=2021-02-01" -H "Content-Type: application/json"
```
```json
{"id":"/subscriptions/2b0942f3-9bca-484b-a508-abdae2db5e64/resourceGroups/northpole-rg1/providers/Microsoft.Web/sites/northpole-ssh-certs-fa/sourcecontrols/web","name":"northpole-ssh-certs-fa","type":"Microsoft.Web/sites/sourcecontrols","location":"East US","tags":{"project":"northpole-ssh-certs","create-cert-func-url-path":"/api/create-cert?code=candy-cane-twirl"},"properties":{"repoUrl":"https://github.com/SantaWorkshopGeeseIslandsDevOps/northpole-ssh-certs-fa","branch":"main","isManualIntegration":false,"isGitHubAction":true,"deploymentRollbackEnabled":false,"isMercurial":false,"provisioningState":"Succeeded","gitHubActionConfiguration":{"codeConfiguration":null,"containerConfiguration":null,"isLinux":true,"generateWorkflowFile":true,"workflowSettings":{"appType":"functionapp","publishType":"code","os":"linux","variables":{"runtimeVersion":"3.11"},"runtimeStack":"python","workflowApiVersion":"2020-12-01","useCanaryFusionServer":false,"authType":"publishprofile"}}}}
```
```bash
ssh alabaster@ssh-server-vm.santaworkshopgeeseislands.org -i  mysshkey.alabaster -i mysshkey.alabaster.cert -v
```
```
alabaster_todo.md  impacket
alabaster@ssh-server-vm:~$ cat alabaster_todo.md 
# Geese Islands IT & Security Todo List

- [X] Sleigh GPS Upgrade: Integrate the new "Island Hopper" module into Santa's sleigh GPS. Ensure Rudolph's red nose doesn't interfere with the signal.
- [X] Reindeer Wi-Fi Antlers: Test out the new Wi-Fi boosting antler extensions on Dasher and Dancer. Perfect for those beach-side internet browsing sessions.
- [ ] Palm Tree Server Cooling: Make use of the island's natural shade. Relocate servers under palm trees for optimal cooling. Remember to watch out for falling coconuts!
- [ ] Eggnog Firewall: Upgrade the North Pole's firewall to the new EggnogOS version. Ensure it blocks any Grinch-related cyber threats effectively.
- [ ] Gingerbread Cookie Cache: Implement a gingerbread cookie caching mechanism to speed up data retrieval times. Don't let Santa eat the cache!
- [ ] Toy Workshop VPN: Establish a secure VPN tunnel back to the main toy workshop so the elves can securely access to the toy blueprints.
- [ ] Festive 2FA: Roll out the new two-factor authentication system where the second factor is singing a Christmas carol. Jingle Bells is said to be the most secure.
```
