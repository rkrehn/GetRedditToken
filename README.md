# GetRedditToken
This tutorial is intended for Reddit developers who need to retrieve their API access token. I spent a significant amount of time trying to figure out how to do this, and the recommended guide did not help, as I kept getting various errors. I hope this tutorial will be more helpful to others.

An API access token is required to access the Reddit API. To retrieve your access token, you can use the following PowerShell script.

# Setup

Your first step will need to be creating a Reddit app

1. Go to [https://www.reddit.com/prefs/apps](https://www.reddit.com/prefs/apps) once you've logged into Reddit.
2. Click "Create App"
3. Enter name, select "script", and enter everything. None of the following fields really matter since they won't be used.
![image](https://github.com/rkrehn/GetRedditToken/assets/15220483/158d363c-7892-47e3-a0da-b372b3457c5b)
4. Click "Create App"

# PowerShell

Once you have an App ID and a secret key, you will need to save the following into a text document:

```PowerShell
$Headers = @{
    'User-Agent'  = 'User agent'
    'Authorization' = 'Basic ' + [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes('[App Id]:[Secret Key]'))
}

$Body = @{
    'grant_type' = 'password'
    'username'   = '[username]'
    'password'   = '[password]'
}

$response = Invoke-WebRequest -Uri 'https://www.reddit.com/api/v1/access_token' -Method POST -Headers $Headers -Body $Body

# Display the response content
$responseContent = $response.Content
Write-Host "Response Content:"
Write-Host $responseContent
```

In the above, replace anything in the brackets [] with the correct information. For example, where it says **[App Id]**, then replace it your App ID from the above steps. Mine looks something like the following, except I scrambled some of the letters for security reasons:

```PowerShell
$Headers = @{
    'User-Agent'  = 'User agent'
    'Authorization' = 'Basic ' + [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes('pasFsdLQ3swX3dsfdzx35w:aeEGAROQdwqPftDD5UBCf_3rDGTnbs'))
}

$Body = @{
    'grant_type' = 'password'
    'username'   = 'lordkrehn'
    'password'   = 'l0ln1c3try'
}

$response = Invoke-WebRequest -Uri 'https://www.reddit.com/api/v1/access_token' -Method POST -Headers $Headers -Body $Body

# Display the response content
$responseContent = $response.Content
Write-Host "Response Content:"
Write-Host $responseContent
```

Save this text file as a PowerShell script (*.ps1) somewhere you can easily access it. I saved it on my desktop as "new.ps1". 

# Running the PowerShell

Now that you've created the script, you'll need to run it.

1. Open PowerShell as an Administrator
2. Type the following and hit enter
```
Set -ExecutionPolicy RemoteSigned
```
3. Type "Y" and hit enter
4. Navigate to your directory using the CD command. Mine was saved on the desktop
```
CD C:\Users\Me\Desktop
```
5. Now, execute the script
```
\new.ps1
```
6. If you set up everything correctly, you'll receive a response with your access token. It will be the long block of text following **access_token**:

![image](https://github.com/rkrehn/GetRedditToken/assets/15220483/253001d6-54e1-4e75-ad94-13bacb90acbd)

