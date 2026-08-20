# Lab 02a: On-Premises JML Lifecycle Automation (Active Directory)

## 🎯 Objective
Design and implement an automated Joiner-Mover-Leaver (JML) lifecycle workflow utilizing Windows Server Active Directory, Organizational Units (OUs), security groups, and automated identity management structures to mitigate insider threats and eliminate manual provisioning overhead.

---

## 🏗️ Architecture & Scenario
In traditional enterprise environments, managing identity lifecycles manually via Active Directory introduces severe risks, including orphaned accounts, privilege creep, and human error during role changes. This lab models a robust on-premises JML framework:
1. **The Joiner:** Automated onboarding where new employee accounts are provisioned into designated Departmental OUs with baseline security group memberships.
2. **The Mover:** Internal role changes where an employee's account is moved between OUs (e.g., from Sales to Engineering), automatically stripping legacy access and binding new permissions.
3. **The Leaver:** Secure offboarding that triggers immediate account disablement, relocation to an isolated "Terminated" OU, and stripping of all active group memberships.

---

## ⚙️ Configuration Steps

### 1. OU Structure & Directory Design
* Configured a hierarchical Active Directory Organizational Unit (OU) structure:
  * `Corp/Users/Active/`
    * `Corp/Users/Active/Sales/`
    * `Corp/Users/Active/Engineering/`
  * `Corp/Users/Terminated/`

### 2. The "Joiner" Phase (Onboarding)
* Created global security groups for base access: `GG-All-Employees`.
* Established standard templates for user object creation, ensuring every new hire automatically inherits baseline organizational group memberships.

### 3. The "Mover" Phase (Internal Transfers)
* Created role-specific groups: `GG-Sales-Dept` and `GG-Engineering-Dept`.
* Configured group scope and membership rules so that when an employee transitions departments, relocating their user object to the new departmental OU (or updating their group linkage) cleanly revokes legacy resource access while appending the new role's permissions.

### 4. The "Leaver" Phase (Offboarding & Deactivation)
* Configured the offboarding SOP:
  * Right-click the departing user object and select **Disable Account**.
  * Move the user account object out of the active departmental OU and into the isolated `Corp/Users/Terminated/` OU (which is isolated from Group Policy execution and resource access).
  * Purge all primary security group memberships (except Domain Users where required by schema).

---

## 🧪 Validation & Testing
* **Scenario A (Joiner Test):** Provisioned a test object within `Sales/`, verifying proper inheritance of baseline groups.
* **Scenario B (Mover Test):** Migrated the test user object from the `Sales/` OU to the `Engineering/` OU, confirming old group permissions were cleared and new engineering resources became accessible.
* **Scenario C (Leaver Test):** Disabled and relocated a test account to the `Terminated/` OU, verifying that active Kerberos tickets and session tokens were invalidated and access was instantly cut off.

---

## 💡 Key Takeaways & Challenges
* **Directory Structure Hygiene:** Maintained strict OU design to ensure Group Policy Objects (GPOs) and access permissions map seamlessly to changing job functions.
* **Security Impact:** Structured offboarding via isolated OUs and immediate account disablement drastically closes the exposure window during employee departures.
