# 50 Beginner-Friendly Bash Shell Script Examples for System Administrators

This guide contains **50 basic Bash shell scripting examples** designed for beginners and junior Linux/System Administrators.

Topics covered:

- Command-line parameters: `$0`, `$1`, `$2`, `$#`, `$@`, `$*`
- Exit status: `$?`
- Process IDs: `$$`, `$!`
- `if`, `if-else`, and `elif`
- `for` loops
- `while` loops
- `until` loops
- `case` statements
- `select` keyboard menus
- Functions
- Files, directories, users, services, disks, memory, and logs
- Basic system administration automation

> **Important:** Some system administration scripts require `sudo` or root access.

---

## How to Use These Examples

Create a file:

```bash
vi script.sh
```

Add the script content, save it, and make it executable:

```bash
chmod +x script.sh
```

Run it:

```bash
./script.sh
```

You can also run a Bash script without executable permission:

```bash
bash script.sh
```

---

# Part 1 — Parameters and Special Variables

## 1. Display Script Name and Parameters

**Purpose:** Learn `$0`, `$1`, `$2`, and `$3`.

```bash
#!/bin/bash

echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Third argument: $3"
```

Run:

```bash
./parameters.sh Linux Docker Kubernetes
```

Output:

```text
Script name: ./parameters.sh
First argument: Linux
Second argument: Docker
Third argument: Kubernetes
```

---

## 2. Count the Number of Arguments

**Purpose:** `$#` gives the total number of command-line arguments.

```bash
#!/bin/bash

echo "Number of arguments: $#"
```

Run:

```bash
./count-args.sh one two three four
```

Output:

```text
Number of arguments: 4
```

---

## 3. Display All Arguments Using `$@`

**Purpose:** `"$@"` treats every parameter as a separate value.

```bash
#!/bin/bash

echo "All arguments:"

for argument in "$@"
do
    echo "$argument"
done
```

Run:

```bash
./all-args.sh Linux "Red Hat" Ubuntu
```

Output:

```text
All arguments:
Linux
Red Hat
Ubuntu
```

---

## 4. Difference Between `$*` and `$@`

```bash
#!/bin/bash

echo 'Using "$*":'

for argument in "$*"
do
    echo "$argument"
done

echo 'Using "$@":'

for argument in "$@"
do
    echo "$argument"
done
```

Run:

```bash
./star-vs-at.sh Linux Docker Kubernetes
```

Output:

```text
Using "$*":
Linux Docker Kubernetes

Using "$@":
Linux
Docker
Kubernetes
```

**Interview point:** Prefer `"$@"` when processing arguments.

---

## 5. Validate the Number of Arguments

```bash
#!/bin/bash

if [ "$#" -ne 2 ]
then
    echo "Usage: $0 <username> <department>"
    exit 1
fi

echo "Username: $1"
echo "Department: $2"
```

Run correctly:

```bash
./validate.sh sreejith devops
```

Output:

```text
Username: sreejith
Department: devops
```

Run incorrectly:

```bash
./validate.sh sreejith
```

Output:

```text
Usage: ./validate.sh <username> <department>
```

---

## 6. Check the Exit Status Using `$?`

```bash
#!/bin/bash

ls /etc/passwd

echo "Exit status: $?"
```

Output:

```text
/etc/passwd
Exit status: 0
```

A successful command normally returns `0`. A failed command returns a non-zero value.

---

## 7. Check Whether a Command Succeeded

```bash
#!/bin/bash

ping -c 1 8.8.8.8 > /dev/null 2>&1

if [ "$?" -eq 0 ]
then
    echo "Network is reachable."
else
    echo "Network is not reachable."
fi
```

---

## 8. Display the Current Script Process ID

**Purpose:** `$$` contains the PID of the current script.

```bash
#!/bin/bash

echo "Current script PID: $$"
```

Example output:

```text
Current script PID: 24510
```

---

## 9. Display the Last Background Process ID

**Purpose:** `$!` contains the PID of the most recently started background process.

```bash
#!/bin/bash

sleep 30 &

echo "Background process PID: $!"
```

---

## 10. Use `shift` to Process Parameters

`shift` removes the first argument and moves the remaining arguments left.

```bash
#!/bin/bash

while [ "$#" -gt 0 ]
do
    echo "Current argument: $1"
    shift
done
```

Run:

```bash
./shift-example.sh server1 server2 server3
```

Output:

```text
Current argument: server1
Current argument: server2
Current argument: server3
```

---

# Part 2 — Conditions

## 11. Check Whether a File Exists

```bash
#!/bin/bash

file_name="$1"

if [ -f "$file_name" ]
then
    echo "$file_name exists and is a regular file."
else
    echo "$file_name does not exist."
fi
```

Run:

```bash
./check-file.sh /etc/passwd
```

---

## 12. Check Whether a Directory Exists

```bash
#!/bin/bash

directory="$1"

if [ -d "$directory" ]
then
    echo "$directory exists."
else
    echo "$directory does not exist."
fi
```

Run:

```bash
./check-directory.sh /var/log
```

---

## 13. Create a Directory Only If It Does Not Exist

```bash
#!/bin/bash

directory="$1"

if [ -z "$directory" ]
then
    echo "Usage: $0 <directory>"
    exit 1
fi

if [ -d "$directory" ]
then
    echo "Directory already exists: $directory"
else
    mkdir -p "$directory"

    if [ "$?" -eq 0 ]
    then
        echo "Directory created: $directory"
    else
        echo "Failed to create directory."
        exit 1
    fi
fi
```

---

## 14. Compare Two Numbers

```bash
#!/bin/bash

number1="$1"
number2="$2"

if [ "$number1" -gt "$number2" ]
then
    echo "$number1 is greater than $number2"
elif [ "$number1" -lt "$number2" ]
then
    echo "$number1 is less than $number2"
else
    echo "Both numbers are equal."
fi
```

---

## 15. Check Whether a Number Is Even or Odd

```bash
#!/bin/bash

number="$1"

if [ $((number % 2)) -eq 0 ]
then
    echo "$number is even."
else
    echo "$number is odd."
fi
```

Run:

```bash
./even-odd.sh 15
```

Output:

```text
15 is odd.
```

---

## 16. Check Whether a String Is Empty

```bash
#!/bin/bash

read -r -p "Enter your name: " name

if [ -z "$name" ]
then
    echo "You did not enter a name."
else
    echo "Hello, $name"
fi
```

---

## 17. Compare Two Strings

```bash
#!/bin/bash

read -r -p "Enter environment: " environment

if [ "$environment" = "production" ]
then
    echo "Production environment selected."
else
    echo "Non-production environment selected."
fi
```

---

## 18. Check Whether the Current User Is Root

```bash
#!/bin/bash

if [ "$EUID" -eq 0 ]
then
    echo "You are running as root."
else
    echo "Run this script using sudo."
    exit 1
fi
```

---

## 19. Check Whether a User Exists

```bash
#!/bin/bash

username="$1"

if id "$username" > /dev/null 2>&1
then
    echo "User exists: $username"
else
    echo "User does not exist: $username"
fi
```

Run:

```bash
./check-user.sh root
```

---

## 20. Check Whether a Service Is Active

```bash
#!/bin/bash

service_name="$1"

if systemctl is-active --quiet "$service_name"
then
    echo "$service_name is running."
else
    echo "$service_name is not running."
fi
```

Run:

```bash
./check-service.sh ssh
```

> On RHEL-based systems, the SSH service is usually named `sshd`.

---

# Part 3 — `for` Loops

## 21. Basic `for` Loop

```bash
#!/bin/bash

for item in Linux Docker Kubernetes Ansible
do
    echo "Technology: $item"
done
```

---

## 22. Loop Through Numbers

```bash
#!/bin/bash

for number in {1..10}
do
    echo "Number: $number"
done
```

---

## 23. C-Style `for` Loop

```bash
#!/bin/bash

for ((number=1; number<=5; number++))
do
    echo "Iteration: $number"
done
```

---

## 24. Loop Through Files in a Directory

```bash
#!/bin/bash

directory="/var/log"

for file in "$directory"/*
do
    echo "Found: $file"
done
```

---

## 25. Display the Size of Every Log File

```bash
#!/bin/bash

for file in /var/log/*.log
do
    if [ -f "$file" ]
    then
        size=$(du -h "$file" | awk '{print $1}')
        echo "$file : $size"
    fi
done
```

---

## 26. Ping Multiple Servers

```bash
#!/bin/bash

servers=("8.8.8.8" "1.1.1.1" "192.168.0.1")

for server in "${servers[@]}"
do
    if ping -c 1 -W 2 "$server" > /dev/null 2>&1
    then
        echo "$server is reachable."
    else
        echo "$server is unreachable."
    fi
done
```

---

## 27. Read Server Names from a File

Create `servers.txt`:

```text
server1
server2
server3
```

Script:

```bash
#!/bin/bash

while IFS= read -r server
do
    echo "Processing server: $server"
done < servers.txt
```

---

## 28. Create Multiple Directories

```bash
#!/bin/bash

for directory in backup logs reports scripts
do
    mkdir -p "$directory"
    echo "Created directory: $directory"
done
```

---

## 29. Create Multiple Users from Parameters

```bash
#!/bin/bash

if [ "$#" -eq 0 ]
then
    echo "Usage: $0 <user1> <user2> ..."
    exit 1
fi

for username in "$@"
do
    if id "$username" > /dev/null 2>&1
    then
        echo "User already exists: $username"
    else
        sudo useradd "$username"

        if [ "$?" -eq 0 ]
        then
            echo "Created user: $username"
        else
            echo "Failed to create user: $username"
        fi
    fi
done
```

> Test user-management scripts only in a lab system.

---

## 30. Find Large Files

```bash
#!/bin/bash

directory="${1:-/var/log}"
size="${2:-100M}"

echo "Files larger than $size in $directory:"

find "$directory" -type f -size +"$size" -exec ls -lh {} \; 2>/dev/null
```

Run:

```bash
./large-files.sh /var 50M
```

---

# Part 4 — `while` and `until` Loops

## 31. Basic `while` Loop

```bash
#!/bin/bash

counter=1

while [ "$counter" -le 5 ]
do
    echo "Counter: $counter"
    counter=$((counter + 1))
done
```

---

## 32. Countdown Using `while`

```bash
#!/bin/bash

counter=5

while [ "$counter" -gt 0 ]
do
    echo "$counter"
    sleep 1
    counter=$((counter - 1))
done

echo "Completed."
```

---

## 33. Read User Input Until `exit`

```bash
#!/bin/bash

while true
do
    read -r -p "Enter a command or type exit: " command

    if [ "$command" = "exit" ]
    then
        echo "Exiting."
        break
    fi

    echo "You entered: $command"
done
```

---

## 34. Monitor Disk Usage Continuously

```bash
#!/bin/bash

while true
do
    clear
    echo "Disk usage at $(date)"
    df -h
    sleep 10
done
```

Stop it using:

```text
Ctrl+C
```

---

## 35. Wait Until a Service Starts

```bash
#!/bin/bash

service_name="$1"

if [ -z "$service_name" ]
then
    echo "Usage: $0 <service-name>"
    exit 1
fi

until systemctl is-active --quiet "$service_name"
do
    echo "Waiting for $service_name to start..."
    sleep 5
done

echo "$service_name is now running."
```

---

## 36. Wait Until a Host Becomes Reachable

```bash
#!/bin/bash

host="$1"

if [ -z "$host" ]
then
    echo "Usage: $0 <hostname-or-ip>"
    exit 1
fi

until ping -c 1 -W 2 "$host" > /dev/null 2>&1
do
    echo "Waiting for $host..."
    sleep 5
done

echo "$host is reachable."
```

---

## 37. Retry a Command Three Times

```bash
#!/bin/bash

attempt=1
maximum_attempts=3

while [ "$attempt" -le "$maximum_attempts" ]
do
    echo "Attempt $attempt of $maximum_attempts"

    curl -fsS https://example.com > /dev/null

    if [ "$?" -eq 0 ]
    then
        echo "Command succeeded."
        exit 0
    fi

    attempt=$((attempt + 1))
    sleep 2
done

echo "Command failed after $maximum_attempts attempts."
exit 1
```

---

## 38. Read a File Line by Line

```bash
#!/bin/bash

file_name="$1"

if [ ! -f "$file_name" ]
then
    echo "File not found: $file_name"
    exit 1
fi

while IFS= read -r line
do
    echo "$line"
done < "$file_name"
```

---

# Part 5 — `case` and Keyboard Menus

## 39. Basic `case` Statement

```bash
#!/bin/bash

read -r -p "Enter start, stop, restart, or status: " action

case "$action" in
    start)
        echo "Starting service..."
        ;;
    stop)
        echo "Stopping service..."
        ;;
    restart)
        echo "Restarting service..."
        ;;
    status)
        echo "Checking service status..."
        ;;
    *)
        echo "Invalid option."
        ;;
esac
```

---

## 40. Service Management with `case`

```bash
#!/bin/bash

service_name="$1"
action="$2"

if [ "$#" -ne 2 ]
then
    echo "Usage: $0 <service-name> <start|stop|restart|status>"
    exit 1
fi

case "$action" in
    start)
        sudo systemctl start "$service_name"
        ;;
    stop)
        sudo systemctl stop "$service_name"
        ;;
    restart)
        sudo systemctl restart "$service_name"
        ;;
    status)
        systemctl status "$service_name"
        ;;
    *)
        echo "Invalid action: $action"
        exit 1
        ;;
esac
```

Run:

```bash
./service-control.sh ssh status
```

---

## 41. Check Operating System Using `case`

```bash
#!/bin/bash

if [ -f /etc/os-release ]
then
    . /etc/os-release
else
    echo "Cannot identify the operating system."
    exit 1
fi

case "$ID" in
    ubuntu|debian)
        echo "Debian-based system detected."
        ;;
    rhel|centos|rocky|almalinux|fedora)
        echo "Red Hat-based system detected."
        ;;
    *)
        echo "Other Linux distribution detected: $ID"
        ;;
esac
```

---

## 42. Simple Keyboard Menu Using `select`

```bash
#!/bin/bash

PS3="Select an option: "

select option in "Show date" "Show users" "Show disk usage" "Exit"
do
    case "$option" in
        "Show date")
            date
            ;;
        "Show users")
            who
            ;;
        "Show disk usage")
            df -h
            ;;
        "Exit")
            echo "Goodbye."
            break
            ;;
        *)
            echo "Invalid selection."
            ;;
    esac
done
```

The user selects an option by entering its number.

---

## 43. System Administration Menu

```bash
#!/bin/bash

while true
do
    echo
    echo "===== System Administration Menu ====="
    echo "1. Show hostname"
    echo "2. Show IP addresses"
    echo "3. Show disk usage"
    echo "4. Show memory usage"
    echo "5. Show logged-in users"
    echo "6. Exit"

    read -r -p "Enter your choice: " choice

    case "$choice" in
        1)
            hostname
            ;;
        2)
            hostname -I
            ;;
        3)
            df -h
            ;;
        4)
            free -h
            ;;
        5)
            who
            ;;
        6)
            echo "Exiting."
            break
            ;;
        *)
            echo "Invalid choice."
            ;;
    esac
done
```

---

# Part 6 — Functions and System Administration Scripts

## 44. Basic Function

```bash
#!/bin/bash

show_system_info() {
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
    echo "Current user: $(whoami)"
}

show_system_info
```

---

## 45. Function with Parameters

```bash
#!/bin/bash

check_file() {
    local file_name="$1"

    if [ -f "$file_name" ]
    then
        echo "File exists: $file_name"
        return 0
    else
        echo "File not found: $file_name"
        return 1
    fi
}

check_file "/etc/passwd"

echo "Function return status: $?"
```

---

## 46. Disk Usage Alert

```bash
#!/bin/bash

threshold="${1:-80}"

usage=$(df / | awk 'NR==2 {gsub("%", "", $5); print $5}')

echo "Root filesystem usage: ${usage}%"

if [ "$usage" -ge "$threshold" ]
then
    echo "WARNING: Disk usage is above ${threshold}%."
    exit 1
else
    echo "Disk usage is normal."
fi
```

Run:

```bash
./disk-alert.sh 75
```

---

## 47. Memory Usage Report

```bash
#!/bin/bash

total_memory=$(free -m | awk '/Mem:/ {print $2}')
used_memory=$(free -m | awk '/Mem:/ {print $3}')
free_memory=$(free -m | awk '/Mem:/ {print $4}')
usage_percent=$((used_memory * 100 / total_memory))

echo "Total memory: ${total_memory} MB"
echo "Used memory: ${used_memory} MB"
echo "Free memory: ${free_memory} MB"
echo "Memory usage: ${usage_percent}%"
```

---

## 48. Backup a Directory

```bash
#!/bin/bash

source_directory="$1"
backup_directory="$2"
timestamp=$(date +"%Y%m%d_%H%M%S")

if [ "$#" -ne 2 ]
then
    echo "Usage: $0 <source-directory> <backup-directory>"
    exit 1
fi

if [ ! -d "$source_directory" ]
then
    echo "Source directory does not exist: $source_directory"
    exit 1
fi

mkdir -p "$backup_directory"

archive_name="$backup_directory/backup_$timestamp.tar.gz"

tar -czf "$archive_name" "$source_directory"

if [ "$?" -eq 0 ]
then
    echo "Backup created successfully: $archive_name"
else
    echo "Backup failed."
    exit 1
fi
```

Run:

```bash
./backup.sh /etc /tmp/backups
```

---

## 49. Delete Old Log Files

```bash
#!/bin/bash

log_directory="${1:-/var/log/myapp}"
retention_days="${2:-30}"

if [ ! -d "$log_directory" ]
then
    echo "Directory not found: $log_directory"
    exit 1
fi

echo "Files older than $retention_days days:"

find "$log_directory" -type f -mtime +"$retention_days" -print

read -r -p "Delete these files? Type yes to continue: " confirmation

if [ "$confirmation" = "yes" ]
then
    find "$log_directory" -type f -mtime +"$retention_days" -delete
    echo "Old files deleted."
else
    echo "Deletion cancelled."
fi
```

> Review the `find` output carefully before deleting files.

---

## 50. Complete Beginner System Health Check

```bash
#!/bin/bash

output_file="/tmp/system_health_$(date +%Y%m%d_%H%M%S).txt"

{
    echo "======================================"
    echo "       SYSTEM HEALTH CHECK"
    echo "======================================"
    echo "Date: $(date)"
    echo "Hostname: $(hostname)"
    echo "Current user: $(whoami)"
    echo

    echo "----- Operating System -----"
    if [ -f /etc/os-release ]
    then
        grep -E '^(NAME|VERSION)=' /etc/os-release
    fi
    echo

    echo "----- Kernel -----"
    uname -r
    echo

    echo "----- Uptime -----"
    uptime
    echo

    echo "----- IP Addresses -----"
    hostname -I
    echo

    echo "----- Disk Usage -----"
    df -h
    echo

    echo "----- Memory Usage -----"
    free -h
    echo

    echo "----- Top CPU Processes -----"
    ps -eo pid,user,%cpu,%mem,comm --sort=-%cpu | head
    echo

    echo "----- Top Memory Processes -----"
    ps -eo pid,user,%cpu,%mem,comm --sort=-%mem | head
    echo

    echo "----- Logged-In Users -----"
    who
    echo

    echo "----- Failed Services -----"
    systemctl --failed --no-pager
    echo

    echo "Report completed."
} | tee "$output_file"

echo
echo "Report saved to: $output_file"
```

---

# Important Bash Test Operators

## File Test Operators

| Operator | Meaning |
|---|---|
| `-f file` | File exists and is a regular file |
| `-d directory` | Directory exists |
| `-e path` | File or directory exists |
| `-r file` | File is readable |
| `-w file` | File is writable |
| `-x file` | File is executable |
| `-s file` | File exists and is not empty |
| `! condition` | Negates a condition |

Example:

```bash
if [ -r /etc/passwd ]
then
    echo "The file is readable."
fi
```

---

## Numeric Comparison Operators

| Operator | Meaning |
|---|---|
| `-eq` | Equal |
| `-ne` | Not equal |
| `-gt` | Greater than |
| `-ge` | Greater than or equal |
| `-lt` | Less than |
| `-le` | Less than or equal |

Example:

```bash
if [ "$age" -ge 18 ]
then
    echo "Adult"
fi
```

---

## String Comparison Operators

| Operator | Meaning |
|---|---|
| `=` | Strings are equal |
| `!=` | Strings are not equal |
| `-z string` | String is empty |
| `-n string` | String is not empty |

---

# Important Special Variables

| Variable | Meaning |
|---|---|
| `$0` | Script name |
| `$1` | First argument |
| `$2` | Second argument |
| `${10}` | Tenth argument |
| `$#` | Number of arguments |
| `"$@"` | All arguments as separate values |
| `"$*"` | All arguments as one value |
| `$?` | Exit status of the previous command |
| `$$` | PID of the current shell/script |
| `$!` | PID of the last background process |

---

# Important Loop Control Commands

| Command | Purpose |
|---|---|
| `break` | Exit the loop |
| `continue` | Skip the current iteration |
| `exit 0` | End the script successfully |
| `exit 1` | End the script with an error |
| `shift` | Remove the first positional parameter |

---

# Basic Script Template for Beginners

```bash
#!/bin/bash

set -u

usage() {
    echo "Usage: $0 <argument>"
}

if [ "$#" -ne 1 ]
then
    usage
    exit 1
fi

value="$1"

echo "Processing: $value"

if [ -e "$value" ]
then
    echo "The path exists."
else
    echo "The path does not exist."
    exit 1
fi

exit 0
```

---

# Common Beginner Mistakes

## 1. Missing Spaces Around `[` and `]`

Incorrect:

```bash
if ["$name" = "admin"]
```

Correct:

```bash
if [ "$name" = "admin" ]
```

## 2. Not Quoting Variables

Risky:

```bash
rm $file
```

Better:

```bash
rm -- "$file"
```

## 3. Using `$10` Instead of `${10}`

Incorrect:

```bash
echo "$10"
```

This may be interpreted as `${1}0`.

Correct:

```bash
echo "${10}"
```

## 4. Checking `$?` Too Late

Incorrect:

```bash
cp source destination
echo "Copy completed"
if [ "$?" -eq 0 ]
```

`$?` now contains the status of `echo`, not `cp`.

Better:

```bash
if cp source destination
then
    echo "Copy completed."
else
    echo "Copy failed."
fi
```

## 5. Running Destructive Commands Without Validation

Always validate variables before using commands such as:

```bash
rm -rf
find -delete
userdel
shutdown
reboot
```

---

# Practice Tasks

After completing the examples, try writing scripts that:

1. Check whether three services are running.
2. Display files larger than 500 MB.
3. Alert when memory usage exceeds 80%.
4. Create five users from a text file.
5. Back up `/etc` and keep only seven backups.
6. Display listening TCP ports.
7. Check whether a website returns HTTP status `200`.
8. Restart a service when it is inactive.
9. Display the ten largest directories under `/var`.
10. Build a keyboard-based user and service management menu.

---

# Final Interview Summary

A Bash shell script is a file containing Linux commands that are executed sequentially by a shell such as Bash. Shell scripts help system administrators automate repetitive work such as checking services, monitoring disk and memory usage, managing users, reading logs, taking backups, and validating servers. Command-line arguments are accessed using variables such as `$1`, `$2`, `$#`, and `"$@"`. Conditions such as `if` and `case` control decisions, while `for`, `while`, and `until` repeat commands. A successful Linux command normally returns exit status `0`, while a non-zero status indicates an error.
