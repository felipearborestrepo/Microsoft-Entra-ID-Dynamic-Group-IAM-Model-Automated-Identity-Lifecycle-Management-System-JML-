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

<img width="1360" height="611" alt="Screenshot 2026-01-14 091311" src="https://github.com/user-attachments/assets/d50a7ecc-4796-4257-9cf3-c51bd105e844" />

---

## Part 2 — Create Dynamic Groups (Automation Engine)

Dynamic groups automatically place users into the correct department group based on `Department`.

### Create these groups

### ✅ IT Department — Dynamic Group

Group name: IAM-IT-Users-Dynamic
Type: Security
Membership: Dynamic user
Purpose: Automatically assigns IT users to the IT access model

Dynamic rule:
(user.department -eq "IT")

### ✅ HR Department — Dynamic Group

Group name: IAM-HR-Users-Dynamic
Type: Security
Membership: Dynamic user
Purpose: Automatically assigns HR users to the HR access model

Dynamic rule:
(user.department -eq "HR")

### ✅ Finance Department — Dynamic Group

Group name: IAM-Finance-Users-Dynamic
Type: Security
Membership: Dynamic user
Purpose: Automatically assigns Finance users to the Finance access model

Dynamic rule:
(user.department -eq "Finance")

📸 Screenshots
<img width="732" height="491" alt="Screenshot 2026-01-14 084649" src="https://github.com/user-attachments/assets/82897a41-e4e6-4aef-8c1a-c0a2419ffa1b" />

<img width="1357" height="600" alt="Screenshot 2026-01-14 084716" src="https://github.com/user-attachments/assets/624a4e31-fed7-4d37-9d08-9e2e4ff67f0b" />

<img width="1360" height="608" alt="Screenshot 2026-01-14 085018" src="https://github.com/user-attachments/assets/8893bda5-b88a-4cf1-b838-d49e08c5c8c4" />

<img width="1359" height="603" alt="Screenshot 2026-01-14 085608" src="https://github.com/user-attachments/assets/a2670023-367d-4a9a-9c61-a3277fddcb90" />

<img width="1357" height="600" alt="Screenshot 2026-01-14 085236" src="https://github.com/user-attachments/assets/9c60561c-bec6-45b0-8a17-d024576a6529" />

<img width="1360" height="607" alt="Screenshot 2026-01-14 085210" src="https://github.com/user-attachments/assets/377a4efd-80e5-4cb7-99a3-6f819fca471b" />

###bPart 3 — Create Enterprise Apps + Group-Based Access
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

<img width="331" height="139" alt="Screenshot 2026-01-14 085658" src="https://github.com/user-attachments/assets/089601c6-7e26-4985-a747-33bfb7ce0a01" />

<img width="558" height="170" alt="Screenshot 2026-01-14 085905" src="https://github.com/user-attachments/assets/02407d8e-b767-48fa-a92f-971269dedd86" />

<img width="592" height="562" alt="Screenshot 2026-01-14 090017" src="https://github.com/user-attachments/assets/a4566405-4127-4354-928c-d2cff596a1a0" />


<img width="366" height="184" alt="Screenshot 2026-01-14 090052" src="https://github.com/user-attachments/assets/92ea95b6-0945-4cdc-9c9c-5f8570584ee6" />

<img width="992" height="602" alt="Screenshot 2026-01-14 090126" src="https://github.com/user-attachments/assets/731c9903-efda-42d7-b48e-2527a517acef" />

<img width="586" height="565" alt="Screenshot 2026-01-14 090308" src="https://github.com/user-attachments/assets/13688c53-6522-4ae8-8252-aa2b813eaff9" />

<img width="1359" height="601" alt="Screenshot 2026-01-14 090440" src="https://github.com/user-attachments/assets/4245965f-6a81-41d7-a3a0-4e30f572d4c2" />

<img width="588" height="555" alt="Screenshot 2026-01-14 090605" src="https://github.com/user-attachments/assets/7880fdc0-6a2d-4b05-b1c7-07e0db0f1a60" />

### 🔄 Identity Lifecycle Testing (JML)
✅ Joiner Test — Automated Onboarding
Steps
Create a new user: newhire.user

Set: Department = HR

Wait 1–3 minutes

Expected results
User is automatically added to IAM-HR-Users-Dynamic

HR app access is granted through group assignment

📸 Screenshots

<img width="630" height="237" alt="Screenshot 2026-01-14 090735" src="https://github.com/user-attachments/assets/6d0789d1-2505-4149-84fb-e8e5210925bd" />

<img width="701" height="590" alt="Screenshot 2026-01-14 091156" src="https://github.com/user-attachments/assets/5018d834-1ec9-40df-9727-9d0a3e0255ac" />

<img width="892" height="597" alt="Screenshot 2026-01-14 091208" src="https://github.com/user-attachments/assets/fb293536-8719-4c95-98eb-052fc1503689" />

<img width="1357" height="602" alt="Screenshot 2026-01-14 091356" src="https://github.com/user-attachments/assets/ddf9e0c5-77ad-4e9c-8c05-3d46dc021b0b" />

<img width="1342" height="512" alt="Screenshot 2026-01-14 091512" src="https://github.com/user-attachments/assets/20c71f4b-803c-40be-bda7-2461a214b720" />

<img width="709" height="593" alt="Screenshot 2026-01-14 091521" src="https://github.com/user-attachments/assets/d7fa865c-c51a-4d4d-9b2e-043395815cd4" />

<img width="1362" height="607" alt="Screenshot 2026-01-14 091534" src="https://github.com/user-attachments/assets/586600e6-6279-49c6-94c8-d8c9d98cfc60" />

### 🏅 Mover Test — Automated Role Change
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

<img width="673" height="392" alt="Screenshot 2026-01-14 092113" src="https://github.com/user-attachments/assets/99d8ec92-1575-4c82-bddb-0e69a244695f" />

<img width="877" height="544" alt="Screenshot 2026-01-14 092158" src="https://github.com/user-attachments/assets/ed0bb971-2aac-4dfc-adeb-0eb6f0c6f0a0" />

<img width="1356" height="604" alt="Screenshot 2026-01-14 092838" src="https://github.com/user-attachments/assets/51a91a60-66d8-4b35-ad46-307276af9b90" />

<img width="1152" height="459" alt="Screenshot 2026-01-14 092947" src="https://github.com/user-attachments/assets/c86f8bf2-6a87-4dc9-b04c-750ba6fa70f4" />

<img width="1323" height="545" alt="Screenshot 2026-01-14 093020" src="https://github.com/user-attachments/assets/eaea0484-495f-46ce-a17e-de32f0002335" />

### 🛡 Leaver Test — Secure Offboarding
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

<img width="885" height="462" alt="Screenshot 2026-01-14 110050" src="https://github.com/user-attachments/assets/ac1b162c-de45-4481-9b91-e67040ac4df8" />

<img width="1156" height="148" alt="Screenshot 2026-01-14 100547" src="https://github.com/user-attachments/assets/f05ee14b-119e-48e1-9d14-f4a16143ca8e" />

<img width="706" height="117" alt="Screenshot 2026-01-14 100555" src="https://github.com/user-attachments/assets/511d0055-c908-4495-bf05-55db7077012a" />

<img width="356" height="81" alt="Screenshot 2026-01-14 100601" src="https://github.com/user-attachments/assets/bfa24414-e444-46e2-bca7-4775c47f7c38" />

### 📊 Audit & Compliance Evidence
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

<img width="1358" height="600" alt="Screenshot 2026-01-14 102413" src="https://github.com/user-attachments/assets/a1f2de4e-b9b0-4a84-83b9-061aabffdae8" />

<img width="1359" height="606" alt="Screenshot 2026-01-14 102809" src="https://github.com/user-attachments/assets/b051bb54-8874-414b-b930-cb25f4b071e8" />

<img width="1029" height="560" alt="Screenshot 2026-01-14 102841" src="https://github.com/user-attachments/assets/6ba2bcdb-1d66-4478-b448-0dac36f1b1ce" />

Sign-in Logs
Go to: Monitoring → Sign-in logs

Filter by user:

newhire.user

Capture:

Successful sign-in (before offboarding, if tested)

Failed sign-in (after account disabled)

📸 Screenshots

<img width="493" height="402" alt="Screenshot 2026-01-14 100641" src="https://github.com/user-attachments/assets/e8c0c60e-025a-4543-9d3b-86ab86a2a267" />

<img width="481" height="382" alt="Screenshot 2026-01-14 100647" src="https://github.com/user-attachments/assets/630d7f22-a706-481a-85ee-b132091a097e" />

<img width="1364" height="603" alt="Screenshot 2026-01-14 103127" src="https://github.com/user-attachments/assets/9438b064-97d7-41ca-b141-1289cd524c68" />

🛡 Security Controls Applied
Attribute-driven access automation (Dynamic Groups)

Group-based enterprise application access

Conditional Access enforcement (MFA / policies)

Privileged Identity Management (PIM)

Session revocation to prevent token reuse

Centralized logging for auditing and investigations

### 🧠 Key Skills Demonstrated
Identity lifecycle management (Joiner–Mover–Leaver)

Microsoft Entra ID administration

Dynamic group rule design

Group-based access control

Enterprise application access governance

IAM troubleshooting and validation

Audit log review and evidence collection

### 📌 Lessons Learned
Dynamic groups provide scalable IAM automation

Attribute quality is critical for correct access

Offboarding must include session/token revocation

Audit logs validate security controls and changes

