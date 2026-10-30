# Ransomware CTF Challenge

A realistic ransomware simulation for cybersecurity training.

## Quick Start

New to this project? Start here:

- **[QUICKSTART.md](QUICKSTART.md)** - Fast overview for both instructors and students
- **[SETUP.md](SETUP.md)** - Detailed instructor setup guide
- **[STUDENT_GUIDE.md](STUDENT_GUIDE.md)** - Step-by-step student walkthrough

## What It Is

Students analyze a compromised Windows VM, find hidden decryption tools in deep directory structures, extract RSA keys from Alternate Data Streams (ADS), and recover encrypted files.

## Why I Built It

To teach incident response, digital forensics, and cryptography in a safe, controlled environment. This hands-on approach helps students understand both attacker techniques and defensive countermeasures.

## What You Learn

- PowerShell forensics and Windows internals
- Registry analysis and persistence mechanisms
- Alternate Data Streams (ADS) detection and extraction
- Hybrid encryption (RSA-2048 + AES-256-CBC)
- Deep directory traversal and hidden file discovery

## Project Structure
Corporate_Archives/ # Decryptor hidden deep here
└── 02_Legal_And_Compliance/
└── Litigation_Threats/
└── .../Denied/recuva/

Corporate_Archive/ # Private key in ADS here
└── 03_RND_Archive_DoNotDistribute/
└── .../Changelogs_Deprecated/

IncidentResponse/ # Target files to encrypt
└── Documents/
├── Projects/
├── Financial/
├── HR/ # Access code hidden here
└── IT/

Windows/ # Ransomware hides here
└── System32/
└── Tasks/
└── Maintenance/

## Encryption Details

| Component | Implementation |
|-----------|---------------|
| Asymmetric | RSA-2048-OAEP |
| Symmetric | AES-256-CBC |
| Key Management | Unique AES key per file, encrypted with RSA public key |
| File Marker | `ENCRYPTED` (9 bytes) |
| Extension | `.crypt` |

## Quick Reference

| Aspect | Details |
|--------|---------|
| **Difficulty** | Medium-Hard |
| **Time** | 3-4 hours |
| **Platform** | Windows 10 VM |
| **Login** | analyst / IncidentResponse2024! |
| **Access Code** | XK9mP2vL7qN3Wy4829 |

## Files

| File | Purpose |
|------|---------|
| [QUICKSTART.md](QUICKSTART.md) | Fast setup and solution summaries |
| [SETUP.md](SETUP.md) | Complete instructor setup guide |
| [STUDENT_GUIDE.md](STUDENT_GUIDE.md) | Detailed student walkthrough |
| [LICENSE](LICENSE) | MIT License |

## Author

Created for educational cybersecurity training.

## License

MIT License - See [LICENSE](LICENSE) file for details.

## Disclaimer

This project is for **educational purposes only**. Use only in isolated, authorized environments for security training.
File 2: QUICKSTART.md
markdown
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
