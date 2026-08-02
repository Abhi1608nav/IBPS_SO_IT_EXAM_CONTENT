# Linux, Unix &amp; Windows Commands — Comprehensive Notes
### (Basic to Advanced) — IBPS SO / SBI SO IT Officer

---

## 1. Linux vs Unix — Quick Context

| Aspect | Unix | Linux |
|---|---|---|
| Origin | Developed at AT&T Bell Labs (1969) | Created by Linus Torvalds (1991), inspired by Unix |
| Kernel | Proprietary (varies by vendor) | Open-source Linux kernel |
| Examples | Solaris, AIX, HP-UX, macOS (BSD-based) | Ubuntu, RHEL, CentOS, Fedora, Debian |
| License | Mostly commercial/proprietary | Mostly free & open-source (GPL) |
| Command syntax | Nearly identical to Linux (POSIX-compliant) | Same command set, extended with GNU utilities |

> **Exam tip:** Since Linux commands are POSIX-compliant and modeled on Unix, almost all commands below work identically on both — that's why they're grouped together.

---

## 2. File & Directory Navigation (Basic)

| Command | Purpose | Example |
|---|---|---|
| `pwd` | Print Working Directory — shows absolute path of current location | `pwd` → `/home/user` |
| `ls` | Lists files/directories | `ls -l` (long format), `ls -a` (show hidden files), `ls -lh` (human-readable sizes) |
| `cd` | Change Directory | `cd /var/log`, `cd ..` (up one level), `cd ~` (home directory) |
| `mkdir` | Make a new directory | `mkdir project`, `mkdir -p a/b/c` (create nested dirs) |
| `rmdir` | Remove an **empty** directory | `rmdir old_folder` |
| `touch` | Create an empty file, or update its timestamp if it exists | `touch file.txt` |
| `cp` | Copy files/directories | `cp file1 file2`, `cp -r dir1 dir2` (recursive for directories) |
| `mv` | Move or rename files/directories | `mv old.txt new.txt` |
| `rm` | Remove files | `rm file.txt`, `rm -r dir/` (recursive), `rm -rf dir/` (force, no prompt — **dangerous**) |

### Directory Navigation Trick (frequently tested)
```
Current: /var/log
cd ../tmp   → moves UP one level to /var, then INTO tmp  →  Result: /var/tmp
cd tmp      → moves INTO a subfolder named tmp inside current dir → /var/log/tmp
cd /tmp     → absolute path, goes directly to root-level /tmp
```

---

## 3. Viewing & Editing Files

| Command | Purpose |
|---|---|
| `cat` | Displays entire file content at once (`cat file.txt`) |
| `less` | View file page-by-page (scrollable, doesn't load whole file into memory) |
| `more` | Similar to `less`, but more basic (older, forward-only scrolling) |
| `head` | Shows first 10 lines by default (`head -n 20 file.txt` for first 20) |
| `tail` | Shows last 10 lines by default (`tail -f file.txt` — **follow mode**, live-updates as file grows, very useful for log monitoring) |
| `nano` | Simple, beginner-friendly terminal text editor |
| `vi` / `vim` | Powerful modal text editor (Insert mode `i`, Command mode `Esc`, Save & quit `:wq`) |

---

## 4. File Permissions & Ownership

| Command | Purpose |
|---|---|
| `chmod` | Changes file/directory **permissions** (read/write/execute) |
| `chown` | Changes file **owner** |
| `chgrp` | Changes file **group** |
| `umask` | Sets default permission mask for newly created files |

### Understanding `chmod` (Numerical Notation) — Very Frequently Asked
```
Permission values:  r (read) = 4,  w (write) = 2,  x (execute) = 1

chmod 755 script.sh
        │││
        ││└─ Others: 5 = r-x (4+1)
        │└── Group:  5 = r-x (4+1)
        └─── Owner:  7 = rwx (4+2+1)

Result: rwxr-xr-x
```

| Value | Permission |
|---|---|
| 7 | rwx (read+write+execute) |
| 6 | rw- (read+write) |
| 5 | r-x (read+execute) |
| 4 | r-- (read only) |
| 0 | --- (no permission) |

**Symbolic notation alternative:** `chmod u+x file.sh` (add execute for user/owner), `chmod g-w file.sh` (remove write for group)

---

## 5. Searching for Files & Content

| Command | Purpose |
|---|---|
| `find` | Searches for files in a directory tree — by name, size, type, modification time, etc. Searches **live/real-time**. `find / -name "*.log"` |
| `locate` | Searches using a **pre-built database** (`updatedb`) — much faster than `find`, but can return stale results if the DB isn't refreshed recently |
| `grep` | Searches for a **text pattern** inside file contents (not filenames) `grep "error" logfile.txt` |
| `grep -r` | Recursively searches a pattern across all files in a directory tree |
| `grep -i` | Case-insensitive search |
| `grep -ri "error" .` | Recursively + case-insensitively searches for "error" starting from current directory (`.`) |
| `which` | Shows the full path of a command's executable (`which python3`) |
| `whereis` | Locates binary, source, and manual page files for a command |

### find vs locate — Key Difference (exam favorite)
| Feature | `find` | `locate` |
|---|---|---|
| Search method | Live traversal of file system | Queries a pre-indexed database |
| Speed | Slower (searches in real-time) | Much faster |
| Accuracy | Always up-to-date | Can be outdated until `updatedb` runs |
| Filter options | Very rich (size, time, permissions, type) | Name-based only |

---

## 6. Text Processing (Intermediate–Advanced)

| Command | Purpose |
|---|---|
| `awk` | Powerful pattern-scanning and text-processing language — used for column-based data extraction. `awk '{print $1}' file.txt` prints the first column |
| `sed` | Stream editor for find-and-replace and text transformations. `sed 's/old/new/g' file.txt` replaces all occurrences |
| `cut` | Extracts specific columns/fields from text. `cut -d',' -f1 file.csv` (splits by comma, prints field 1) |
| `sort` | Sorts lines of text (`sort -n` numeric, `sort -r` reverse) |
| `uniq` | Removes duplicate adjacent lines (often used with `sort` first) |
| `wc` | Word/line/character count. `wc -l file.txt` counts lines |
| `diff` | Compares two files and reports the differences line-by-line. `diff file1.txt file2.txt` |
| `tr` | Translates or deletes characters (`tr 'a-z' 'A-Z'` converts to uppercase) |
| `xargs` | Builds and executes commands from standard input — used to pass output of one command as arguments to another |

---

## 7. Process Management

| Command | Purpose |
|---|---|
| `ps` | Displays currently running processes (`ps aux` for all processes with details) |
| `top` | Real-time, dynamic view of running processes and system resource usage |
| `htop` | Enhanced, colorized, interactive version of `top` (not always pre-installed) |
| `kill` | Terminates a process by its PID (`kill 1234`) |
| `kill -9` | Force-kills a process (SIGKILL, cannot be ignored) |
| `killall` | Kills all processes matching a name (`killall firefox`) |
| `jobs` | Lists background jobs in the current shell |
| `bg` / `fg` | Resumes a job in the background/foreground |
| `nice` / `renice` | Sets/changes the priority of a process |

---

## 8. Networking Commands

| Command | Purpose |
|---|---|
| `ping` | Tests network reachability using **ICMP echo request/reply** |
| `traceroute` | Shows the path (hop-by-hop) packets take to reach a destination |
| `ifconfig` | Displays/configures network interfaces (older, being replaced by `ip`) |
| `ip addr` | Modern replacement for `ifconfig` — shows IP addresses and interfaces |
| `netstat` | Displays network connections, routing tables, and listening ports |
| `ss` | Modern replacement for `netstat` — faster socket statistics |
| `ssh` | Securely logs into a remote machine (`ssh user@host`) |
| `scp` | Securely copies files between hosts over SSH (`scp file.txt user@host:/path`) |
| `wget` | Non-interactive downloader for files from the internet |
| `curl` | Transfers data to/from a server, supports many protocols (HTTP, FTP, etc.), often used to test APIs |
| `nmap` | **Network Mapper** — used for network scanning, port scanning, and security auditing |
| `tcpdump` | Captures and analyzes network packets at the command line |
| `dig` | Queries DNS servers — provides detailed DNS record information (more detailed than `nslookup`) |
| `nslookup` | Basic DNS lookup tool (older, less detailed than `dig`) |
| `host` | Simple DNS lookup utility |

---

## 9. Disk & Storage Management

| Command | Purpose |
|---|---|
| `df` | Displays disk space usage of file systems (`df -h` human-readable) |
| `du` | Displays disk usage of files/directories (`du -sh folder/` shows total size) |
| `mount` | Mounts a file system to a directory |
| `umount` | Unmounts a file system |
| `fdisk` | Partition table manipulator (`fdisk -l` lists partitions) |
| `lsblk` | Lists information about block devices (disks, partitions) |

---

## 10. Package Management

| Distro Family | Package Manager | Common Commands |
|---|---|---|
| Debian/Ubuntu | `apt` / `apt-get` | `apt install <pkg>`, `apt update`, `apt remove <pkg>` |
| RedHat/CentOS/Fedora | `yum` / `dnf` | `yum install <pkg>`, `dnf update` |
| Low-level (Debian) | `dpkg` | `dpkg -i package.deb` |
| Low-level (RedHat) | `rpm` | `rpm -ivh package.rpm` |

---

## 11. Compression & Archiving

| Command | Purpose |
|---|---|
| `tar` | Archives multiple files into one (`tar -cvf archive.tar files/`); extract with `tar -xvf archive.tar` |
| `tar -czvf` | Create a **compressed** (gzip) tar archive |
| `gzip` / `gunzip` | Compresses/decompresses a single file |
| `zip` / `unzip` | Creates/extracts `.zip` archives |

---

## 12. User & Permission Management

| Command | Purpose |
|---|---|
| `useradd` | Creates a new user account |
| `passwd` | Sets or changes a user's password |
| `su` | Switches to another user account |
| `sudo` | Executes a command with superuser (root) privileges |
| `whoami` | Displays the current logged-in username |
| `who` | Shows who is currently logged into the system |
| `w` | Shows logged-in users and what they're doing |

---

## 13. System Information & Monitoring

| Command | Purpose |
|---|---|
| `uname -a` | Displays system/kernel information |
| `uptime` | Shows how long the system has been running, plus load average |
| `free -h` | Displays free/used memory (RAM) in human-readable format |
| `vmstat` | Reports virtual memory, processes, and CPU statistics |
| `df -h` | Disk space (see Section 9) |
| `history` | Shows previously executed commands |

---

## 14. Advanced / System Administration

| Command | Purpose |
|---|---|
| `systemctl` | Controls the systemd system/service manager (`systemctl start nginx`, `systemctl status ssh`) |
| `journalctl` | Views logs collected by systemd's journal |
| `cron` / `crontab -e` | Schedules recurring tasks (jobs run automatically at specified times) |
| `strace` | Traces system calls made by a process — used for debugging |
| `lsof` | Lists open files and the processes using them (`lsof -i` shows network connections) |
| `env` | Displays or sets environment variables |
| `alias` | Creates a shortcut/nickname for a command (`alias ll='ls -la'`) |
| `export` | Sets an environment variable for the current shell session |

---

## 15. Windows Commands (CMD)

| Command | Purpose | Linux Equivalent |
|---|---|---|
| `dir` | Lists files/directories | `ls` |
| `cd` | Change directory | `cd` |
| `md` / `mkdir` | Make a new directory | `mkdir` |
| `rd` / `rmdir` | Remove a directory | `rmdir` |
| `copy` | Copy files | `cp` |
| `move` | Move/rename files | `mv` |
| `del` / `erase` | Delete files | `rm` |
| `ren` | Rename a file | `mv` (with same location) |
| `type` | Display file contents | `cat` |
| `cls` | Clear the screen | `clear` |
| `ipconfig` | Display IP configuration | `ifconfig` / `ip addr` |
| `ping` | Test network reachability | `ping` (same) |
| `tracert` | Trace route to a destination | `traceroute` |
| `netstat` | Display network connections | `netstat` (same) |
| `nslookup` | DNS lookup | `nslookup` (same) |
| `tasklist` | Lists running processes | `ps` |
| `taskkill` | Terminates a process (`taskkill /PID 1234 /F`) | `kill` |
| `systeminfo` | Displays detailed system configuration | `uname -a` (partial) |
| `sfc /scannow` | Scans and repairs protected system files | N/A |
| `chkdsk` | Checks disk for errors | `fsck` |
| `attrib` | Displays/changes file attributes (read-only, hidden, etc.) | `chmod` (partial) |
| `whoami` | Displays current username | `whoami` (same) |
| `net user` | Manages user accounts | `useradd` / `passwd` |
| `shutdown` | Shuts down/restarts the system (`shutdown /s /t 0`) | `shutdown` |

---

## 16. Windows PowerShell (Advanced — Modern Windows Admin)

| Command (Cmdlet) | Purpose | Legacy CMD Equivalent |
|---|---|---|
| `Get-ChildItem` (alias `ls`, `dir`) | Lists files/directories | `dir` |
| `Get-Location` (alias `pwd`) | Shows current directory | `cd` (no args) |
| `Set-Location` (alias `cd`) | Changes directory | `cd` |
| `Copy-Item` | Copies files | `copy` |
| `Move-Item` | Moves/renames files | `move` |
| `Remove-Item` | Deletes files/directories | `del` / `rd` |
| `Get-Process` | Lists running processes | `tasklist` |
| `Stop-Process` | Terminates a process | `taskkill` |
| `Get-Service` | Lists system services | `sc query` |
| `Start-Service` / `Stop-Service` | Starts/stops a service | `net start` / `net stop` |
| `Get-Content` (alias `cat`) | Displays file content | `type` |
| `Select-String` | Searches text (like `grep`) | N/A |
| `Test-Connection` | Pings a host (PowerShell version of ping) | `ping` |
| `Get-NetIPConfiguration` | Shows network configuration | `ipconfig` |
| `Invoke-WebRequest` | Downloads web content/files | `wget`/`curl` equivalent |
| `Get-EventLog` | Views Windows event logs | Event Viewer GUI equivalent |

---

## 17. Cross-Platform Quick Reference Table (High-Yield for Exams)

| Task | Linux/Unix | Windows CMD | PowerShell |
|---|---|---|---|
| List files | `ls` | `dir` | `Get-ChildItem` |
| Current directory | `pwd` | `cd` | `Get-Location` |
| Change directory | `cd` | `cd` | `Set-Location` |
| Copy file | `cp` | `copy` | `Copy-Item` |
| Move/rename | `mv` | `move` / `ren` | `Move-Item` |
| Delete file | `rm` | `del` | `Remove-Item` |
| View file content | `cat` | `type` | `Get-Content` |
| Clear screen | `clear` | `cls` | `Clear-Host` |
| Network config | `ifconfig`/`ip addr` | `ipconfig` | `Get-NetIPConfiguration` |
| Test connectivity | `ping` | `ping` | `Test-Connection` |
| Trace route | `traceroute` | `tracert` | `Test-NetConnection -TraceRoute` |
| List processes | `ps` | `tasklist` | `Get-Process` |
| Kill process | `kill` | `taskkill` | `Stop-Process` |
| Search text in file | `grep` | `findstr` | `Select-String` |
| Disk usage | `df -h` | N/A (use GUI) | `Get-PSDrive` |
| Current user | `whoami` | `whoami` | `whoami` |

---

## 18. High-Yield One-Liners for Exam

- `rm -rf` is one of the most dangerous commands — `-r` (recursive) + `-f` (force, no confirmation) deletes a directory and everything inside it **permanently**, with no recycle bin.
- `chmod 755` = owner: read+write+execute (7), group: read+execute (5), others: read+execute (5).
- `find` searches **live** on the file system; `locate` searches a **pre-built database** — much faster but can be outdated.
- `tail -f` is the standard way to **live-monitor** a growing log file in real time.
- `dig` gives more detailed DNS information than `nslookup` and is the modern preferred tool.
- `nmap` = **security/network auditing tool** for port scanning and host discovery — frequently confused with `tcpdump` (packet capture) in exams.
- In Windows, `ipconfig` is the direct equivalent of Linux's `ifconfig`/`ip addr`; `tracert` = `traceroute`.
- PowerShell cmdlets follow a **Verb-Noun** naming convention (e.g., `Get-Process`, `Stop-Service`) — this pattern itself is often tested.
- `grep -r` recursively searches directories; `grep -i` is case-insensitive; combined as `grep -ri` for both.
- `su` switches user (asks for target user's password); `sudo` runs a single command with elevated privileges (asks for **current** user's password).

---

*Notes compiled for IBPS SO / SBI SO IT Officer — Operating Systems / Linux command-line preparation.*
