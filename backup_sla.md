# Backup SLA

## Coverage

We back up services that satisfy at least one of these criteria:
- are primary source of truth for particular data
- contain customer and/or client data
- are not feasible (or very costly) to restore by other means

Services that are backed up:
- **MySQL** (contains AGAMA application data)
- **InfluxDB** (stores monitoring data and statistics)
- **Ansible Git repository** (infrastructure as code)

## Schedule

- **MySQL** backups are created every **24 hours**; it takes up to **15 minutes** to create and store the backup.
- **InfluxDB** backups are created every **24 hours**; it takes up to **20 minutes** to create and store the backup.
- **Ansible Git repository** backups are mirrored automatically in real-time to the internal Git server.

All backups are started automatically by **cron jobs and backup automation scripts**.

Backup RPO (recovery point objective) is:
- **24 hours** for **MySQL**
- **24 hours** for **InfluxDB**
- **real-time** for **Ansible Git repository**

## Storage

- **MySQL** and **InfluxDB** backups are uploaded to the backup server.
- **Ansible Git repository** is mirrored to the internal Git server.

Backup data from both servers will be synchronized to an encrypted AWS S3 bucket in the future (work in progress).

## Retention

- **MySQL** backups are stored for **30 days**; **30 versions** (recovery points) are available to restore.
- **InfluxDB** backups are stored for **14 days**; **14 versions** are available to restore.
- **Ansible Git repository** backups are stored indefinitely as the repository is version-controlled.

## Usability checks

- **MySQL** backups are verified every **7 days** by **restoring to a test database and running integrity checks**.
- **InfluxDB** backups are verified every **7 days** by **restoring to a test instance and checking data integrity**.
- **Ansible Git repository** backups are verified every **push** by **CI/CD pipeline checks**.

## Restore process

Service is recovered from the backup in case of an incident, and when the service cannot be restored in any other way.

RTO (recovery time objective) is:
- **1 hour** for **MySQL**
- **1.5 hours** for **InfluxDB**
- **immediate** for **Ansible Git repository** (cloned directly from the mirror)

Detailed backup restore procedures are documented in the [backup_restore.md](./backup_restore.md).