# Config
$org = "your-organization-name"
$project = "your-project-name"
$connectionId = "service-connection-guid" # Get from service connection URL
$pat = "your-pat-with-service-connections-manage-scope"

# Auth
$base64Auth = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(":$pat"))
$headers = @{
    Authorization = "Basic $base64Auth"
    "Content-Type" = "application/json"
}

# 1. GET current service connection details
$getUrl = "https://dev.azure.com/$org/$project/_apis/serviceendpoint/endpoints/$connectionId?api-version=7.1"
$serviceConnection = Invoke-RestMethod -Uri $getUrl -Method Get -Headers $headers

# 2. Update the connection to disable it
$serviceConnection.isReady = $false  # Disable flag

# 3. PATCH updated configuration back
$patchUrl = "https://dev.azure.com/$org/$project/_apis/serviceendpoint/endpoints/$($serviceConnection.id)?api-version=7.1"
$body = $serviceConnection | ConvertTo-Json -Depth 10
Invoke-RestMethod -Uri $patchUrl -Method Patch -Headers $headers -Body $body
