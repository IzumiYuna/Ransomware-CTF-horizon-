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


powershell
Get-Content "C:\IncidentResponse\Documents\HR\Employee-Directory-2024.txt" | Select-String "Recovery Code"
Found: XK9mP2vL7qN3Wy4829

Submit this code to complete the challenge.
