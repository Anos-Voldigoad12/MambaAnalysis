# Mamba (HDDCryptor) – Malware Analysis

Analysis artifacts for a Windows ransomware sample belonging to the **HDDCryptor** family (also tracked as *Mamba*; detections include `Trojan.Ransom.HDDCryptor` and `exe.trojan.dcryptor`). The sample abuses the open-source **DiskCryptor** utility to encrypt disks and replace the MBR with a ransom bootloader.

> ⚠️ **WARNING – LIVE MALWARE**
> `Ransomware.Mamba.zip` contains a real, functional ransomware sample. Handle it **only inside an isolated, disposable lab VM** (e.g. FLARE-VM) with no network access to production systems or shared drives. Never run it on a host you care about. This repository is for education and defensive research only.

## 🔐 Sample archive password

| Archive | Password |
| --- | --- |
| `Ransomware.Mamba.zip` | **`infected`** |

## Sample details

| Attribute | Value |
| --- | --- |
| MD5 | `409d80bb94645fbc4a1fa61c07806883` |
| SHA-256 | `2ecc525177ed52c74ddaaacd47ad513450e85c01f2616bf179be5b576164bf63` |
| File type | PE32 executable (console), Intel 80386 |
| Requires | Administrator rights and a password passed as a command-line argument |
| Detection | VirusTotal 66/74, MalwareBazaar 17/23 |

## Repository contents (`Mamba/`)

| File | Description |
| --- | --- |
| `Malware_Analysis_Report.pdf` | Full malware analysis report: executive summary, static/dynamic analysis, MITRE ATT&CK mapping, IOCs and recommendations |
| `Analyst_notes.txt` | Raw analyst notes: hashes, imports, strings, IDA code review, dynamic observations and Regshot results |
| `ransomware_logs.txt` | Execution log written by the malware (`C:\DC22\log_file.txt`) |
| `ProcmonDump.CSV` | Process Monitor capture of the sample's runtime activity |
| `packets_20261004_205320.pcap` | Network capture taken during execution (no C2 traffic observed) |
| `Boot.png` | Screenshot of the ransom note shown after reboot |
| `DroppedFiles.png` | Screenshot of the `C:\DC22` staging folder with dropped components |
| `Ransomware.Mamba.zip` | Password-protected malware sample (password above) |

## Key findings (summary)

- Extracts embedded DiskCryptor components (driver, console, GUI, API DLL) plus `Mount.exe` and `netpass.exe` into `C:\DC22`.
- Persists as the auto-start SYSTEM service **`DefragmentService`**, masquerading as a Windows defragmentation service.
- Creates a local administrator account `mythbusters`, harvests saved network credentials and attempts to mount network shares.
- Encrypts local disks and overwrites the MBR; the victim sees *"Missing operating system"* and a ransom note with a contact e-mail.
- No network (C2) communication was observed.
- The password `123456` seen in the logs and service entry was entered by the analyst to execute the sample; it is not an attacker-chosen key.

## Analysis environment

FLARE-VM with HashMyFiles, Detect It Easy, CFF Explorer, HxD, Strings, IDA, Regshot, Process Monitor and FakeNet.

## Disclaimer

Provided for research and educational purposes. The authors accept no responsibility for misuse or damage resulting from handling the sample.
