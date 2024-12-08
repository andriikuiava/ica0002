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