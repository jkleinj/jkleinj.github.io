---
layout: post
title: "rsync backups with increments"
date: 2020-05-03
---

The 'rsync' program is in my view the most efficient and robust tool for data backups,
unless one uses a file system that can take differential snapshots, like zfs for example.
I use 'rsync' in conjunction with 'cron' to schedule automated daily backups.
The backup should ideally reflect the latest state of the source file system.
However, sometimes errors happen, the user might have deleted files unvoluntarily
and a mirrored backup would wipe out those files as well.
In those cases it is helpful to keep additional incremental difference directories,
where deleted files are kept for a while as a safeguard against accidental data loss.
The following 'rsync' call achieves just that.

```
0 10,16 * * *  rsync -av --delete --backup --backup-dir=../rsync_laptop_user_old \
               -e 'ssh -p 54321' --timeout=120 \
               --exclude=.cache --exclude=.config --exclude=.local --exclude=.mozilla \
               --log-file=$HOME/.rsync/rsync_user_`date +\%y\%m\%d\%H`.log \
               /home/user/ user@192.168.1.185:/volume1/user_remote/rsync_laptop_user/
```

Step by step:

- 0 10,16 * * * : The cron job will be run daily at 10:00 and 16:00.
- rsync -av : Run rsync in archive mode and create verbose output.
- \-\-delete : Delete files on the target (=backup) file system that have been deleted on the source file system.
      In other words, create a mirror backup. 
- \-\-backup --backup-dir=../rsync_laptop_user_old : This is the location on the target (=backup) file system
      where files that have been deleted on the source file system (relative to the last backup) are going to be preserved.
      Note that the path is relative to the target path (see below).
- \-e 'ssh -p 55443' --timeout=120 : Use ssh encryption over port 54321 and a long timeout threshold of 120 seconds.
- \-\-exclude=.cache ... : Directories excluded from 'rsync' copy operations.
- \-\-log-file=$HOME/... : Output directory for the verbose log text. The 'date' command adds the current date and hour
      to the output file name. The backslash in the format file (\\%) is needed for execution in 'cron',
      but not required on the terminal shell.
- /home/user/ user@192.168.1.185:... : The actual copy instruction from the source (/home/user/)
      to the target path (user@192.168.1.185:/volume1/user_remote/rsync_laptop_user/),
      which is here a remote server on the local network.

The log files keep an incremental record of the files that are being copied and deleted.
Alternatively (or additionally), one could create incremental 'backup-dir's
with an added 'date' string, similar to the log files.

This backup option with incremental files works for deleted files.
What about files that have not been deleted but modified, for which we might want to access older versions?
Those files are usually text or code files and should be put under a version control system like 'git'.
The git files are included in the 'rsync' backup and therefore the version history is going to
be preserved along with the files themselves.

Happy rsyncing!

