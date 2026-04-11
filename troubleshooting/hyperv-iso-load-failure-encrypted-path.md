# Title: Unable to Load ISO into VM Due to Encrypted File Path

## Environment
- Host Machine: Windows 11 Pro (Lenovo ThinkPad)
- Virtualization: Hyper-V
- VM Type: Windows Server 2022
- File Location: Home Lab directory (user profile path)
- Security Tool: Windows File Encryption (EFS)

## Problem Description
The virtual machine failed to recognize or boot from the Windows Server 2022 ISO file during setup.

## Symptoms
- ISO file appeared selectable but would not load properly
- VM failed to boot from the ISO
- No explicit error message, but installation would not initiate
- Hyper-V behaved as if the ISO was inaccessible or invalid

## Initial Hypothesis
- Incorrect ISO file
- Corrupted download
- Misconfigured VM settings (Generation, boot order)

## Troubleshooting Steps Taken
1. Verified ISO integrity and file size
2. Confirmed correct Hyper-V settings (Generation 2, proper boot order)
3. Reattached ISO to VM multiple times
4. Reviewed file path and permissions
5. Identified that the Home Lab folder was encrypted using Windows EFS
6. Removed encryption from the folder and all subfolders/files
7. Ensured the ISO was in a standard, accessible directory
8. Reattached ISO and rebooted VM

## Root Cause
The ISO file was stored in an encrypted directory. Hyper-V was unable to properly access encrypted files, preventing the VM from loading the ISO.

## Resolution
- Removed encryption from the Home Lab folder and all contents
- Ensured the ISO file was stored in a non-encrypted, accessible path
- Reattached the ISO to the VM

## Validation
- VM successfully booted from the ISO
- Windows Server installation process initiated without issue

## Lessons Learned
- Avoid storing VM resources (ISO files, VHDs) in encrypted directories
- Always verify file accessibility, not just file presence
- Encryption can interfere with virtualization tools if not properly configured

## References
- Microsoft Docs: Hyper-V VM Setup
- Windows EFS Encryption Documentation
