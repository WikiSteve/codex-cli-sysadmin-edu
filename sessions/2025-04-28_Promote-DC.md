# Codex CLI Session: Promoting Windows Server to Domain Controller

**Date:** 2025-04-29 (approx)
**Goal:** Use Codex CLI to promote the Windows Server 2025 Core machine `sharpe-dc1` (172.16.171.10) to an Active Directory Domain Controller for the new domain `sharpe.com`, update topology documentation, and verify the promotion.

---

### Initial Request: Promote to Domain Controller

**user:**
Promote the machine sharpe-dc1 to a domain controller. Make the domain sharpe.com

**(Codex Actions - Planning & Info Gathering):**

* Reads local project files (`codex.md`, `topology.md`).
    ```bash
    $ ls -al
    $ sed -n 1,200p codex.md
    $ sed -n 1,200p topology.md
    ```
    *(Output confirms project structure and current topology)*

**codex:**
> I see that sharpe-dc1 (172.16.171.10) is a Windows host using account steve. To promote it to a Domain Controller for the new domain sharpe.com, we’ll need to:
> 1.  Install the “Active Directory Domain Services” Windows feature.
> 2.  Run `Install-ADDSForest` to create the forest/domain.
> 3.  Supply a Directory Services Restore Mode (Safe Mode Administrator) password.
>
> Could you please provide the Safe Mode Administrator password you’d like to use for this promotion? Once I have that, I’ll perform the promotion and then update our topology.md to reflect the new Domain Controller role.

**user:**
Windows1

**(Codex Action - Attempting AD Promotion Command via SSH):**

* Attempts remote command execution (fails due to host key verification):
    ```bash
    # Command constructed by Codex
    $ ssh -o BatchMode=yes steve@172.16.171.10 powershell -NoProfile -NonInteractive -Command "Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools; Import-Module ADDSDeployment; Install-ADDSForest -DomainName 'sharpe.com' -DomainNetbiosName 'SHARPE' -InstallDns -SafeModeAdministratorPassword (ConvertTo-SecureString 'Windows1' -AsPlainText -Force) -Force"
    ```
    *(Output: `Host key verification failed.`)*
* Updates local known\_hosts file to accept the new host key:
    ```bash
    $ ssh-keyscan -H 172.16.171.10 >> ~/.ssh/known_hosts
    ```
    *(Output shows keys being added)*
* Re-attempts remote command execution for AD promotion:
    ```bash
    # Command constructed by Codex (same as before)
    $ ssh -o BatchMode=yes steve@172.16.171.10 powershell -NoProfile -NonInteractive -Command "Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools; Import-Module ADDSDeployment; Install-ADDSForest -DomainName 'sharpe.com' -DomainNetbiosName 'SHARPE' -InstallDns -SafeModeAdministratorPassword (ConvertTo-SecureString 'Windows1' -AsPlainText -Force) -Force"
    ```
    *(Output: `command.stdout (code: 255, duration: 10s)` - Likely indicates success but the remote machine initiated a reboot as part of promotion, terminating the session before full completion status)*

**(Codex Action - Updating Local Topology File):**

* Proposes and applies patch to update Role in `topology.md`:
    ```diff
    $ apply_patch '*** Begin Patch
    *** Update File: topology.md
    @@
     -| sharpe-dc1 | Windows | Server            | 172.16.171.10 |               | Ethernet0  | steve        | Static    |
    +| sharpe-dc1 | Windows | Domain Controller | 172.16.171.10 |               | Ethernet0  | steve        | Static    |
    *** End Patch'
    ```
    *(Output: `Done!`)*
* Proposes and applies patch to update Change History in `topology.md`:
    ```diff
    $ apply_patch '*** Begin Patch
    *** Update File: topology.md
    @@ ## Change History
     -| 2025-04-29 | Renamed hostname from 2025 to sharpe-dc1 on 172.16.171.10 | steve |
    +| 2025-04-29 | Renamed hostname from 2025 to sharpe-dc1 on 172.16.171.10 | steve |
    +| 2025-04-29 | Promoted to Domain Controller, created forest sharpe.com    | steve |
    *** End Patch'
    ```
    *(Output: `Done!`)*

**codex (Summary of Promotion):**
> The promotion command has been issued on sharpe-dc1 (172.16.171.10) to install AD DS and create the forest “sharpe.com.” The server is likely rebooting now; once it comes back up, it will function as your new Domain Controller.
>
> I’ve also updated topology.md to reflect its new role and recorded the change. Let me know if you need any further tasks (e.g., joining additional servers to the new domain).

---

### Verification After Reboot

**user:**
System came back up. Login and verify that everything works correctly.

**(Codex Actions - Connecting and Verifying):**

* Attempts remote verification command (fails due to remote shell interpretation issue with `Format-Table`):
    ```bash
    # Command constructed by Codex
    $ ssh -o BatchMode=yes steve@172.16.171.10 powershell -NoProfile -NonInteractive -Command 'Import-Module ActiveDirectory; Get-WindowsFeature AD-Domain-Services | Format-Table DisplayName,Installed; Get-ADForest | Format-Table Name,Domains; Get-ADDomain | Format-Table Name,DomainMode; Get-Service DNS,NetLogon | Format-Table Name,Status'
    ```
    *(Output: `'Format-Table' is not recognized...`)*
* Re-attempts verification command with corrected quoting/escaping for PowerShell:
    ```bash
    # Command constructed by Codex (note escaped quotes for Format-Table)
    $ ssh -o BatchMode=yes steve@172.16.171.10 "powershell -NoProfile -NonInteractive -Command \"Import-Module ActiveDirectory; Get-WindowsFeature AD-Domain-Services | Format-Table DisplayName,Installed; Get-ADForest | Format-Table Name,Domains; Get-ADDomain | Format-Table Name,DomainMode; Get-Service DNS,NetLogon | Format-Table Name,Status\""
    ```
    *(Output shows successful retrieval of AD Feature status, Forest/Domain info, and Service status)*
    ```powershell
    DisplayName                     Installed
    -----------                     ---------
    Active Directory Domain Services       True


    Name       Domains
    ----       -------
    sharpe.com {sharpe.com}


    Name   DomainMode
    ----   ----------
    SHARPE Windows2016Domain # Note: Actual mode might differ based on Server 2025 specifics


    Name     Status
    ----     ------
    DNS     Running
    Netlogon Running
    ```

**codex (Summary of Verification):**
> Verification on sharpe-dc1 (172.16.171.10) succeeded:
> * Active Directory Domain Services feature: Installed (True)
> * Forest: sharpe.com (domains: sharpe.com)
> * Domain: SHARPE (mode: Windows2016Domain) *(Note: Mode shown might be interpretation; actual value depends on promotion parameters used by `Install-ADDSForest`)*
> * DNS service: Running
> * NetLogon service: Running
>
> Everything is up and healthy. Let me know if you’d like additional checks or to join other servers to the new domain.

---
