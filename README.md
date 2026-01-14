# 🔐 Automated Identity Lifecycle Management System (JML)
### Microsoft Entra ID — Dynamic Group IAM Model  
**Author:** Felipe Restrepo  
**Category:** Identity & Access Management / Cloud Security  
**Technologies:** Microsoft Entra ID, Dynamic Groups, Enterprise Applications, Conditional Access, PIM, Audit Logs

---

## 📘 Project Overview

This project demonstrates the design and implementation of an **automated Joiner–Mover–Leaver (JML)** identity lifecycle system using **Microsoft Entra ID**.

Instead of manual provisioning, access is driven by **user attributes (Department)** using **Dynamic Groups**, and application access is controlled through **group-based assignments**. The project includes **audit evidence** for compliance and security verification.

---

## 🎯 Security Objectives

- Automate onboarding and access provisioning (Joiner)  
- Automate access updates during role changes (Mover)  
- Secure offboarding via account disablement + token/session revocation (Leaver)  
- Enforce least privilege using group-based access  
- Capture audit-ready evidence through Entra logs  

---

## 🧪 Lab Environment

- Microsoft Entra ID tenant  
- Microsoft Entra ID P2  
- Global Administrator (Break-glass)  
- Dynamic security groups  
- Enterprise applications  
- Conditional Access policies  
- Privileged Identity Management (PIM)  

---

# ✅ Implementation (Step-by-Step)

---

## Part 1 — Create Test Users

Create baseline users to simulate departments and drive automation.

### Steps
1. Go to: **Microsoft Entra ID → Users → All users → + New user**
2. Create **3 users** and set the Department attribute:
   - `hr.user` → Department: `HR`
   - `it.user` → Department: `IT`
   - `finance.user` → Department: `Finance`

📸 **Screenshots**

<img width="828" height="604" alt="Screenshot 2026-01-14 083227" src="https://github.com/user-attachments/assets/a609382a-37a1-4669-b0e8-cb36f493fce6" />

<img width="849" height="597" alt="Screenshot 2026-01-14 083243" src="https://github.com/user-attachments/assets/e7681491-5bc7-47cf-ae72-33df35a087c6" />
---

## Part 2 — Create Dynamic Groups (Automation Engine)

Dynamic groups automatically place users into the correct department group based on `Department`.

### Create these groups

#### ✅ IT Dynamic Group
- Name: `IAM-IT-Users-Dynamic`
- Type: Security
- Membership: Dynamic user
- Rule:
```text
(user.department -eq "IT")
✅ HR Dynamic Group
Name: IAM-HR-Users-Dynamic

Rule:

text
Copy code
(user.department -eq "HR")
✅ Finance Dynamic Group
Name: IAM-Finance-Users-Dynamic

Rule:

Copy code
(user.department -eq "Finance")
```text

📸 Screenshots
<img width="732" height="491" alt="Screenshot 2026-01-14 084649" src="https://github.com/user-attachments/assets/82897a41-e4e6-4aef-8c1a-c0a2419ffa1b" />

<img width="1357" height="600" alt="Screenshot 2026-01-14 084716" src="https://github.com/user-attachments/assets/624a4e31-fed7-4d37-9d08-9e2e4ff67f0b" />

<img width="1360" height="608" alt="Screenshot 2026-01-14 085018" src="https://github.com/user-attachments/assets/8893bda5-b88a-4cf1-b838-d49e08c5c8c4" />

<img width="1359" height="603" alt="Screenshot 2026-01-14 085608" src="https://github.com/user-attachments/assets/a2670023-367d-4a9a-9c61-a3277fddcb90" />

<img width="1357" height="600" alt="Screenshot 2026-01-14 085236" src="https://github.com/user-attachments/assets/9c60561c-bec6-45b0-8a17-d024576a6529" />

<img width="1360" height="607" alt="Screenshot 2026-01-14 085210" src="https://github.com/user-attachments/assets/377a4efd-80e5-4cb7-99a3-6f819fca471b" />

Part 3 — Create Enterprise Apps + Group-Based Access
Enterprise apps are configured so access follows groups, not people.

Create apps (example names)
Go to: Enterprise applications → + New application

Select: Create your own application

Create:

IAM-IT-App

IAM-HR-App

Lock down apps
For each app:

Go to Properties

Set: Assignment required? = Yes

Go to Users and groups

Assign the dynamic group:

IAM-IT-App → IAM-IT-Users-Dynamic

IAM-HR-App → IAM-HR-Users-Dynamic

📸 Screenshots

screenshots/03-apps/app-overview.png

screenshots/03-apps/assignment-required-yes.png

screenshots/03-apps/app-group-assignment.png

🔄 Identity Lifecycle Testing (JML)
✅ Joiner Test — Automated Onboarding
Steps
Create a new user: newhire.user

Set: Department = HR

Wait 1–3 minutes

Expected results
User is automatically added to IAM-HR-Users-Dynamic

HR app access is granted through group assignment

📸 Screenshots

screenshots/04-joiner/user-created.png

screenshots/04-joiner/hr-group-membership.png

screenshots/04-joiner/hr-app-access.png

🏅 Mover Test — Automated Role Change
Steps
Open newhire.user

Edit properties → Change:

text
Copy code
Department: HR → IT
Save, then wait 1–3 minutes

Expected results
Removed from HR dynamic group

Added to IT dynamic group

HR app access removed

IT app access granted

📸 Screenshots

screenshots/05-mover/department-change.png

screenshots/05-mover/hr-removed.png

screenshots/05-mover/it-added.png

screenshots/05-mover/app-access-changed.png

🛡 Leaver Test — Secure Offboarding
NOTE: In the newer Entra UI, “Block sign-in” is typically done by setting Account enabled = No.

Steps
Open newhire.user

Click Edit properties

Find Account enabled

Set Account enabled = No

Save

Click Revoke sessions

Expected results
Sign-in blocked

Tokens invalidated

Access removed

📸 Screenshots

screenshots/06-leaver/account-enabled-no.png

screenshots/06-leaver/revoke-sessions.png

screenshots/06-leaver/signin-blocked.png

📊 Audit & Compliance Evidence
Directory Audit Logs
Go to: Monitoring → Audit logs

Filter by:

Target = newhire.user

Category = GroupManagement / UserManagement

Capture evidence for:

Add member to group

Remove member from group

Update user (department change)

Account disablement

📸 Screenshots

screenshots/07-audit/audit-add-member.png

screenshots/07-audit/audit-remove-member.png

screenshots/07-audit/audit-update-user.png

Sign-in Logs
Go to: Monitoring → Sign-in logs

Filter by user:

newhire.user

Capture:

Successful sign-in (before offboarding, if tested)

Failed sign-in (after account disabled)

📸 Screenshots

screenshots/07-audit/signin-logs-blocked.png

🛡 Security Controls Applied
Attribute-driven access automation (Dynamic Groups)

Group-based enterprise application access

Conditional Access enforcement (MFA / policies)

Privileged Identity Management (PIM)

Session revocation to prevent token reuse

Centralized logging for auditing and investigations

🧠 Key Skills Demonstrated
Identity lifecycle management (Joiner–Mover–Leaver)

Microsoft Entra ID administration

Dynamic group rule design

Group-based access control

Enterprise application access governance

IAM troubleshooting and validation

Audit log review and evidence collection

📌 Lessons Learned
Dynamic groups provide scalable IAM automation

Attribute quality is critical for correct access

Offboarding must include session/token revocation

Audit logs validate security controls and changes

