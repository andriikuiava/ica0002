# Backup SLA

## Coverage

We back up services that satisfy at least one of these criteria:
- Are the primary source of truth for particular data.
- Contain customer and/or client data.
- Are not feasible (or very costly) to restore by other means.

Services that are backed up:
- **MySQL** (contains Agama application data).
- **InfluxDB** (stores monitoring data and statistics).
- **Ansible Git repository** (infrastructure as code).

## Schedule

- **MySQL**:
    - **Full backup** every **Sunday** at **00:12 UTC**.
    - **Incremental backups** from **Monday to Saturday** at **00:12 UTC**.

- **InfluxDB**:
    - **Full backup** every day at **00:12 UTC**.
    - **Incremental backups** every **Sunday** at **00:12 UTC**.

- **Ansible Git repository**:
    - Backups are created in **real-time** and mirrored automatically to the internal Git server.

## Retention

- **MySQL**:
    - **3 full backup versions** and **1 incremental backup version** stored.
    - Retention period: **40 days**.

- **InfluxDB**:
    - **2 full backup versions** and **2 incremental backup versions** stored.
    - Retention period: **40 days**.

- **Ansible Git repository**:
    - Backups are stored for **6 months** with **40 versions** available.

## Usability Checks

- **MySQL**:
    - The "Alpha" team tests backups every **Monday** by restoring them to a **sandbox** or **non-production environment**.

- **InfluxDB**:
    - The "Beta" team tests backups every **Monday** by restoring them to a **sandbox** or **non-production environment**.

- **Ansible Git repository**:
    - Backups are verified by **periodic test restores** to a separate directory.

## Restore Process

Service is recovered from the backup in case of an incident, and when the service cannot be restored in any other way.

RTO (recovery time objective):
- **MySQL**: **8 hours**.
- **InfluxDB**: **8 hours**.
- **Ansible Git repository**: **8 hours**.

Restoration criteria include:
- **MySQL** and **InfluxDB**: Database corruption, accidental data deletion, or ransomware encryption.
- **Ansible Git repository**: Critical playbook or role deletion or repository corruption.




# Backup Policy and SLA

## Purpose
This SLA outlines the backup and recovery strategy for critical services managed within the Ansible repository. These services include:
- Configurable services: nginx, uwsgi, bind9, agama, prometheus, pinger, grafana.
- Recoverable services: MySQL, InfluxDB.

## Scope
- Configurable services: Configurations can be restored using the Ansible repository.
- Recoverable services: Full data backups are created and stored in multiple locations to ensure data integrity.

---

## Backup Objectives

### Recovery Point Objective (RPO)
- The RPO is 24 hours, ensuring data generated within the past day is preserved in case of a disaster.

### Recovery Time Objective (RTO)
- The RTO is 2 hours, targeting the complete restoration of services and data within this period.

---

## Backup Schedule and Retention

- MySQL Full Backup: Sundays at 20:15 UTC
- InfluxDB Full Backup: Sundays at 21:15 UTC
- MySQL Incremental Backup: Daily (excluding Sunday) at 20:20 UTC
- InfluxDB Incremental Backup: Daily (excluding Sunday) at 21:20 UTC

Backups are retained for 6 months to ensure long-term data recovery capabilities. Each backup is timestamped and labeled for easy identification.

---

## Usability Checks
Backup integrity checks are conducted every 14 days. These tests include:
- Verifying backup files.
- Restoring data to test environments.
- Validating the integrity of recovered data.

Issues identified during these checks are resolved immediately to maintain reliability.

---

## Restoration Process

1. Configurable Services:  
   Restored by re-running the Ansible repository.

2. Recoverable Services (MySQL, InfluxDB):  
   Follow detailed steps in the backup_restore.md document for data recovery.

---