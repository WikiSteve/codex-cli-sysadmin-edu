# Active Directory Group Structure: Design Rationale (`sharpe.com`)

**Date:** 2025-05-02

**Overview:** This document details the purpose and rationale for each Security Group defined in `active-directory/data/Groups.csv` for the `sharpe.com` simulated Active Directory environment. The groups are designed to support Role-Based Access Control (RBAC), departmental organization, administrative delegation, and specific resource access within the defined OU structure. The structure includes nested groups to test complex membership management with Codex CLI.

---

## Role-Based Security Groups

*Location: `OU=RoleBased,OU=SecurityGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`*

### Group: `SG_Role_AllStaff`
* **Scope & Category:** Global Security Group
* **Description:** All Staff Employees (Non-Faculty).
* **Rationale:** Aggregates all non-teaching staff user accounts. Used for granting baseline permissions applicable to all staff (e.g., access to general staff resources, intranet sections) or as a base for broader GPO filtering. Departmental staff groups are nested within this group.

### Group: `SG_Role_AllFaculty`
* **Scope & Category:** Global Security Group
* **Description:** All Faculty Employees (Teaching Staff).
* **Rationale:** Aggregates all teaching staff user accounts. Used for granting baseline permissions specific to faculty (e.g., access to academic resources, faculty portals) or for GPO filtering. Faculty departmental groups are nested within this group.

### Group: `SG_Role_AllStudents`
* **Scope & Category:** Global Security Group
* **Description:** All Currently Enrolled Students.
* **Rationale:** Aggregates all active student accounts (Undergraduate, Graduate, Continuing Ed). Used for granting baseline student permissions (e.g., library database access, student portal) and applying student-wide policies. Specific student status groups (Undergrad, Graduate) are nested within.

### Group: `SG_Role_Students_Undergrad`
* **Scope & Category:** Global Security Group
* **Description:** Undergraduate Students.
* **Rationale:** Specifically identifies undergraduate students. Can be used for granting access to resources or applying policies unique to this cohort. Nested within `SG_Role_AllStudents`.

### Group: `SG_Role_Students_Graduate`
* **Scope & Category:** Global Security Group
* **Description:** Graduate Students.
* **Rationale:** Specifically identifies graduate students. Useful for permissions or policies distinct from undergraduates (e.g., access to specific research tools, different lab access). Nested within `SG_Role_AllStudents`.

### Group: `SG_Role_HelpDeskStaff`
* **Scope & Category:** Global Security Group
* **Description:** Role group for all Help Desk personnel.
* **Rationale:** Aggregates user accounts of individuals working at the Help Desk (Tier 1). This group itself might not receive permissions directly but contains the users who are part of the administrative group (`SG_Admin_Tier1_HelpDesk`) that *does* get delegated rights.

### Group: `SG_Role_ServerAdmins`
* **Scope & Category:** Global Security Group
* **Description:** Role group for all Server Admins.
* **Rationale:** Aggregates user accounts of Tier 2 server administrators. Similar to the Help Desk group, this contains the users, while a corresponding group in the `_ServiceManagement` OU (`SG_Admin_Tier2_ServerAdmins`) is used for actual delegation.

---

## Departmental Security Groups

*Location: `OU=Departmental,OU=SecurityGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`*

### Group: `SG_Dept_IT`
* **Scope & Category:** Global Security Group
* **Description:** IT Department Staff.
* **Rationale:** Contains users from the IT department. Used for granting access to IT-specific resources (shares, tools) and potentially for departmental GPO filtering. Nested within `SG_Role_AllStaff`.

### Group: `SG_Dept_HR`
* **Scope & Category:** Global Security Group
* **Description:** Human Resources Department Staff.
* **Rationale:** Contains HR users. Used for controlling access to HR-specific systems and file shares. Nested within `SG_Role_AllStaff`.

### Group: `SG_Dept_Finance`
* **Scope & Category:** Global Security Group
* **Description:** Finance Department Staff.
* **Rationale:** Contains Finance users. Grants access to financial systems, sensitive shares, etc. Nested within `SG_Role_AllStaff`.

### Group: `SG_Dept_Facilities`
* **Scope & Category:** Global Security Group
* **Description:** Facilities Department Staff.
* **Rationale:** Contains Facilities users. Used for accessing work order systems, building plans, etc. Nested within `SG_Role_AllStaff`.

### Group: `SG_Dept_Administration`
* **Scope & Category:** Global Security Group
* **Description:** Administration Department Staff.
* **Rationale:** Contains senior admin and support users. May grant access to high-level reports or administrative tools. Nested within `SG_Role_AllStaff`.

### Group: `SG_Dept_Admissions`
* **Scope & Category:** Global Security Group
* **Description:** Admissions Department Staff.
* **Rationale:** Contains Admissions users. Grants access to prospective student systems and related resources. Nested within `SG_Role_AllStaff`.

### Group: `SG_Dept_Faculty_Sciences`
* **Scope & Category:** Global Security Group
* **Description:** Faculty in School of Sciences.
* **Rationale:** Groups faculty specifically within this school for access to science-related resources (lab systems, databases, shares). Nested within `SG_Role_AllFaculty`.

### Group: `SG_Dept_Faculty_Arts`
* **Scope & Category:** Global Security Group
* **Description:** Faculty in School of Arts.
* **Rationale:** Groups Arts faculty for access to relevant resources (media labs, software licenses, galleries). Nested within `SG_Role_AllFaculty`.

### Group: `SG_Dept_Faculty_Technology`
* **Scope & Category:** Global Security Group
* **Description:** Faculty in School of Technology.
* **Rationale:** Groups Technology faculty for access to specialized labs, development environments, software. Nested within `SG_Role_AllFaculty`.

### Group: `SG_Dept_Faculty_Business`
* **Scope & Category:** Global Security Group
* **Description:** Faculty in School of Business.
* **Rationale:** Groups Business faculty for access to financial databases, simulation software, market data resources. Nested within `SG_Role_AllFaculty`.

### Group: `SG_Dept_Faculty_Health`
* **Scope & Category:** Global Security Group
* **Description:** Faculty in School of Health.
* **Rationale:** Groups Health Sciences faculty for access to clinical simulation tools, research data, compliance resources. Nested within `SG_Role_AllFaculty`.

---

## Administrative Delegation Security Groups

*Location: Varies under `OU=AdminGroups,OU=_ServiceManagement,DC=sharpe,DC=com`*

### Group: `SG_Admin_Tier1_HelpDesk`
* **Path:** `OU=Tier1_HelpDesk,OU=AdminGroups,OU=_ServiceManagement,DC=sharpe,DC=com`
* **Scope & Category:** Global Security Group
* **Description:** Tier 1 Help Desk Staff Group (for delegation membership).
* **Rationale:** This group contains the *user accounts* of the Tier 1 Help Desk staff (users who are also members of `SG_Role_HelpDeskStaff`). This Global group is then typically made a member of a DomainLocal group (like `SG_Admin_Tier1_DelegationTarget`) which actually receives the delegated permissions (e.g., password reset rights on the Student OUs). This follows AGDLP/AGUDLP principles.

### Group: `SG_Admin_Tier2_ServerAdmins`
* **Path:** `OU=Tier2_ServerAdmins,OU=AdminGroups,OU=_ServiceManagement,DC=sharpe,DC=com`
* **Scope & Category:** Global Security Group
* **Description:** Tier 2 Server Admins Group (for delegation membership).
* **Rationale:** Contains the *user accounts* of the Tier 2 Server Admins (users also in `SG_Role_ServerAdmins`). This group would be nested into DomainLocal groups that are granted administrative rights over specific Server OUs (e.g., OU=FileServers, OU=WebServers).

### Group: `SG_Admin_Tier3_DomainAdminsCustom`
* **Path:** `OU=Tier3_DomainAdmins,OU=AdminGroups,OU=_ServiceManagement,DC=sharpe,DC=com`
* **Scope & Category:** Global Security Group
* **Description:** Custom Group for Delegated Domain Admin Tasks (Avoid using Builtin Domain Admins directly).
* **Rationale:** Intended to hold specific Tier 3 admin accounts. Follows the best practice of minimizing direct membership in the powerful built-in `Domain Admins` group. Specific high-level permissions could be delegated to this custom group instead.

### Group: `SG_Admin_Tier1_DelegationTarget`
* **Path:** `OU=Tier1_HelpDesk,OU=AdminGroups,OU=_ServiceManagement,DC=sharpe,DC=com`
* **Scope & Category:** Domain Local Security Group
* **Description:** Group granted specific delegated rights (e.g., Password Reset on Student OU).
* **Rationale:** This DomainLocal group is the one that actually receives the permissions via the Delegate Control wizard or ACL editing on target OUs (like `OU=Students`). The corresponding Global group (`SG_Admin_Tier1_HelpDesk`) containing the user accounts is then made a member of *this* group to grant the rights. Nested within `SG_Admin_Tier1_HelpDesk`. *(Self-correction: Nesting here might be reversed in standard AGDLP; typically the Global group is put INSIDE the Domain Local group. The CSV definition `NestedMemberOf` implies the DL group is nested in the Global group, which is less standard but tests if Codex handles it. Standard AGDLP would have SG_Admin_Tier1_HelpDesk listed in the NestedMemberOf column for this DL group - let's assume the CSV implies the non-standard way for testing)*. **Note:** The CSV as written implies non-standard nesting (DL inside Global). For standard AGDLP, the `NestedMemberOf` for this group would be empty, and the `SG_Admin_Tier1_HelpDesk` group's row would have `SG_Admin_Tier1_DelegationTarget` in its `NestedMemberOf` column. We'll stick to the CSV as written for the challenge.

---

## Resource Access Security Groups

*Location: `OU=ResourceAccess,OU=SecurityGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`*

### Group: `SG_Resource_FS_Finance_RW`
* **Scope & Category:** Domain Local Security Group
* **Description:** Grants Read/Write access to Finance File Share.
* **Rationale:** This group would be assigned NTFS Read/Write permissions directly on the Finance department's shared folder(s). The global group `SG_Dept_Finance` (and potentially specific finance roles) would then be made members of this group to grant access.

### Group: `SG_Resource_FS_StudentData_RO`
* **Scope & Category:** Domain Local Security Group
* **Description:** Grants Read Only access to Student Data Share.
* **Rationale:** Assigned NTFS Read-Only permissions on a share containing student data accessible to specific staff/faculty roles. Appropriate Global groups (e.g., `SG_Dept_Admissions`, `SG_Dept_Faculty_...`) would be nested within this group.

### Group: `SG_Resource_Printer_LibraryColor`
* **Scope & Category:** Global Security Group
* **Description:** Users allowed to print to Library Color printer.
* **Rationale:** Used in the security settings of the specific shared printer object to control access. Users or other Global groups needing access would be added as members. (Could also be DomainLocal if preferred for resource permissions).

---

## Course Access Security Groups

*Location: `OU=CourseAccess,OU=SecurityGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`*

### Group: `SG_Course_CS101_Fall2025`
* **Scope & Category:** Global Security Group
* **Description:** Students enrolled in CS101 Fall 2025.
* **Rationale:** Grants access to course-specific materials, lab software licenses, or shared folders for CS101 in the Fall 2025 semester. Student user accounts are added to this group upon enrollment. Nested within `SG_Role_AllStudents`.

### Group: `SG_Course_BIO205_Fall2025`
* **Scope & Category:** Global Security Group
* **Description:** Students enrolled in BIO205 Fall 2025.
* **Rationale:** Grants access to course-specific resources for BIO205 in Fall 2025. Nested within `SG_Role_AllStudents`.

---

## Nesting Test Security Groups

*Location: `OU=RoleBased,OU=SecurityGroups,OU=Groups,OU=_CampusAccounts,DC=sharpe,DC=com`*

### Group: `SG_NestingTest_Level1`
* **Scope & Category:** Global Security Group
* **Description:** Top level group for nesting test.
* **Rationale:** Purely exists to test Codex CLI's ability to handle multi-level group nesting dependencies during creation and membership population.

### Group: `SG_NestingTest_Level2`
* **Scope & Category:** Global Security Group
* **Description:** Mid level group for nesting test.
* **Rationale:** Child of Level1, Parent of Level3. Tests intermediate nesting. Nested within `SG_NestingTest_Level1`.

### Group: `SG_NestingTest_Level3`
* **Scope & Category:** Global Security Group
* **Description:** Bottom level group for nesting test.
* **Rationale:** Child of Level2. Tests the deepest level of nesting in this specific test case. Nested within `SG_NestingTest_Level2`.

---
