# Powershell-scripts
$org = "your-organization"
$project = "your-project"
$connectionId = "service-connection-id" # Find this in the service connection URL
$pat = "your-personal-access-token" # Requires 'Service Connections Read & Query' scope

$base64Auth = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(":$pat"))
$url = "https://dev.azure.com/$org/$project/_apis/serviceendpoint/endpoints/$connectionId?api-version=7.1-preview.4"

$body = @{ isReady = $false } | ConvertTo-Json # Set isReady=$false to disable

Invoke-RestMethod -Uri $url -Method Patch -Body $body -ContentType "application/json" `
-Headers @{Authorization = "Basic $base64Auth"}
