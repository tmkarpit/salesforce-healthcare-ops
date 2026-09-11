# 🏥 Healthcare Operations Management System — Salesforce

> A Salesforce-native platform for managing patient records, doctor scheduling, appointment automation, treatment tracking, medical inventory, and insurance coordination — built to demonstrate enterprise data modeling, declarative automation, and role-based security architecture on the Salesforce Platform.

---

## 📋 Professional Summary

This project simulates a real-world hospital operations system on Salesforce, addressing the core challenge healthcare providers face: coordinating patient flow, doctor availability, and treatment processes across departments while maintaining strict data security and compliance standards.

Built entirely using Salesforce's declarative toolset — custom objects, Flow Builder automation, validation rules, and role-based sharing — the system reduces manual scheduling conflicts, automates operational alerts (low inventory, non-emergency triage), and gives administrators real-time visibility into hospital operations through a live dashboard.

**Key engineering decisions demonstrated in this project:**
- Relational data modeling using Lookup vs. Master-Detail relationships based on ownership and cascade-delete requirements
- Conflict-detection logic in Screen Flows to prevent double-booking before record creation
- Private org-wide defaults with field-level security to protect sensitive medical data, layered with sharing rules for legitimate cross-role access
- Event-driven automation (record-triggered flows) for inventory and scheduling alerts, rather than manual admin monitoring

---

## ✨ Features

| Module | Description |
|---|---|
| 🗂️ **Data Model** | 7 interconnected custom objects covering patients, doctors, departments, appointments, treatments, inventory, and insurance |
| 📅 **Appointment Booking** | Interactive Screen Flow with real-time doctor availability filtering and automatic double-booking prevention |
| 🔔 **Automated Reminders** | Scheduled-path automation sends appointment reminders ahead of time |
| 📦 **Inventory Management** | Automatic reorder task creation when medical supplies fall below threshold |
| 🚑 **Triage Escalation** | Automatic task creation for non-emergency appointments to reduce patient wait times |
| 🔒 **Security Architecture** | Role hierarchy, custom profiles, field-level security, and sharing rules protecting sensitive patient data |
| 📊 **Operations Dashboard** | Real-time reporting on appointment status, doctor utilization, no-show rates, and inventory levels |

---

## 🏗️ Data Model

```
Department__c ──┬── Doctor__c ──── Appointment__c ──── Treatment__c (Master-Detail)
                │        │              │
                │        │              └── Patient__c ──── Insurance_Provider__c
                │        │
                └── Medical_Supply__c
```

| Object | Purpose | Relationships |
|---|---|---|
| `Department__c` | Hospital departments | Lookup → `Doctor__c` (Head Doctor) |
| `Doctor__c` | Doctor profiles, specialization, availability | Lookup → `Department__c` |
| `Insurance_Provider__c` | Insurance company records | Referenced by `Patient__c` |
| `Patient__c` | Patient demographics and medical history | Lookup → `Insurance_Provider__c` |
| `Appointment__c` | Scheduled visits | Lookup → `Patient__c`, `Doctor__c` |
| `Treatment__c` | Diagnosis & prescribed treatment | Master-Detail → `Appointment__c` |
| `Medical_Supply__c` | Inventory tracking | Lookup → `Department__c` |

---

## ⚙️ Automation

**Book Appointment** *(Screen Flow)*
Guides staff through selecting a patient and an available doctor, checks for scheduling conflicts against existing appointments in real time, and blocks the booking with a clear message if the slot is already taken.

**Appointment Reminder** *(Record-Triggered Flow, Scheduled Path)*
Automatically sends a reminder email one day before the scheduled appointment.

**Low Inventory Reorder Alert** *(Record-Triggered Flow)*
Detects when a medical supply's quantity falls below its reorder threshold and automatically generates a reorder task with a notification.

**Non-Emergency Wait Time Escalation** *(Record-Triggered Flow)*
Flags newly created non-emergency appointments for manual front-desk triage, supporting faster turnaround for routine cases.

---

## 🛡️ Security Architecture

- **Org-Wide Defaults:** `Patient__c` and `Treatment__c` set to **Private**
- **Role Hierarchy:** Hospital Admin → Department Head → Doctor → Front Desk Staff
- **Custom Profiles:** Distinct Doctor, Front Desk, and Admin profiles with scoped object permissions
- **Field-Level Security:** `Medical_History__c` hidden from Front Desk profile
- **Sharing Rules:** Patient records shared with the Doctor role on top of the Private baseline

---

## 📊 Reports & Dashboard

**Hospital Operations Overview** dashboard includes:
- Appointments by Status (pie chart)
- Doctor Utilization (bar chart)
- No-Show Rate (metric)
- Low Stock Inventory Items (table)

---

## 🧰 Tech Stack

`Salesforce Platform` · `Custom Objects & Relationships` · `Flow Builder` · `Validation Rules` · `Profiles & Role Hierarchy` · `Field-Level Security` · `Sharing Rules` · `Reports & Dashboards` · `Salesforce CLI (SFDX)`

---

## 📁 Project Structure

```
force-app/main/default/
├── objects/          # Custom object definitions and fields
├── flows/             # Screen Flow and Record-Triggered Flow metadata
├── layouts/            # Page layouts per object
├── profiles/           # Custom profile permissions
└── reports/            # Report definitions
```

---

## 🚀 Future Enhancements

- [ ] Apex REST callout for real-time insurance verification against an external API
- [ ] Experience Cloud self-service portal for patients to book their own appointments
- [ ] Salesforce Shield Platform Encryption on sensitive medical fields
- [ ] Batch Apex for archiving historical records at scale

---

## 👤 About This Project

Built end-to-end in a Salesforce Developer Edition org as a hands-on demonstration of practical Salesforce architecture: relational data modeling, declarative process automation, enterprise-grade security design, and operational reporting — applied to a realistic healthcare operations scenario.
