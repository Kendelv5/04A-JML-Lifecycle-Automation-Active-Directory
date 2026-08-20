# 04-JML-Lifecycle-Automation

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
