Install and configure infrastructure with Ansible:

    ansible-playbook infra.yaml

Restore MySQL data from the backup:

    sudo -u backup -i
    duplicity --no-encryption restore rsync://andiikuiava@backup.megacoolproject.io/mysql /home/backup/restore/mysql

Verify the restored data:

    ls -l /home/backup/restore/mysql

Restore the MySQL data:

        sudo -i
        mysql agama < /home/backup/restore/mysql/agama.sql








InfluxDB data restore:

        service telegraf stop
        sudo -u backup duplicity --no-encryption restore rsync://andriikuiava@backup.megacoolproject.io/influxdb /home/backup/restore/influxdb

Verify the restored data:

        sudo su
        influx -execute 'DROP DATABASE telegraf'
        influxd restore -portable -database telegraf /home/backup/restore/influxdb


Run the playbook:
    
        ansible-playbook infra.yaml