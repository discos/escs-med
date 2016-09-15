Backup
------

Machines' backups are executed manually by operators after every significant
change. 

4 USB disks are stored at the Medicina station with labels on them indicating
which of the 4 machines they backup, each disk is partitioned exactly
as the original one and can be substituted in the server for a quick reboot in
case of failures.

For executing the backup simply connect the USB disk to the corresponding
machine and run::

    [root@escsSomething]# ./root/bin/doBackup

This will invoke rsync on each significant filesystem partition. if run with
*--boot* option the script will also add labels to the filesystems, and this
should be done only during the first backup operation on the disk.

