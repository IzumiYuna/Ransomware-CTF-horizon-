---

## File 4: STUDENT_GUIDE.md

```markdown
# Student Walkthrough - Ransomware Incident Response

## Your Mission

A company workstation has been hit by ransomware. Your job:
1. Analyze the attack
2. Find the decryption tools
3. Recover the files
4. Find the hidden access code

**Time:** 3-4 hours  
**Difficulty:** Medium-Hard

## What You Start With

- VM file: `Ransomware-Challenge.ova`
- Login: `analyst` / `IncidentResponse2024!`

## Phase 1: Initial Assessment (15 min)

### Login and Look Around
Username: analyst
Password: IncidentResponse2024!

**What you see:**
- Desktop file: `URGENT_README.txt`
- Files with `.crypt` extension

### Check Encrypted Files
```powershell
cd C:\IncidentResponse\Documents
dir
Result: All files have .crypt extension. They're encrypted.

Read the Ransom Note
powershell
Get-Content "C:\IncidentResponse\Documents\README.txt"
Shows ransom demand.

Phase 2: Find the Malware (30 min)
Check Persistence
powershell
Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"
Found: WindowsUpdate pointing to a PowerShell script.

Locate the Malware
powershell
cd "C:\Windows\System32\Tasks\Maintenance"
dir
Found: svchost.ps1 - this is the ransomware.

Analyze It
powershell
Get-Content "svchost.ps1" | Select-Object -First 50
Learn: It uses RSA+AES encryption. The public key is embedded.

Phase 3: Find the Decryptor (45 min)
Search for Archives
powershell
Get-ChildItem C:\ -Recurse -Filter "*.zip" -ErrorAction SilentlyContinue
Found: Path to a ZIP file deep in Corporate_Archives.

Navigate There
powershell
cd "C:\Corporate_Archives\02_Legal_And_Compliance\Litigation_Threats\Class_Action_Noise_Complaints\Zoning_And_Environmental_Shields\County_Permits\Acoustic_Exemptions\Appeals_2025\Denied"
Show Hidden Items
powershell
dir -Force
Found: recuva folder (hidden).

Extract Tools
powershell
cd recuva
Expand-Archive -Path "tools.zip" -DestinationPath "C:\Tools"
Found: decryptor.ps1

Phase 4: Find the Private Key (60 min)
Navigate to R&D
powershell
cd "C:\Corporate_Archive\03_RND_Archive_DoNotDistribute\Expiremental_Data\System_Logs_Archive_DoNotDelete\2026_Active\Node_Alpha\Telemetry\Raw\Changelogs_Deprecated"
Show Hidden Files
powershell
dir -Force
Found: Node_Alpha_Emergency_Patch_20260518 (hidden file)

Check for Alternate Data Streams
powershell
Get-Item "Node_Alpha_Emergency_Patch_20260518" -Stream *
Found: recovery_key stream

Extract the Key
powershell
Get-Content "Node_Alpha_Emergency_Patch_20260518" -Stream recovery_key | Out-File "C:\Tools\private-key.xml"
Phase 5: Decrypt Files (20 min)
Run the Decryptor
powershell
cd C:\Tools
.\decryptor.ps1 -TargetDir "C:\IncidentResponse\Documents" -KeyFile ".\private-key.xml"
Result: Files decrypted!

Phase 6: Find the Access Code (10 min)
Search Recovered Files
powershell
Get-Content "C:\IncidentResponse\Documents\Projects\secret.txt"
Found: Code: XK9mP2vL7qN3Wy4829

Submission
Submit this code to complete the challenge:

XK9mP2vL7qN3Wy4829
Complete Command List
powershell
# Assessment
cd C:\IncidentResponse\Documents
dir

# Find malware
Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"
cd "C:\Windows\System32\Tasks\Maintenance"

# Find decryptor
cd "C:\Corporate_Archives\02_Legal_And_Compliance\Litigation_Threats\Class_Action_Noise_Complaints\Zoning_And_Environmental_Shields\County_Permits\Acoustic_Exemptions\Appeals_2025\Denied"
dir -Force
cd recuva
Expand-Archive "tools.zip" -DestinationPath "C:\Tools"

# Find private key
cd "C:\Corporate_Archive\03_RND_Archive_DoNotDistribute\Expiremental_Data\System_Logs_Archive_DoNotDelete\2026_Active\Node_Alpha\Telemetry\Raw\Changelogs_Deprecated"
dir -Force
Get-Content "Node_Alpha_Emergency_Patch_20260518" -Stream recovery_key | Out-File "C:\Tools\private-key.xml"

# Decrypt
cd C:\Tools
.\decryptor.ps1 -TargetDir "C:\IncidentResponse\Documents" -KeyFile ".\private-key.xml"

# Find code
Get-Content "C:\IncidentResponse\Documents\Projects\secret.txt"
What You Learned
Registry forensics
Hidden file discovery
Alternate Data Streams (ADS)
Deep directory navigation
Hybrid encryption (RSA+AES)
Congratulations! You solved the challenge.
