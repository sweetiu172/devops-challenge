Provide your solution here:

# Step by step debug
## Assumptions
- You have root privileges over the VM
- You can use ssh command to access the VM
## Scenario #1: Supposed we have large log files
Over time, NGINX access/error logs can grow excessively large due to traffic or verbose logging settings, filling up the disk.

- Check log files sizes
```bash
du -sh /var/log/nginx/*
```
```log
8.0K    /var/log/nginx/access.log
0       /var/log/nginx/error.log
```

In the above example, problem doesn't come from the files sizes. But suppose the file is so large such as 50GB.

- Temporary solution:
We can compress the files and send to a blob storage (such as S3), then we can truncate the file to free up storage space.

- Permanent solution:
Create a cronjob to check the total size of all files in /var/log/nginx/. If total size exceeds 60GB, we could compress, backup, and truncate the logs safely.

## Scenario #2: Out of storage because of temporary or cache files
If nginx log files are not the problem, probably there are temporay or cache files.

- Check overall disk usage by directories
```bash
sudo du -sh /* | sort -h
```
```log
0       /bin
0       /dev
0       /lib
0       /lib64
0       /proc
0       /sbin
0       /sys
4.0K    /bin.usr-is-merged
4.0K    /lib.usr-is-merged
4.0K    /media
4.0K    /mnt
4.0K    /opt
4.0K    /sbin.usr-is-merged
4.0K    /srv
16K     /lost+found
32K     /home
40K     /root
64K     /tmp
892K    /run
6.4M    /etc
82M     /boot
486M    /var
506M    /snap
1.5G    /usr
```

- Identify directories taking excessive space, supposed they are /tmp and /var/tmp
- Clear APT cache and clear temporary files.
```bash
apt clean
rm -rf /var/lib/apt/lists/*
rm -rf /tmp/*
rm -rf /var/tmp/*
```
- Verify the free space again
```bash
df -h /
```