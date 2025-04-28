# Codex Project Instructions

## Topology Management

- Maintain an up-to-date list of servers, IP addresses, login usernames, operating systems, and assigned roles.  
- Use the project root `codex.md` to track project-wide topology information.  
- Use `codex.md` files in subdirectories for sub-project or module-specific overrides.

## Topology Update Policy

- When discovering new details about a host (hostname, IP, role, credentials), update the appropriate `codex.md` immediately.  
- Record the following fields:  
  - **Hostname** (current)  
  - **Primary IP address** (for SSH and management)  
  - **SSH username**  
  - **Operating system** (Windows/Linux)  
  - **Assigned role** (for example, Domain Controller, File Server, Web Server)  
  - **IP configuration type** (Static or DHCP)  
  - **Primary VLAN** if applicable

## Command Behavior Expectations

- When instructed to **“SSH to _[hostname/role]_”**, resolve it using the latest information in `codex.md`.  
- Assume `BatchMode=yes` (key-based authentication) unless explicitly told otherwise.  
- Prefer the **Primary IP address** for management tasks unless otherwise instructed.  
- If **DHCP** is in use, verify the current IP before attempting connection.

## Automatic Information Gathering

- If any critical information is missing (OS type, IP address, interface details), perform basic discovery automatically:  
  - **Connect via SSH** (using available keys or credentials).  
  - **Determine Operating System**:  
    - Windows: `systeminfo` or `ver`  
    - Linux: `uname -a` or `lsb_release -a`  
  - **Gather network information**:  
    - Windows: `Get-NetIPAddress` (PowerShell)  
    - Linux: `ip addr` or `ifconfig`  
- If results are incomplete or unclear, **prompt the user** with the gathered output and ask for clarification before making any changes.

## Data Consistency Rules

- Keep all topology entries clear, minimal, and current.  
- Confirm whether an entry already exists before overwriting or duplicating.  
- If discovery reveals discrepancies with existing records, alert the user before updating `codex.md`.  
- Document changes promptly in the appropriate `codex.md` file.

---

*End of Project Instructions*
