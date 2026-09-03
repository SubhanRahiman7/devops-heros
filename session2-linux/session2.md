# Linux Homework Tasks

## Task 1: Soft Link & Hard Link

### Difference between soft link and hard link

- Soft link (symbolic link):
  - Created using `ln -s`.
  - It points to the file path, not the file itself.
  - If the original file is deleted or moved, the soft link breaks.
  - It can point to a file on another filesystem.

- Hard link:
  - Created using `ln`.
  - It points to the same inode as the original file.
  - If the original file is deleted, the hard link still works as long as at least one hard link remains.
  - It usually cannot be created across different filesystems.

### Commands to create both

```bash
# create a file
echo "hello" > original.txt

# create a soft link
ln -s original.txt soft_link.txt

# create a hard link
ln original.txt hard_link.txt
```

### Practice creating and deleting

```bash
ls -l
ls -li

# delete the original file
rm original.txt

# check what happens
cat hard_link.txt
cat soft_link.txt
```

### Interview answer

A soft link is like a shortcut to another file, while a hard link is another name for the same file data. Soft links break when the source file is removed, but hard links continue to work because they share the same inode.

---

## Task 2: adduser vs useradd

### Difference

- `adduser`:
  - User-friendly interactive command.
  - Common on Ubuntu/Debian.
  - Creates home directory and sets defaults automatically.

- `useradd`:
  - Low-level command.
  - More manual and flexible.
  - Common on many Linux systems.

### Preferred command on Ubuntu/Linux

On Ubuntu, `adduser` is preferred because it is simpler, interactive, and creates user accounts with better default settings.

### Example: create a test user using the recommended command

```bash
sudo adduser devops_test_user
```

After running, fill in the prompts for password and user details if needed.

To check the user:

```bash
id devops_test_user
```

---

## Task 3: journalctl

### What it is used for

`journalctl` is used to view logs from `systemd` and services. It helps administrators check system events, service logs, kernel messages, and boot logs.

### View system and service logs

```bash
# view all logs
journalctl

# view logs from the current boot
journalctl -b

# follow logs live
journalctl -f

# view logs for a specific service
journalctl -u ssh.service

# view the last 50 lines for a service
journalctl -u ssh.service -n 50 --no-pager
```

### Example service log check

```bash
sudo journalctl -u nginx.service
```

This shows the logs of the Nginx service.

---

## Task 4: Linux Command Cheat Sheet

### Important commands and their purpose

```bash
pwd              # print working directory
ls               # list files
ls -l            # list files with details
ls -a            # show hidden files
cd               # change directory
mkdir dir_name   # create a directory
touch file.txt   # create a file
cp src dest      # copy files
mv src dest      # move or rename files
rm file.txt      # remove a file
rm -r dir_name   # remove a directory
cat file.txt     # view file contents
head file.txt    # first lines of a file
tail file.txt    # last lines of a file
grep "text" file.txt   # search text in a file
find . -name "*.txt"  # find files
df -h            # check disk space
du -sh dir       # check directory size
ps aux           # view running processes
top              # monitor processes
whoami           # current user
hostname         # machine name
uname -a         # system information
chmod +x script.sh   # make script executable
chown user:user file # change ownership
sudo             # run command with admin privileges
```

