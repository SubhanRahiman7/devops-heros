# Session 2 — Linux Answers

## Task 1: Soft Link & Hard Link

### Summary (interview-ready)
- Soft link (symbolic link): a special file that points to a pathname. Created with `ln -s <target> <linkname>`.
  - Cross-filesystem capable.
  - Can point to directories.
  - If target is removed or moved, the symlink becomes dangling (broken).
  - `ls -l` shows the arrow and target: `lrwxrwxrwx  1 user group ... link -> target`

- Hard link: another directory entry pointing to the same inode as the target. Created with `ln <target> <linkname>`.
  - Cannot cross filesystems.
  - Usually cannot create hard links to directories (to prevent loops), except for special admin actions.
  - Deleting the original filename does not remove the data as long as at least one hard link (directory entry) exists.
  - `ls -li` shows same inode number for hard-linked files.

### Commands and practice
```bash
# prepare
mkdir -p ~/link-practice && cd ~/link-practice
echo "hello links" > original.txt

# create a soft link
ln -s original.txt soft_link.txt

# create a hard link
ln original.txt hard_link.txt

ls -li
# try reading through either link
cat soft_link.txt
cat hard_link.txt

# delete original and observe
rm original.txt
ls -li
# hard_link.txt still contains data; soft_link.txt is broken
cat hard_link.txt
cat soft_link.txt  # will fail if broken
```

Notes for interview: explain inode concept briefly — hard links reference same inode, symlinks reference a path. Mention differences (cross-filesystem, directories, broken links) and typical commands (`ln` vs `ln -s`).

---

## Task 2: adduser vs useradd

### Differences
- `useradd`:
  - Low-level tool (binary) for adding users.
  - Non-interactive by default; needs options (`-m` to create home, `-s` to set shell, etc.).
  - Present on most distributions.

- `adduser` (Debian/Ubuntu wrapper):
  - Higher-level friendly script that wraps `useradd` (on Debian-based systems).
  - Interactive prompts for password, full name, and creates home directory by default.
  - Uses sane defaults and creates user skeleton files.

### Which to use on Ubuntu/Linux and why
- On Ubuntu/Debian, prefer `adduser` for interactive user creation because it is safer and more user-friendly.
- On other distributions (RHEL/CentOS/Fedora) `adduser` may not exist or is just a symlink; use `useradd` with flags.

### Example (recommended on Ubuntu)
```bash
sudo adduser devops_test_user
# follow prompts to set password and details
id devops_test_user
# cleanup
sudo deluser --remove-home devops_test_user
```

---

## Task 3: journalctl

### What is journalctl used for?
- `journalctl` is the tool to query and view logs collected by `systemd-journald`.
- It centralizes kernel logs, system logs, and service logs managed by systemd.

### Useful commands
```bash
# show entire journal (paged)
journalctl

# show logs from current boot only
journalctl -b

# follow (like tail -f)
journalctl -f

# show logs for a specific service
journalctl -u ssh.service

# show last N lines for a service
journalctl -u ssh.service -n 100 --no-pager

# show kernel messages only
journalctl -k

# show logs between two dates
journalctl --since "2026-09-03" --until "2026-09-04 00:00"

# show logs with priority (e.g., err and above)
journalctl -p err
```

### Practice
- Try `sudo journalctl -u systemd-resolved` (or any installed service) and use `-f` to follow while restarting the service in another terminal to observe logs in real time.

---

## Task 4: Linux Command Cheat Sheet (important commands & purpose)

Below are concise commands you should practice and be able to explain briefly.

- `pwd` — print working directory
- `ls -lah` — list files with human-readable sizes and hidden files
- `cd <dir>` — change directory
- `mkdir -p <dir>` — create directory and parents
- `touch <file>` — create empty file or update timestamp
- `cp <src> <dst>` — copy file
- `mv <src> <dst>` — move/rename file
- `rm <file>` / `rm -rf <dir>` — remove file/directory
- `cat <file>` — show file content
- `less <file>` — view file paged
- `head` / `tail -n 100` — show start or end of files
- `grep -i "pattern" <file>` — search text in files
- `find . -name "*.sh"` — find files by name
- `chmod +x script.sh` — make script executable
- `chown user:group file` — change owner and group
- `df -h` — disk usage per filesystem (human readable)
- `du -sh <dir>` — disk usage of a directory (summary)
- `free -h` — memory usage
- `uname -a` — kernel and system information
- `ps aux` — list processes
- `ps -ef` — another full-format process listing
- `top` / `htop` — interactive process viewer
- `systemctl status <service>` — check systemd service status
- `journalctl -u <service>` — view service logs (systemd)
- `ssh user@host` — connect to remote host
- `scp` / `rsync` — file copy between hosts
- `iptables` / `ufw` / `firewalld` — basic firewall management (distro dependent)

---

## Quick practice tasks (one-liners)
```bash
# find large files
sudo find / -type f -size +100M -exec ls -lh {} \; | awk '{ print $NF, $5 }'

# check listening ports
ss -tulnp

# disk summary of current directory
du -sh .
```

---

If you want these answers split into smaller files or expanded with screenshots/command outputs, tell me which format and I will update accordingly.
