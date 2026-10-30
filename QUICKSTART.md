# Quick Start Guide

Fast reference for instructors and students. For full details, see [SETUP.md](SETUP.md) and [STUDENT_GUIDE.md](STUDENT_GUIDE.md).

---

## For Instructors: Setup Summary

**Time:** 30 minutes  
**Goal:** Create infected VM for students to solve

### Step-by-Step

1. **Create VM**
   - Install Windows 10 in VirtualBox
   - Username: `analyst` / Password: `IncidentResponse2024!`

2. **Generate Keys**
   - Run PowerShell script to create RSA key pair
   - Save private key as `recovery-key.xml`
   - Save public key as `public-key.xml`

3. **Create Fake Documents**
   - Create text files in `C:\IncidentResponse\Documents\`
   - Include access code in HR folder: `XK9mP2vL7qN3Wy4829`

4. **Run Ransomware**
   - Execute script with embedded public key
   - Encrypts all documents to `.crypt` files
   - Drops ransom notes

5. **Hide Decryptor**
   - ZIP the decryptor script
   - Hide in deep path: `Corporate_Archives\02_Legal_And_Compliance\Litigation_Threats\Class_Action_Noise_Complaints\Zoning_And_Environmental_Shields\County_Permits\Acoustic_Exemptions\Appeals_2025\Denied\recuva\`

6. **Hide Private Key**
   - Store in Alternate Data Stream (ADS)
   - Hide in: `Corporate_Archive\03_RND_Archive_DoNotDistribute\Expiremental_Data\System_Logs_Archive_DoNotDelete\2026_Active\Node_Alpha\Telemetry\Raw\Changelogs_Deprecated\Node_Alpha_Emergency_Patch_20260518`
   - Stream name: `recovery_key`

7. **Export VM**
   - Export as `.ova` file
   - Distribute to students

**What Students Get:** Infected VM with encrypted files  
**What You Keep:** Solution guide and backup keys

---

## For Students: Solution Summary

**Time:** 3-4 hours  
**Goal:** Find decryptor and private key, decrypt files, find access code

### Phase 1: Find Malware (30 min)

Check registry for persistence:
```powershell
Get-ItemProperty "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run"
Follow path to malware:

powershell
cd "C:\Windows\System32\Tasks\Maintenance"
Phase 2: Find Decryptor (1 hour)
Navigate to deep folder structure:

powershell
cd "C:\Corporate_Archives\02_Legal_And_Compliance\Litigation_Threats\Class_Action_Noise_Complaints\Zoning_And_Environmental_Shields\County_Permits\Acoustic_Exemptions\Appeals_2025\Denied"
Show hidden folder:

powershell
dir -Force
Enter recuva folder and extract ZIP:

powershell
cd recuva
Expand-Archive -Path "tools.zip" -DestinationPath "C:\Tools"
Phase 3: Find Private Key (1 hour)
Navigate to R&D folder:

powershell
cd "C:\Corporate_Archive\03_RND_Archive_DoNotDistribute\Expiremental_Data\System_Logs_Archive_DoNotDelete\2026_Active\Node_Alpha\Telemetry\Raw\Changelogs_Deprecated"
Show hidden file:

powershell
dir -Force
Check for ADS streams:

powershell
Get-Item "Node_Alpha_Emergency_Patch_20260518" -Stream *
Extract private key:

powershell
Get-Content "Node_Alpha_Emergency_Patch_20260518" -Stream recovery_key | Out-File "C:\Tools\private-key.xml"
Phase 4: Decrypt (20 min)
Run decryptor:

powershell
cd C:\Tools
.\decryptor.ps1 -TargetDir "C:\IncidentResponse\Documents" -KeyFile ".\private-key.xml"
Phase 5: Find Access Code (10 min)
Search decrypted files:

powershell
Get-Content "C:\IncidentResponse\Documents\HR\Employee-Directory-2024.txt" | Select-String "Recovery Code"
Found: XK9mP2vL7qN3Wy4829

Submit this code to complete the challenge.

Quick Command Reference
Task	Command
Show hidden files	dir -Force or Get-ChildItem -Force
Check ADS	Get-Item "filename" -Stream *
Extract from ADS	Get-Content "file" -Stream streamname > output
Extract ZIP	Expand-Archive -Path "zipfile" -DestinationPath "folder"
Search text	Select-String -Pattern "text" -Path "file"

## See SETUP.md and STUDENT_GUIDE.md for complete details.
