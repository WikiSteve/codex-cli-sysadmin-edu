# Active Directory OU Structure: Design Rationale (`sharpe.com`)

**Date:** 2025-05-02

**Overview:** This document details the purpose and rationale for each Organizational Unit (OU) defined in `active-directory/data/OUs.csv` for the `sharpe.com` simulated Active Directory environment. The design follows a hybrid model, balancing resource-based organization with functional/role-based needs typical of an educational institution, while providing sufficient complexity for testing Codex CLI capabilities. The primary drivers for OU creation are distinct Group Policy Object (GPO) requirements and/or the need for delegated administration.

---

## Top-Level OUs

### OU: `_CampusAccounts`
* **Path:** `DC=sharpe,DC=com`
* **Description:** Top-level container for User and Group objects.
* **Rationale:** Separates identity objects (users, groups) from resource objects (computers, printers) at the highest level. Facilitates applying broad GPOs related to accounts or delegating high-level account management tasks if needed. The underscore prefix distinguishes it visually from built-in containers.

### OU: `_CampusResources`
* **Path:** `DC=sharpe,DC=com`
* **Description:** Top-level container for Computer, Server, and other resource objects.
* **Rationale:** Consolidates all non-identity, manageable resources. Allows for distinct high-level policies or delegation related to infrastructure assets separate from user accounts. The underscore prefix distinguishes it visually.

### OU: `_ServiceManagement`
* **Path:** `DC=sharpe,DC=com`
* **Description:** Top-level container for administrative accounts and groups used for delegation.
* **Rationale:** Isolates high-privilege groups and service accounts used for managing the AD environment itself, enhancing security and simplifying auditing of administrative access. The underscore prefix distinguishes it visually.

---

## `_CampusAccounts` Sub-OUs

### OU: `Users`
* **Path:** `OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Parent OU for all user accounts.
* **Rationale:** Provides a primary container under `_CampusAccounts` specifically for user objects, allowing policies or delegation specific to users without affecting groups or service accounts within the same top-level OU.

### OU: `Staff`
* **Path:** `OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Container for all non-teaching staff user accounts.
* **Rationale:** Separates staff users from faculty and students, enabling application of staff-specific GPOs (e.g., software deployment, security settings) and delegation of administration for staff accounts.

### OU: `IT` (under Staff)
* **Path:** `OU=Staff,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Information Technology department staff.
* **Rationale:** Allows targeted GPOs for IT staff (e.g., access to specific tools, different security policies) and potential delegation of management tasks within the IT department.

### OU: `HR` (under Staff)
* **Path:** `OU=Staff,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Human Resources department staff.
* **Rationale:** Enables specific GPOs for HR staff (e.g., access to HR systems) and potential delegation of user attribute management relevant to HR functions.

### OU: `Finance` (under Staff)
* **Path:** `OU=Staff,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Finance department staff.
* **Rationale:** Facilitates targeted GPOs for finance personnel (e.g., access to financial software, potentially stricter security settings) and relevant administrative delegation.

### OU: `Facilities` (under Staff)
* **Path:** `OU=Staff,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Facilities and Maintenance staff.
* **Rationale:** Allows for specific policies or application deployments relevant to facilities staff and potential delegation.

### OU: `Administration` (under Staff)
* **Path:** `OU=Staff,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Administrative leadership and support staff.
* **Rationale:** Groups senior administrators and their support staff for potentially distinct GPO requirements or delegation structures.

### OU: `Admissions` (under Staff)
* **Path:** `OU=Staff,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Admissions department staff.
* **Rationale:** Enables specific GPOs related to admissions systems or data access and potential delegation for managing prospective student data interfaces.

### OU: `Faculty`
* **Path:** `OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Container for all teaching staff (faculty) user accounts.
* **Rationale:** Separates faculty from staff and students for distinct GPOs (e.g., academic software, research resource access) and delegation relevant to academic departments.

### OU: `Sciences` (under Faculty)
* **Path:** `OU=Faculty,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Faculty within the School of Sciences.
* **Rationale:** Allows GPOs specific to Science faculty (e.g., lab software, database access) and delegation of administrative tasks within the school.

### OU: `Arts` (under Faculty)
* **Path:** `OU=Faculty,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Faculty within the School of Arts.
* **Rationale:** Enables targeted GPOs (e.g., design software, media resources) and delegation specific to the Arts school.

### OU: `Technology` (under Faculty)
* **Path:** `OU=Faculty,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Faculty within the School of Technology.
* **Rationale:** Facilitates GPOs for technology-focused faculty (e.g., development tools, specialized lab access) and school-specific delegation.

### OU: `Business` (under Faculty)
* **Path:** `OU=Faculty,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Faculty within the School of Business.
* **Rationale:** Allows GPOs tailored to Business faculty (e.g., statistical software, financial databases) and relevant delegation.

### OU: `Health` (under Faculty)
* **Path:** `OU=Faculty,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Faculty within the School of Health Sciences.
* **Rationale:** Enables specific GPOs for Health Sciences faculty (e.g., compliance training reminders, access to specific clinical systems or data) and school-level delegation.

### OU: `Students`
* **Path:** `OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Container for all student user accounts.
* **Rationale:** Separates student accounts from employee accounts for fundamentally different GPO requirements (e.g., stricter internet filtering, lab software access, different password policies) and delegation (e.g., Help Desk password resets).

### OU: `Undergraduate` (under Students)
* **Path:** `OU=Students,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Undergraduate student accounts.
* **Rationale:** Allows for policies potentially specific to undergraduate students (e.g., access to certain labs or resources) compared to graduate students.

### OU: `Graduate` (under Students)
* **Path:** `OU=Students,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Graduate student accounts.
* **Rationale:** Enables potentially different GPOs for graduate students (e.g., access to research databases, different software licenses) and distinct administrative processes.

### OU: `ContinuingEducation` (under Students)
* **Path:** `OU=Students,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Continuing Education student accounts.
* **Rationale:** Caters to potentially different policy needs or shorter account lifecycles for non-degree or professional development students.

### OU: `Alumni` (under Students)
* **Path:** `OU=Students,OU=Users,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Alumni accounts (potentially disabled or with limited access).
* **Rationale:** Provides a place to move student accounts upon graduation, allowing application of specific alumni policies (e.g., disabled account state, different email forwarding rules) without deleting them immediately.

### OU: `ServiceAccounts`
* **Path:** `OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Container for non-interactive service accounts.
* **Rationale:** Isolates service accounts from regular user accounts for security purposes. Allows applying strict GPOs (e.g., deny interactive logon) and simplifies auditing of these powerful accounts. *Note: Could also reside under _ServiceManagement, placement here groups it with user-like objects.*

### OU: `Groups`
* **Path:** `OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Parent OU for all group objects.
* **Rationale:** Centralizes group management and separates group objects from user objects within the `_CampusAccounts` hierarchy.

### OU: `SecurityGroups` (under Groups)
* **Path:** `OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Container for groups used for permissions.
* **Rationale:** Organizes security groups separately from distribution groups, simplifying administration and locating groups used for controlling access.

### OU: `RoleBased` (under SecurityGroups)
* **Path:** `OU=SecurityGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Security groups based on campus roles (e.g., AllStaff, AllStudents).
* **Rationale:** Groups together role-based security principals for clarity.

### OU: `Departmental` (under SecurityGroups)
* **Path:** `OU=SecurityGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Security groups based on departments (e.g., SG_Dept_IT, SG_Dept_HR).
* **Rationale:** Organizes departmental security groups for easier management.

### OU: `CourseAccess` (under SecurityGroups)
* **Path:** `OU=SecurityGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Security groups granting access to course resources (e.g., specific file shares, software licenses).
* **Rationale:** Centralizes groups related to academic course access control.

### OU: `ResourceAccess` (under SecurityGroups)
* **Path:** `OU=SecurityGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Security groups granting access to specific non-course resources (files, printers, applications).
* **Rationale:** Organizes groups used for general resource permissions.

### OU: `DistributionGroups` (under Groups)
* **Path:** `OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Container for groups used primarily for email distribution.
* **Rationale:** Separates distribution lists from security groups, clarifying intent and simplifying email list management.

### OU: `Announcements` (under DistributionGroups)
* **Path:** `OU=DistributionGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Campus-wide or role-wide announcement email lists.
* **Rationale:** Organizes broad communication lists.

### OU: `DepartmentalLists` (under DistributionGroups)
* **Path:** `OU=DistributionGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Email lists for specific departments.
* **Rationale:** Organizes department-specific communication lists.

### OU: `StudentCohorts` (under DistributionGroups)
* **Path:** `OU=DistributionGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`
* **Description:** Email lists for specific student cohorts or programs.
* **Rationale:** Organizes communication lists targeted at specific student groups.

---

## `_CampusResources` Sub-OUs

### OU: `Computers`
* **Path:** `OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Parent OU for computer accounts.
* **Rationale:** Central container for all managed computer objects (workstations, laptops, kiosks), allowing for base computer policies.

### OU: `Workstations_Staff` (under Computers)
* **Path:** `OU=Computers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Workstations assigned to staff.
* **Rationale:** Enables GPOs specific to staff machines (e.g., standard office software, security configurations) separate from faculty or lab machines.

### OU: `Workstations_Faculty` (under Computers)
* **Path:** `OU=Computers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Workstations assigned to faculty.
* **Rationale:** Allows GPOs tailored to faculty needs (e.g., academic software, possibly different security profiles) separate from staff or lab machines.

### OU: `Labs_General` (under Computers)
* **Path:** `OU=Computers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** General access computer labs available to most students.
* **Rationale:** Applies common lab policies (e.g., mandatory profiles, security lockdowns, public printing setup).

### OU: `Labs_Technology` (under Computers)
* **Path:** `OU=Computers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Specialized Technology computer labs.
* **Rationale:** Deploys specific software (e.g., IDEs, simulation tools) and potentially different security settings required for technology courses, separate from general labs.

### OU: `Labs_HealthSciences` (under Computers)
* **Path:** `OU=Computers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Specialized Health Sciences computer labs.
* **Rationale:** Deploys specific health sciences software, potentially applies stricter data handling policies, or configures access to specialized peripherals.

### OU: `Kiosks` (under Computers)
* **Path:** `OU=Computers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Public access kiosk machines (e.g., library lookup, registration).
* **Rationale:** Applies highly restrictive GPOs for security, locking down the interface to specific applications and preventing general Browse or system changes.

### OU: `Servers`
* **Path:** `OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Parent OU for server accounts (excluding Domain Controllers).
* **Rationale:** Separates servers from workstations for distinct server-specific GPOs (e.g., security hardening, backup agent deployment, performance monitoring settings) and delegation of server administration. Domain Controllers remain in their default OU for security.

### OU: `Infrastructure` (under Servers)
* **Path:** `OU=Servers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Core infrastructure servers (DHCP, DNS, etc.).
* **Rationale:** Groups critical network infrastructure servers for applying specific availability monitoring, security policies, and administration delegation separate from application or file servers.

### OU: `FileServers` (under Servers)
* **Path:** `OU=Servers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** File server computer objects.
* **Rationale:** Allows GPOs specific to file servers (e.g., FSRM quotas/screens, DFS configuration, specific backup policies).

### OU: `WebServers` (under Servers)
* **Path:** `OU=Servers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Web server computer objects.
* **Rationale:** Enables application of web server specific GPOs (e.g., IIS hardening, certificate management settings, logging configurations).

### OU: `DatabaseServers` (under Servers)
* **Path:** `OU=Servers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Database server computer objects.
* **Rationale:** Facilitates GPOs tailored to database servers (e.g., specific service account permissions, firewall rules for DB ports, performance tuning settings).

### OU: `ApplicationServers` (under Servers)
* **Path:** `OU=Servers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Application server computer objects (e.g., running specific campus applications).
* **Rationale:** Groups servers running specific business or academic applications for targeted GPOs or delegated application administration.

### OU: `Printers`
* **Path:** `OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Container for printer objects published in Active Directory.
* **Rationale:** Organizes published printer objects, potentially simplifying discovery or management if using AD-integrated printing features.

### OU: `LibraryPrinters` (under Printers)
* **Path:** `OU=Printers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Printers located in the library.
* **Rationale:** Primarily for organizational clarity if managing printers via AD.

### OU: `StaffPrinters` (under Printers)
* **Path:** `OU=Printers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Printers located in staff areas.
* **Rationale:** Organizational clarity.

### OU: `LabPrinters` (under Printers)
* **Path:** `OU=Printers,OU=_CampusResources,DC=sharpe,DC=com`
* **Description:** Printers located in computer labs.
* **Rationale:** Organizational clarity.

---

## `_ServiceManagement` Sub-OUs

### OU: `AdminGroups`
* **Path:** `OU=_ServiceManagement,DC=sharpe,DC=com`
* **Description:** Container for groups used for AD delegation and administration.
* **Rationale:** Centralizes groups that are granted specific administrative permissions over parts of the AD structure, separating them from general security or distribution groups.

### OU: `Tier1_HelpDesk` (under AdminGroups)
* **Path:** `OU=AdminGroups,OU=_ServiceManagement,DC=sharpe,DC=com`
* **Description:** Groups for Tier 1 Help Desk delegation (e.g., password resets, basic user attribute changes).
* **Rationale:** Isolates groups used for delegating limited, high-volume administrative tasks, typically over student or general user OUs.

### OU: `Tier2_ServerAdmins` (under AdminGroups)
* **Path:** `OU=AdminGroups,OU=_ServiceManagement,DC=sharpe,DC=com`
* **Description:** Groups for Tier 2 Server Admins delegation (e.g., managing specific server OUs).
* **Rationale:** Contains groups granted administrative rights over server infrastructure, separate from user account management or top-level domain administration.

### OU: `Tier3_DomainAdmins` (under AdminGroups)
* **Path:** `OU=AdminGroups,OU=_ServiceManagement,DC=sharpe,DC=com`
* **Description:** Groups related to Tier 3 Domain Admins delegation (potentially containing custom groups with high privilege, distinct from the built-in Domain Admins group).
* **Rationale:** Organizes groups associated with the highest level of domain administration tasks, ideally following least privilege principles even here.

---
