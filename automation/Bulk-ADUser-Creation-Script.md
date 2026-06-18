# Bulk Active Directory User Creation Script

## Overview

This PowerShell script automates the creation of multiple test users in Active Directory (AD). It is designed for lab environments, testing scenarios, and streamlining user provisioning workflows.

The script creates users with a predefined list of names, automatically generates usernames based on first and last names, and outputs progress as each user is created.

---

## Purpose

- **Automation**: Eliminate manual user creation for test environments
- **Consistency**: Ensure standardized naming conventions across all users
- **Efficiency**: Create 20+ users in seconds instead of minutes
- **Documentation**: Provides clear feedback on success and failures

---

## Prerequisites

- **PowerShell**: Version 5.0 or higher
- **Active Directory Module**: Installed on the Domain Controller or Admin machine
- **Permissions**: Must run as Administrator with privileges to create AD users
- **Domain**: Active Directory domain must be operational

### Verify Prerequisites

```powershell
# Check PowerShell version
$PSVersionTable.PSVersion

# Verify AD module is available
Get-Module -Name ActiveDirectory
```

---

## Script

### File: `Create-TestADUsers.ps1`

```powershell
# ============================================================================
# Bulk Active Directory User Creation Script
# Purpose: Automate test user provisioning in lab environments
# Author: Home Lab Systems Administration
# Date: June 2026
# ============================================================================

# Define parameters
$DomainName = "homelab.local"
$Password = ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force

# Define test users with notable names for easy identification
$Users = @(
    @{First="Denzel"; Last="Washington"},
    @{First="Chadwick"; Last="Boseman"},
    @{First="Morgan"; Last="Freeman"},
    @{First="Will"; Last="Smith"},
    @{First="Idris"; Last="Elba"},
    @{First="Viola"; Last="Davis"},
    @{First="Kerry"; Last="Washington"},
    @{First="Lupita"; Last="Nyongo"},
    @{First="Zendaya"; Last="Coleman"},
    @{First="Michael"; Last="BJordan"},
    @{First="Sterling"; Last="KBrown"},
    @{First="Lakeith"; Last="Stanfield"},
    @{First="Tracee"; Last="EllisRoss"},
    @{First="Issa"; Last="Rae"},
    @{First="Donald"; Last="Glover"},
    @{First="Janelle"; Last="Monae"},
    @{First="Tessa"; Last="Thompson"},
    @{First="Mahershala"; Last="Ali"},
    @{First="Giancarlo"; Last="Esposito"},
    @{First="Anthony"; Last="Mackie"},
    @{First="Michaela"; Last="Rodriguez"},
    @{First="Yara"; Last="Shahidi"},
    @{First="Colman"; Last="Domingo"},
    @{First="Courtney"; Last="Vance"},
    @{First="Danai"; Last="Gurira"}
)

# Initialize counter
$count = 0

# Display start message
Write-Host "========================================" -ForegroundColor Cyan
Write-Host "Active Directory User Creation Script" -ForegroundColor Cyan
Write-Host "========================================" -ForegroundColor Cyan
Write-Host ""
Write-Host "Creating $($Users.Count) test users in domain: $DomainName" -ForegroundColor Yellow
Write-Host ""

# Loop through each user and create them
foreach ($user in $Users) {
    $first = $user.First
    $last = $user.Last
    $username = "$($first.ToLower()).$($last.ToLower())"
    $displayname = "$first $last"
    
    try {
        # Create user with standard properties
        New-ADUser -Name $displayname `
            -GivenName $first `
            -Surname $last `
            -SamAccountName $username `
            -UserPrincipalName "$username@$DomainName" `
            -AccountPassword $Password `
            -Enabled $true `
            -WarningAction SilentlyContinue
        
        Write-Host "✓ Created user: $username" -ForegroundColor Green
        $count++
    }
    catch {
        Write-Host "✗ Failed to create $username - Error: $_" -ForegroundColor Red
    }
}

# Display completion summary
Write-Host ""
Write-Host "========================================" -ForegroundColor Cyan
Write-Host "User Creation Complete" -ForegroundColor Cyan
Write-Host "========================================" -ForegroundColor Cyan
Write-Host "Total users created: $count" -ForegroundColor Green
Write-Host "Default password: P@ssw0rd123!" -ForegroundColor Yellow
Write-Host ""
```

---

## Usage

### Step 1: Open PowerShell as Administrator

```powershell
# Right-click PowerShell and select "Run as Administrator"
```

### Step 2: Set Execution Policy (If Needed)

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

When prompted, type `Y` and press Enter.

### Step 3: Run the Script

```powershell
# Option A: If saved as a file
C:\Scripts\Create-TestADUsers.ps1

# Option B: Paste directly into PowerShell
# (Copy the entire script and right-click to paste)
```

---

## Expected Output

```
========================================
Active Directory User Creation Script
========================================

Creating 25 test users in domain: homelab.local

✓ Created user: denzel.washington
✓ Created user: chadwick.boseman
✓ Created user: morgan.freeman
✓ Created user: will.smith
✓ Created user: idris.elba
✓ Created user: viola.davis
... (19 more)

========================================
User Creation Complete
========================================
Total users created: 25
Default password: P@ssw0rd123!
```

---

## Verification

### Verify Users Were Created

```powershell
# Count total users in Active Directory
Get-ADUser -Filter * | Measure-Object

# List all created users
Get-ADUser -Filter * | Select-Object Name, SamAccountName | Format-Table

# Check specific user
Get-ADUser -Identity "denzel.washington" | Select-Object Name, Enabled, Created
```

---

## Customization

### Change Default Password

Edit this line in the script:

```powershell
# Current:
$Password = ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force

# Change to:
$Password = ConvertTo-SecureString "YourNewPassword123!" -AsPlainText -Force
```

### Modify User List

Edit the `$Users` array to add, remove, or change names:

```powershell
$Users = @(
    @{First="FirstName"; Last="LastName"},
    @{First="AnotherFirst"; Last="AnotherLast"},
    # Add more as needed
)
```

### Change Domain Name

Edit this line:

```powershell
# Current:
$DomainName = "homelab.local"

# Change to:
$DomainName = "yourdomain.com"
```

---

## Create Security Groups and Assign Users

After running this script, create groups and assign users:

```powershell
# Create groups
New-ADGroup -Name "IT_Admins" -GroupScope Global
New-ADGroup -Name "Remote_Users" -GroupScope Global
New-ADGroup -Name "Contractors" -GroupScope Global

# Assign users to groups
Add-ADGroupMember -Identity "IT_Admins" -Members "denzel.washington", "chadwick.boseman"
Add-ADGroupMember -Identity "Remote_Users" -Members "morgan.freeman", "will.smith"
Add-ADGroupMember -Identity "Contractors" -Members "idris.elba"

# Verify group membership
Get-ADGroupMember -Identity "IT_Admins"
```

---

## Troubleshooting

### Error: "User Already Exists"

If a user already exists with the same SamAccountName, the script will skip that user and continue with the next one.

**Solution**: Remove the existing user first, or modify the script to check for existing users.

### Error: "Access Denied"

You do not have permission to create users in Active Directory.

**Solution**: Ensure you are running PowerShell as Administrator and have sufficient AD permissions.

### Error: "Domain Not Found"

The script cannot communicate with the domain controller.

**Solution**: Verify that:
- You are on the domain network
- The domain name is correct
- DNS is configured properly

---

## Security Considerations

### ⚠️ Important Notes

1. **Default Password**: The default password `P@ssw0rd123!` is intentionally weak for testing. **Do not use in production**.

2. **Password Policy**: In production, enforce strong password policies via Group Policy Objects (GPOs).

3. **User Cleanup**: Delete test users after completing testing to avoid cluttering the directory.

4. **Audit Logging**: Enable AD auditing to log all user creation events for compliance and security.

---

## Related Documentation

- [Active Directory Deployment](../active-directory/README.md)
- [Group Policy Configuration](../active-directory/gpo-workstation-security.md)
- [User Management Best Practices](../procedures/)

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | June 2026 | Initial script creation |

---

## Author

Home Lab Systems Administration  
Lab Environment: OPNsense + Cisco SG300 + Windows Server 2022 AD

---

## License

This script is provided as-is for educational and lab purposes.

---
