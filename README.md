# Lab 04: JML Lifecycle Automation (Joiner-Mover-Leaver)

## 🎯 Objective
Automate identity lifecycle transitions (Joiner, Mover, Leaver) using Okta group rules and attribute expressions to eliminate manual provisioning delays, prevent privilege creep, and secure offboarding.

---

## 🏗️ Architecture & Scenario
In a mid-to-large enterprise, managing employee status changes manually creates human error, orphaned accounts, and security vulnerabilities. This lab models a full employment lifecycle:
1. **The Joiner:** A new hire enters the HR system/directory, automatically receiving baseline permissions based on their department.
2. **The Mover:** An employee changes departments (e.g., from Sales to Engineering), requiring an automated swap of group memberships and application entitlements.
3. **The Leaver:** An employee departs, triggering instant account deactivation, session revocation, and license reclamation.

---

## ⚙️ Configuration Steps

### 1. Custom Profile Attributes
* Navigated to **Directory** > **Profile Editor** > **User (default)**.
* Added custom attributes to simulate HR metadata:
  * `department` (String)
  * `employeeStatus` (String: Active, Terminated)

### 2. The "Joiner" Phase (Automated Onboarding)
* Created a group named `US-Employees-Base`.
* Built an Okta Expression Language rule to catch new hires:
  ```text
  user.employeeStatus == "Active" AND user.department != ""

**###3. The "Mover" Phase (Role Shift)**
Created department-specific groups: Dept-Sales and Dept-Engineering.

Configured dynamic rules using attribute matching so that updating a user's department instantly shifts their group memberships:

Sales Rule: user.department == "Sales"

Engineering Rule: user.department == "Engineering"


**###4. The "Leaver" Phase (Offboarding)**
Configured sign-on and lifecycle policies to handle status changes to Terminated.

Tested immediate session invalidation and account suspension.

🧪 Validation & Testing
Test Case 1 (Joiner): Provisioned a test user (test.user@lab.local) with employeeStatus = Active and verified instant drop into the base group and app assignment.

Test Case 2 (Mover): Updated the test user's department from Sales to Engineering. Verified that the user automatically lost access to Sales-specific resources and gained Engineering entitlements without manual helpdesk intervention.

Test Case 3 (Leaver): Switched status to Terminated, confirming active tokens were revoked and the account was disabled.

(Optional: Insert an anonymized screenshot of your Okta Group Rule expression builder here)

[Insert Screenshot: Group Rule Expression Editor]

💡 Key Takeaways & Challenges
Syntax Nuances: Practiced writing clean Okta Expression Language (OEL) strings to handle logical AND operators cleanly without syntax errors.

Security Impact: Automated JML drastically shrinks the "window of exposure" during offboarding and stops privilege creep when employees switch roles internally.
