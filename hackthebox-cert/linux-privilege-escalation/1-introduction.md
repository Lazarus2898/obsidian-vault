# The Basics
Listing the Current Processes
`ps aux | grep root`

Listing Current Terminal-Attached Processes
`ps au`

Looking through the Home directory
`ls /home`
`ls -la /home/usr`

SSH scripts
`ls -l ~/.ssh`

History
`history`

What are the user privs
`sudo -l`

Any configuration files
`cat /etc/passwd`

CronJobs
`ls -la /etc/cron.daily`

File systems
`lsblk`

Writeable directories/files
`find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null`
`find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null`
