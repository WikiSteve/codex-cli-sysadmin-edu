```markdown
# Topology Notes

| Hostname | OS | Role | Primary IP | Primary VLAN | Interfaces | SSH Username | IP Config |
| -------- | -- | ---- | ---------- | ------------ | ---------- | ------------ | --------- |
| sharpe-dc1 | Windows | Domain Controller | 172.16.171.10 |              | Ethernet0  | steve        | Static   |
<!-- Add one row per machine, filling in only real data -->

---

## Field Descriptions

- **Hostname**: Machine name.  
- **OS**: Windows or Linux.  
- **Role**: Function (e.g., Domain Controller, File Server).  
- **Primary IP**: Management/SSH address.  
- **Primary VLAN**: VLAN ID for the management interface.  
- **Interfaces**: NIC names; note which is primary.  
- **SSH Username**: Account for SSH.  
- **IP Config**: “Static” or “DHCP”.

---

## Change History

| Date       | Change                           | Author |
| ---------- | -------------------------------- | ------ |
| YYYY-MM-DD | Initial creation                 | Your Name |
| 2025-04-28 | Configured static IP 172.16.171.10 on Ethernet0 (Windows) | steve |
| 2025-04-29 | Renamed hostname from 2025 to sharpe-dc1 on 172.16.171.10 | steve |
| 2025-04-29 | Promoted to Domain Controller, created forest sharpe.com | steve |

*Fill in actual dates and changes as you update.*  
```
