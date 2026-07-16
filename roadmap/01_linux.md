# 🐧 Linux for DevOps & Platform Engineers

> **Goal:** Build deep, production-grade Linux knowledge from the ground up. Every DevOps and Platform Engineer runs on Linux. This is non-negotiable.
>
> **Target:** 0–1 year experience → confident Linux operator
>
> **Philosophy:** Learn → Understand → Practice in a real terminal → Build → Review

---

## 📚 Resources

### Official / Primary
- **Linux man pages:** https://man7.org/linux/man-pages/
- **The Linux Command Line (free book):** https://linuxcommand.org/tlcl.php
- **Linux Journey (interactive):** https://linuxjourney.com/
- **Linux Foundation training:** https://training.linuxfoundation.org/

### Books
- *The Linux Command Line* — William Shotts (free online, buy the print)
- *How Linux Works* — Brian Ward (best internals book for practitioners)
- *Linux Bible* — Christopher Negus (reference)
- *UNIX and Linux System Administration Handbook* — Evi Nemeth et al.

### Courses / Videos
- **NetworkChuck Linux playlist:** https://youtube.com/c/NetworkChuck
- **LearnLinuxTV (YouTube):** https://www.youtube.com/@LearnLinuxTV
- **Linux Upskill Challenge (free, 20-day):** https://linuxupskillchallenge.org/
- **TechWorld with Nana — Linux for Beginners:** https://youtu.be/rrB13utjYV4
- **Kodekloud Linux Labs:** https://kodekloud.com/courses/the-linux-basics-course/

### Practice Labs
- **OverTheWire: Bandit (CTF-style Linux):** https://overthewire.org/wargames/bandit/
- **TryHackMe Linux rooms:** https://tryhackme.com/
- **KodeKloud Playgrounds:** https://kodekloud.com/playgrounds/
- **Play with Docker (has Linux):** https://labs.play-with-docker.com/

---

## Stage 0 — The Shell & File System

### Learn
- What is a shell? Bash vs sh vs zsh
- The file system hierarchy (/, /etc, /var, /home, /bin, /usr, /proc, /sys)
- Navigating: `pwd`, `ls`, `cd`, `tree`
- Files: `cat`, `less`, `more`, `head`, `tail`, `file`, `stat`
- Creating/moving: `touch`, `mkdir`, `cp`, `mv`, `rm`, `rmdir`
- Wildcards: `*`, `?`, `[]`
- Help: `man`, `info`, `--help`, `tldr`

### Understand
- Why is the file system a tree? What is the root `/`?
- Why is everything a file in Linux?
- What is the difference between absolute and relative paths?

### Guided Examples
```bash
# Explore the file system
ls -la /
ls -lh /etc | head -20
stat /etc/passwd
file /bin/bash

# Navigate and create
mkdir -p ~/projects/linux-practice
cd ~/projects/linux-practice
touch notes.txt
echo "Hello Linux" > notes.txt
cat notes.txt
```

### 🟢 Easy Exercises
1. List all files in `/etc` sorted by modification time (newest first)
2. Show only the first 5 and last 5 lines of `/var/log/syslog` (or `/var/log/messages`)
3. Create a directory tree: `projects/linux/stage0/` in one command
4. Copy `/etc/hostname` to your home directory and rename it to `my_hostname.txt`
5. Find all files with `.conf` extension in `/etc` (use `ls` with wildcards)

### 🟡 Medium Exercises
1. Display the 10 largest files/directories in `/var` using `du` and `sort`
2. Show all hidden files in your home directory and count them
3. Create a file, copy it to 3 locations, then delete the originals using a for loop in a one-liner
4. Display the file system hierarchy under `/usr` to exactly 2 levels deep using `tree`

### 🔴 Hard Exercises
1. Write a Bash script that prints a neatly formatted file system report: total/used/free space, top 5 largest directories under `/var`, and current timestamp
2. Explore `/proc/meminfo` and `/proc/cpuinfo` and write a script that summarizes CPU count and total RAM in a human-readable format

### 💀 Expert
1. Implement a basic `ls -la` equivalent entirely in Bash using `/proc` and `/sys` without calling `ls`
2. Write a script that monitors a directory for new files and logs each new file's name, size, and timestamp

### 🏗 DevOps Project
Build a `system_info.sh` script that prints a formatted system report: hostname, OS version, kernel, uptime, CPU cores, RAM, disk usage per mount point.

### ✅ Self Check
- Can I navigate anywhere in the file system without thinking?
- Can I explain what `/proc`, `/sys`, and `/dev` are for?
- Can I find any file on the system?

---

## Stage 1 — Users, Groups & Permissions

### Learn
- Users: `whoami`, `id`, `w`, `who`, `last`
- User management: `useradd`, `usermod`, `userdel`, `passwd`
- Groups: `groupadd`, `groups`, `/etc/group`
- File permissions: `rwx`, numeric mode, `chmod`, `chown`, `chgrp`
- Special bits: `setuid`, `setgid`, `sticky bit`
- `sudo` and `/etc/sudoers`

### Resources
- https://www.redhat.com/sysadmin/linux-file-permissions-explained
- https://linuxize.com/post/understanding-linux-file-permissions/

### 🟢 Easy Exercises
1. Create a user `devops`, set a password, create their home directory
2. Add `devops` to the `sudo` group
3. Create a file owned by root that only root can write to, but anyone can read
4. Change a directory so that only the owner can delete files inside it (sticky bit)
5. Show all users in the `sudo` group

### 🟡 Medium Exercises
1. Create a shared project directory accessible to a group `devteam` but not others
2. Write a script that creates 5 users (`dev01`–`dev05`) with a home directory and adds them all to a `devteam` group
3. Find all files on the system with SUID set and list them (potential security issue)

### 🔴 Hard Exercises
1. Configure `/etc/sudoers` (via `visudo`) so that `devops` user can run only specific commands without a password: `systemctl restart nginx` and `journalctl -u nginx`
2. Write a script that audits all world-writable files in `/etc` and reports them

### 🏗 DevOps Project
Write a `user_audit.sh` script that lists all users with login shells, their groups, last login time, and whether they have sudo access.

---

## Stage 2 — Process Management

### Learn
- Viewing processes: `ps`, `top`, `htop`, `pgrep`, `pstree`
- Process states (Running, Sleeping, Zombie, Stopped)
- Signals: `kill`, `killall`, `pkill`, signals 9 and 15
- Background/foreground: `&`, `fg`, `bg`, `jobs`
- Priority: `nice`, `renice`
- `/proc/[pid]/` — process information

### Resources
- https://www.digitalocean.com/community/tutorials/how-to-use-ps-aux-command-in-linux
- https://www.redhat.com/sysadmin/linux-signals

### 🟢 Easy Exercises
1. Find all processes owned by your user and sort by CPU usage
2. Kill a process by name using `pkill`
3. Run a long sleep command in the background, then bring it to the foreground
4. Find the PID of `sshd` and display its full command line from `/proc`
5. Check how long the system has been running and current load average

### 🟡 Medium Exercises
1. Write a script that monitors a process by name and restarts it if it dies
2. Find all zombie processes on the system and identify their parent PIDs
3. Reduce the priority of a CPU-intensive background job using `renice`

### 🔴 Hard Exercises
1. Write a script that watches for processes using more than X% CPU for more than Y seconds and sends a SIGTERM, then SIGKILL after another 10 seconds
2. Trace system calls made by a process using `strace` and explain what you observe

### 🏗 DevOps Project
Build a `process_monitor.sh` script: takes a process name as an argument, checks if it's running every 30 seconds, and logs status changes to a file with timestamps.

---

## Stage 3 — File Operations: grep, awk, sed, find

### Learn
- `grep`: pattern matching, regex, `-i`, `-r`, `-l`, `-c`, `-n`, `-v`
- `awk`: field processing, `BEGIN/END`, conditionals, arithmetic
- `sed`: stream editing, substitution, in-place editing
- `find`: by name, type, size, time, permissions; `-exec`
- `xargs`: piping find results
- `cut`, `sort`, `uniq`, `wc`, `tr`

### Resources
- https://www.gnu.org/software/grep/manual/grep.html
- https://www.gnu.org/software/awk/manual/awk.html
- https://www.grymoire.com/Unix/Sed.html
- https://linux.die.net/man/1/find

### 🟢 Easy Exercises
1. Count how many lines in `/var/log/syslog` contain the word "error" (case-insensitive)
2. Print only the IP addresses from a file of nginx log lines using `grep`
3. Find all `.log` files in `/var/log` larger than 10MB
4. Replace all occurrences of "localhost" with "127.0.0.1" in a config file using `sed`
5. Print the 3rd column (space-delimited) from every line of `/etc/passwd` using `awk`

### 🟡 Medium Exercises
1. Parse `/etc/passwd` to print a neat table of username and home directory for users with a login shell
2. Find all files modified in the last 24 hours in `/etc` and list them with their modification times
3. Use `awk` to calculate the total size of all `.log` files in a directory (summing the size column from `ls -l` output)
4. Extract unique HTTP status codes from an nginx access log and count occurrences of each

### 🔴 Hard Exercises
1. Write a one-liner pipeline that: reads an nginx log, extracts IPs, counts them, sorts by frequency, and prints the top 10
2. Use `find` + `xargs` + `grep` to search for a specific config string across all `.conf` files in `/etc` — handle spaces in filenames correctly

### 💀 Expert
1. Write a log analyzer in pure Bash (no Python/awk) that parses nginx combined log format and outputs: total requests, unique IPs, top 10 URLs, error rate percentage, and requests per minute

### 🏗 DevOps Project
Build a `log_report.sh` script: takes a log file path as input, detects the format (nginx/apache/syslog), and outputs a colored summary report.

---

## Stage 4 — Bash Scripting

### Learn
- Shebang: `#!/bin/bash`
- Variables, quoting rules
- Input: `$1`, `$@`, `read`, `getopts`
- Conditionals: `if`/`elif`/`else`, `test`, `[[`
- Loops: `for`, `while`, `until`
- Functions
- Exit codes and `$?`
- Arrays
- Here documents (`<<EOF`)
- Error handling: `set -euo pipefail`

### Resources
- **Bash Guide:** https://mywiki.wooledge.org/BashGuide
- **ShellCheck (linter):** https://www.shellcheck.net/
- **Bash Hackers Wiki:** https://wiki.bash-hackers.org/
- **Pure Bash Bible:** https://github.com/dylanaraps/pure-bash-bible

### 🟢 Easy Exercises
1. Write a script that accepts a name as `$1` and prints a greeting; exit with error if no argument given
2. Write a script that checks if a file exists, is readable, and is non-empty, and prints an appropriate message
3. Write a loop that creates 10 directories: `dir01` through `dir10`
4. Write a function that returns the absolute value of a number
5. Rewrite a manual multi-step task you do often as a Bash script

### 🟡 Medium Exercises
1. Write a script that takes a service name, checks if it's running (exit code), and starts it if not
2. Write a script that backs up a directory to `/tmp/backup_YYYYMMDD/` and removes backups older than 7 days
3. Write a script using `getopts` that accepts `-f <file>` and `-n <count>` flags
4. Write a script that reads a list of servers from a file and SSH-checks if port 22 is open on each

### 🔴 Hard Exercises
1. Write a `deploy.sh` script: take git repo URL and branch, clone it, run tests, and deploy only if tests pass — with full logging and rollback on failure
2. Write a script that processes a CSV file and generates an HTML table output

### 💀 Expert
1. Implement a mini task runner in pure Bash: reads a `Taskfile` (YAML-like syntax) and executes named targets with dependency resolution

### 🏗 DevOps Project
Build a `server_health.sh` script that checks CPU, memory, disk, and a list of services — outputs a JSON report and sends a Slack webhook alert if any metric exceeds a threshold.

---

## Stage 5 — Systemd & Service Management

### Learn
- `systemctl`: start, stop, restart, enable, disable, status, is-active
- Unit files: `[Unit]`, `[Service]`, `[Install]`
- Targets (runlevels)
- Service types: simple, forking, oneshot, notify
- `journalctl`: unit logs, follow, time filters
- Timers as cron replacement

### Resources
- https://www.freedesktop.org/software/systemd/man/systemd.service.html
- https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files
- https://wiki.archlinux.org/title/systemd

### 🟢 Easy Exercises
1. Check the status of `sshd` and `nginx` services
2. Start a service, verify it's running, stop it, and verify it stopped
3. Enable a service to start on boot; disable another
4. Read the last 100 lines of logs for a service using `journalctl`
5. Filter `journalctl` output to show only ERROR-level messages since last boot

### 🟡 Medium Exercises
1. Write a custom systemd service unit that runs your `server_health.sh` script
2. Create a systemd timer that runs your health check every 5 minutes
3. Write a service that restarts automatically on failure (`Restart=on-failure`)

### 🔴 Hard Exercises
1. Write a service that: starts after networking is up, depends on a database service, runs as a non-root user, and exports environment variables from a file
2. Debug a systemd service that fails to start — work through the investigation process using `journalctl`, `systemctl status`, and the unit file

### 🏗 DevOps Project
Package your `server_health.sh` script as a proper systemd service with a timer, install it on a Linux VM, and verify it runs on boot.

---

## Stage 6 — Storage & Disks

### Learn
- Disk info: `lsblk`, `fdisk -l`, `df -h`, `du -sh`
- Partitioning: `fdisk`, `parted`
- Filesystems: `mkfs.ext4`, `mkfs.xfs`, `mount`, `umount`, `/etc/fstab`
- LVM: `pvcreate`, `vgcreate`, `lvcreate`, `lvextend`, `resize2fs`
- Disk performance: `iostat`, `iotop`, `fio`
- NFS and network storage basics

### Resources
- https://www.digitalocean.com/community/tutorials/an-introduction-to-storage-terminology-and-concepts-in-linux
- https://tldp.org/HOWTO/LVM-HOWTO/

### 🟢 Easy Exercises
1. List all block devices and their sizes
2. Check disk usage of all mounted filesystems, sorted by usage percentage
3. Find the top 10 largest directories under `/var`
4. Mount a disk image to a directory and list its contents

### 🟡 Medium Exercises
1. Create a new partition, format it as ext4, mount it, and add an `/etc/fstab` entry for persistence
2. Create an LVM logical volume, format it, mount it, and then extend it without unmounting

### 🔴 Hard Exercises
1. Simulate a disk filling up scenario: set up alerts before it reaches 85% using a script + systemd timer
2. Write a script that automatically extends an LVM volume when usage exceeds a threshold

### 🏗 DevOps Project
Build a disk management script: discovers all mounted volumes, reports usage, sends alerts at 75%/90% thresholds, and logs history to a JSON file.

---

## Stage 7 — Networking on Linux

### Learn
- Network interfaces: `ip addr`, `ip link`, `ip route`, `ifconfig`
- DNS: `nslookup`, `dig`, `host`, `/etc/resolv.conf`, `/etc/hosts`
- Connectivity: `ping`, `traceroute`, `mtr`, `curl`, `wget`
- Port scanning: `netstat`, `ss`, `nmap`
- `tcpdump` for packet capture
- `iptables` basics (INPUT, OUTPUT, FORWARD chains)

### Resources
- https://www.digitalocean.com/community/tutorials/how-to-configure-networking-in-linux
- https://linux.die.net/man/8/ip
- **Julia Evans's tcpdump zine:** https://jvns.ca/

### 🟢 Easy Exercises
1. Show all network interfaces and their IP addresses
2. Show the default gateway and routing table
3. Test DNS resolution for `google.com` using both `dig` and `nslookup`
4. Show all listening ports and the processes using them
5. Capture 10 HTTP packets on the `eth0` interface using `tcpdump`

### 🟡 Medium Exercises
1. Trace the full network path to `8.8.8.8` and explain each hop
2. Block incoming traffic on port 8080 using `iptables` and verify with `nmap`
3. Write a script that checks if a list of hosts is reachable (ping test) and DNS-resolvable

### 🔴 Hard Exercises
1. Use `tcpdump` to capture and analyze an HTTP request — identify the host, path, and status code from the raw packets
2. Set up a persistent firewall rule that allows only SSH and HTTP/HTTPS, blocks everything else, and survives a reboot

### 🏗 DevOps Project
Build a `network_audit.sh` script: discovers all listening services, checks external connectivity, tests DNS resolution for a list of critical domains, and outputs a JSON health report.

---

## Stage 8 — Log Management

### Learn
- Syslog: `/var/log/syslog`, `/var/log/messages`, `/var/log/auth.log`
- `rsyslog` configuration
- Log rotation: `logrotate`
- `journald` and structured logging
- Log parsing: extracting signals from noise

### Resources
- https://www.loggly.com/ultimate-guide/linux-logging-with-rsyslog/
- https://www.digitalocean.com/community/tutorials/how-to-manage-logfiles-with-logrotate-on-ubuntu-16-04

### 🟢 Easy Exercises
1. Find all authentication failures in `/var/log/auth.log`
2. Show all kernel messages since last boot using `journalctl -k`
3. Show logs from `nginx` service for the last hour
4. Count total lines in all `.log` files in `/var/log`

### 🟡 Medium Exercises
1. Write a `logrotate.conf` that rotates an application log daily, keeps 7 days, compresses old logs, and runs a post-rotate hook
2. Write a script that monitors `/var/log/auth.log` for failed SSH login attempts and blocks IPs with more than 5 failures using `iptables`

### 🔴 Hard Exercises
1. Set up `rsyslog` to forward all WARNING+ logs to a remote syslog server (or a file acting as remote) over TCP
2. Write a parser that reads `journald` JSON output and creates a structured alert if any message contains "OOM" or "segfault"

### 🏗 DevOps Project
Build a log analysis pipeline: `tail -f` a log file, parse each line, categorize by level, and write a live summary that updates every 5 seconds in the terminal.

---

## Stage 9 — Security Hardening

### Learn
- SSH hardening: key auth, disable root, `AllowUsers`, `Port`
- Fail2ban
- UFW/iptables firewall rules
- `auditd` for system auditing
- File integrity: `aide` or `tripwire`
- CIS Benchmarks (what to check and why)

### Resources
- **CIS Benchmarks:** https://www.cisecurity.org/cis-benchmarks/
- **Lynis (audit tool):** https://cisofy.com/lynis/
- https://www.ssh.com/academy/ssh/sshd_config

### 🟢 Easy Exercises
1. Disable password authentication for SSH, enable key-based auth only
2. Change the SSH default port and update the firewall accordingly
3. Install and configure `fail2ban` to block IPs after 3 failed SSH attempts
4. Run `lynis audit system` and review the hardening index score

### 🟡 Medium Exercises
1. Implement a full UFW firewall policy: deny all inbound, allow SSH (22) and HTTPS (443) only
2. Set up `auditd` to log all writes to `/etc/passwd` and `/etc/sudoers`
3. Find all users with empty passwords and disable their accounts

### 🔴 Hard Exercises
1. Apply a CIS Level 1 benchmark for Ubuntu (top 20 controls) using a Bash script
2. Write a daily security audit script that checks: SSH config, open ports, SUID files, world-writable files, unauthorized sudoers entries — and emails a report

### 🏗 DevOps Project
Build a `harden.sh` script that applies a baseline security configuration to a fresh Linux server: SSH hardening, firewall setup, fail2ban, disable unused services, and generates a before/after audit report.

---

## Stage 10 — Performance Analysis & Tuning

### Learn
- CPU: `top`, `htop`, `mpstat`, `vmstat`, `perf`
- Memory: `free`, `vmstat`, `/proc/meminfo`, swap
- Disk I/O: `iostat`, `iotop`, `fio`
- Network I/O: `iftop`, `nethogs`, `nload`
- System calls: `strace`, `ltrace`
- Linux tuning: `sysctl`, `/proc/sys/`

### Resources
- **Brendan Gregg's Linux Performance tools:** https://www.brendangregg.com/linuxperf.html
- *Systems Performance* — Brendan Gregg (the bible of Linux perf)
- https://perf.wiki.kernel.org/

### 🟢 Easy Exercises
1. Identify the top 5 CPU-consuming processes right now
2. Show current memory usage, swap usage, and memory pressure indicators
3. Find the process doing the most disk I/O
4. Show network throughput per interface in real time

### 🟡 Medium Exercises
1. Use `fio` to benchmark disk read/write speeds and explain the results
2. Simulate high memory pressure and observe how the OOM killer responds
3. Profile a CPU-intensive script using `perf stat` and explain the output metrics

### 🔴 Hard Exercises
1. Use `strace` to trace a failing service and identify exactly which system call fails and why
2. Tune kernel parameters via `sysctl` to optimize for a high-connection-count web server workload and document every change with the reason

### 🏗 DevOps Project
Build a performance baseline script: captures CPU, memory, disk, network metrics at 5-second intervals for 5 minutes, saves to a JSON file, and generates a summary report.

---

## 🔗 Additional Linux Resources

### Interactive Practice
- **Vim Adventures:** https://vim-adventures.com/ (learn vim the fun way)
- **Regex101:** https://regex101.com/ (practice grep patterns)
- **ExplainShell:** https://explainshell.com/ (explains any command)
- **OverTheWire Bandit:** https://overthewire.org/wargames/bandit/

### Cheat Sheets
- https://cheatography.com/davechild/cheat-sheets/linux-command-line/
- https://github.com/trinib/Linux-Bash-Commands (community cheat sheet)

### Must-Read Articles
- "The Art of the Command Line" https://github.com/jlevy/the-art-of-command-line
- "Linux Performance Analysis in 60,000 Milliseconds" https://netflixtechblog.com/linux-performance-analysis-in-60-000-milliseconds-accc10403c55

---

*Progress: 0/10 stages complete · Est. time to complete: 6–10 weeks at 3 hrs/day*
