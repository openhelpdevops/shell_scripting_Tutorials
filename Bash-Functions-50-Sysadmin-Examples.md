
# Bash Functions for System Administrators — 50 Practical Script Examples

This guide contains **50 Bash scripts using functions** for common Linux system administration tasks.

The examples progress from beginner-friendly function syntax to practical monitoring, user management, service checks, networking, backups, logs, and automation.

> Test scripts in a lab before using them in production.

---

## Basic Function Syntax

```bash
function_name() {
    commands
}
```

Call the function:

```bash
function_name
```

Function with parameters:

```bash
greet_user() {
    local username="$1"
    echo "Hello, $username"
}

greet_user "sreejith"
```

Important function variables:

| Variable | Meaning |
|---|---|
| `$1` | First function argument |
| `$2` | Second function argument |
| `$#` | Number of arguments |
| `$@` | All arguments |
| `$?` | Exit status of the previous command |
| `local` | Creates a variable only inside the function |

---

## 1. Display System Information

Displays basic hostname, kernel, and operating system information.

```bash
#!/bin/bash

show_system_info() {
    echo "Hostname: $(hostname)"
    echo "Kernel: $(uname -r)"
    echo "Operating System:"
    cat /etc/os-release | grep PRETTY_NAME
}

show_system_info
```

---

## 2. Check Disk Usage

Runs `df -h` inside a reusable function.

```bash
#!/bin/bash

check_disk_usage() {
    echo "Current disk usage:"
    df -h
}

check_disk_usage
```

---

## 3. Check Root Filesystem Usage

Reads only the utilization of the root filesystem.

```bash
#!/bin/bash

check_root_disk() {
    local usage
    usage=$(df -P / | awk 'NR==2 {print $5}')
    echo "Root filesystem usage: $usage"
}

check_root_disk
```

---

## 4. Alert When Disk Usage Exceeds Threshold

Accepts a threshold as a function parameter and compares disk usage.

```bash
#!/bin/bash

check_disk_threshold() {
    local threshold="$1"
    local usage

    usage=$(df -P / | awk 'NR==2 {gsub("%","",$5); print $5}')

    if (( usage >= threshold )); then
        echo "ALERT: Root disk usage is ${usage}%"
    else
        echo "Disk usage is normal: ${usage}%"
    fi
}

check_disk_threshold 80
```

---

## 5. Check Memory Usage

Displays current memory and swap usage.

```bash
#!/bin/bash

check_memory() {
    free -h
}

check_memory
```

---

## 6. Calculate Memory Percentage

Calculates used memory as a percentage.

```bash
#!/bin/bash

memory_percentage() {
    local total used percent

    total=$(free -m | awk '/Mem:/ {print $2}')
    used=$(free -m | awk '/Mem:/ {print $3}')
    percent=$(( used * 100 / total ))

    echo "Memory usage: ${percent}%"
}

memory_percentage
```

---

## 7. Check CPU Load

Shows the system load averages using `uptime`.

```bash
#!/bin/bash

check_load() {
    echo "Load average:"
    uptime
}

check_load
```

---

## 8. Display Top CPU Processes

Lists processes consuming the most CPU.

```bash
#!/bin/bash

top_cpu_processes() {
    ps -eo pid,user,comm,%cpu --sort=-%cpu | head
}

top_cpu_processes
```

---

## 9. Display Top Memory Processes

Lists processes consuming the most memory.

```bash
#!/bin/bash

top_memory_processes() {
    ps -eo pid,user,comm,%mem --sort=-%mem | head
}

top_memory_processes
```

---

## 10. Check Service Status

Checks whether a systemd service is active.

```bash
#!/bin/bash

check_service() {
    local service="$1"

    if systemctl is-active --quiet "$service"; then
        echo "$service is running"
    else
        echo "$service is not running"
    fi
}

check_service sshd
```

---

## 11. Restart a Service

Restarts a service and verifies whether it became active.

```bash
#!/bin/bash

restart_service() {
    local service="$1"

    sudo systemctl restart "$service"

    if systemctl is-active --quiet "$service"; then
        echo "$service restarted successfully"
    else
        echo "Failed to restart $service"
    fi
}

restart_service nginx
```

---

## 12. Start a Service

Starts a service and displays its status.

```bash
#!/bin/bash

start_service() {
    local service="$1"
    sudo systemctl start "$service"
    systemctl status "$service" --no-pager
}

start_service httpd
```

---

## 13. Stop a Service

Stops a specified systemd service.

```bash
#!/bin/bash

stop_service() {
    local service="$1"
    sudo systemctl stop "$service"
    echo "$service has been stopped"
}

stop_service httpd
```

---

## 14. Enable a Service at Boot

Enables a service so that it starts automatically after reboot.

```bash
#!/bin/bash

enable_service() {
    local service="$1"
    sudo systemctl enable "$service"
    echo "$service enabled at boot"
}

enable_service nginx
```

---

## 15. Check Multiple Services

Uses an array and loop to check several services.

```bash
#!/bin/bash

check_services() {
    local services=("sshd" "cron" "nginx")

    for service in "${services[@]}"; do
        if systemctl is-active --quiet "$service"; then
            echo "$service: running"
        else
            echo "$service: stopped"
        fi
    done
}

check_services
```

---

## 16. Check Whether a Process Exists

Uses `pgrep` to determine whether a process is running.

```bash
#!/bin/bash

check_process() {
    local process="$1"

    if pgrep -x "$process" >/dev/null; then
        echo "$process is running"
    else
        echo "$process is not running"
    fi
}

check_process sshd
```

---

## 17. Kill a Process by Name

Stops processes matching an exact process name.

```bash
#!/bin/bash

kill_process() {
    local process="$1"

    if pgrep -x "$process" >/dev/null; then
        sudo pkill -x "$process"
        echo "$process stopped"
    else
        echo "$process is not running"
    fi
}

kill_process testapp
```

---

## 18. Create a User

Creates a user only when the account does not already exist.

```bash
#!/bin/bash

create_user() {
    local username="$1"

    if id "$username" >/dev/null 2>&1; then
        echo "User $username already exists"
    else
        sudo useradd -m "$username"
        echo "User $username created"
    fi
}

create_user devuser
```

---

## 19. Delete a User

Deletes a user and the user's home directory.

```bash
#!/bin/bash

delete_user() {
    local username="$1"

    if id "$username" >/dev/null 2>&1; then
        sudo userdel -r "$username"
        echo "User $username deleted"
    else
        echo "User $username does not exist"
    fi
}

delete_user testuser
```

---

## 20. Lock a User Account

Locks a local Linux user account.

```bash
#!/bin/bash

lock_user() {
    local username="$1"
    sudo usermod -L "$username"
    echo "User $username locked"
}

lock_user devuser
```

---

## 21. Unlock a User Account

Unlocks a previously locked account.

```bash
#!/bin/bash

unlock_user() {
    local username="$1"
    sudo usermod -U "$username"
    echo "User $username unlocked"
}

unlock_user devuser
```

---

## 22. Check User Existence

Checks local or directory-backed users using `getent`.

```bash
#!/bin/bash

check_user() {
    local username="$1"

    if getent passwd "$username" >/dev/null; then
        echo "User exists: $username"
    else
        echo "User not found: $username"
    fi
}

check_user root
```

---

## 23. List Logged-In Users

Lists currently logged-in users.

```bash
#!/bin/bash

logged_in_users() {
    echo "Currently logged-in users:"
    who
}

logged_in_users
```

---

## 24. Check Failed SSH Logins

Searches the SSH service journal for failed passwords.

```bash
#!/bin/bash

failed_ssh_logins() {
    echo "Recent failed SSH logins:"
    journalctl -u sshd --since today |
        grep "Failed password" |
        tail -20
}

failed_ssh_logins
```

---

## 25. Check Listening Ports

Displays TCP and UDP listening sockets and owning processes.

```bash
#!/bin/bash

list_ports() {
    sudo ss -tulnp
}

list_ports
```

---

## 26. Check a Specific Port

Checks whether a TCP port is listening locally.

```bash
#!/bin/bash

check_port() {
    local port="$1"

    if ss -lnt | awk '{print $4}' | grep -q ":${port}$"; then
        echo "Port $port is listening"
    else
        echo "Port $port is not listening"
    fi
}

check_port 22
```

---

## 27. Ping a Server

Tests basic network reachability.

```bash
#!/bin/bash

check_host() {
    local host="$1"

    if ping -c 2 "$host" >/dev/null 2>&1; then
        echo "$host is reachable"
    else
        echo "$host is unreachable"
    fi
}

check_host 8.8.8.8
```

---

## 28. Check Multiple Servers

Loops through several server names and checks connectivity.

```bash
#!/bin/bash

check_servers() {
    local servers=("server1" "server2" "server3")

    for server in "${servers[@]}"; do
        if ping -c 1 -W 2 "$server" >/dev/null 2>&1; then
            echo "$server: reachable"
        else
            echo "$server: unreachable"
        fi
    done
}

check_servers
```

---

## 29. Check DNS Resolution

Uses the system resolver to verify DNS resolution.

```bash
#!/bin/bash

check_dns() {
    local hostname="$1"

    if getent hosts "$hostname" >/dev/null; then
        getent hosts "$hostname"
    else
        echo "DNS resolution failed for $hostname"
    fi
}

check_dns google.com
```

---

## 30. Display IP Addresses

Displays network interfaces and IP addresses in brief format.

```bash
#!/bin/bash

show_ip_addresses() {
    ip -brief address
}

show_ip_addresses
```

---

## 31. Check Internet Connectivity

Uses HTTP instead of ICMP to test outbound connectivity.

```bash
#!/bin/bash

check_internet() {
    if curl -Is --max-time 5 https://example.com >/dev/null; then
        echo "Internet connection is available"
    else
        echo "Internet connection failed"
    fi
}

check_internet
```

---

## 32. Check Website HTTP Status

Retrieves the HTTP status code of a website.

```bash
#!/bin/bash

check_website() {
    local url="$1"
    local status

    status=$(curl -s -o /dev/null -w "%{http_code}" "$url")
    echo "$url returned HTTP status $status"
}

check_website https://example.com
```

---

## 33. Create a Directory

Creates a directory safely with `mkdir -p`.

```bash
#!/bin/bash

create_directory() {
    local directory="$1"

    if [[ -d "$directory" ]]; then
        echo "Directory already exists"
    else
        mkdir -p "$directory"
        echo "Directory created: $directory"
    fi
}

create_directory /tmp/admin-demo
```

---

## 34. Check File Existence

Checks whether a regular file exists.

```bash
#!/bin/bash

check_file() {
    local file="$1"

    if [[ -f "$file" ]]; then
        echo "File exists: $file"
    else
        echo "File does not exist: $file"
    fi
}

check_file /etc/passwd
```

---

## 35. Backup a File

Creates a timestamped backup while preserving file metadata.

```bash
#!/bin/bash

backup_file() {
    local source="$1"
    local destination="${source}.$(date +%Y%m%d_%H%M%S).bak"

    cp -p "$source" "$destination"
    echo "Backup created: $destination"
}

backup_file /etc/hosts
```

---

## 36. Backup a Directory

Creates a compressed archive of a directory.

```bash
#!/bin/bash

backup_directory() {
    local source="$1"
    local backup="/tmp/$(basename "$source")-$(date +%F).tar.gz"

    tar -czf "$backup" "$source"
    echo "Backup created: $backup"
}

backup_directory /etc
```

---

## 37. Delete Old Log Files

Deletes log files older than the requested retention period.

```bash
#!/bin/bash

delete_old_logs() {
    local directory="$1"
    local days="$2"

    find "$directory" -type f -name "*.log" -mtime +"$days" -delete
    echo "Deleted log files older than $days days"
}

delete_old_logs /var/log/myapp 30
```

---

## 38. Find Large Files

Finds files above a specified size on the same filesystem.

```bash
#!/bin/bash

find_large_files() {
    local directory="$1"
    local size="$2"

    find "$directory" -xdev -type f -size +"$size" -print
}

find_large_files /var 500M
```

---

## 39. Count Files in a Directory

Counts regular files directly inside a directory.

```bash
#!/bin/bash

count_files() {
    local directory="$1"
    local count

    count=$(find "$directory" -maxdepth 1 -type f | wc -l)
    echo "Files in $directory: $count"
}

count_files /etc
```

---

## 40. Check Inode Usage

Displays inode usage, which can fill up even when disk space remains.

```bash
#!/bin/bash

check_inodes() {
    df -Pi
}

check_inodes
```

---

## 41. Search Logs for Errors

Searches a log file for common error words.

```bash
#!/bin/bash

search_errors() {
    local logfile="$1"

    grep -iE "error|failed|critical" "$logfile" | tail -20
}

search_errors /var/log/syslog
```

---

## 42. Count Error Messages

Counts case-insensitive occurrences of the word `error`.

```bash
#!/bin/bash

count_errors() {
    local logfile="$1"
    local count

    count=$(grep -ic "error" "$logfile")
    echo "Error count: $count"
}

count_errors /var/log/application.log
```

---

## 43. Show Recent Journal Errors

Displays system journal messages with error priority from the last hour.

```bash
#!/bin/bash

recent_errors() {
    journalctl -p err --since "1 hour ago" --no-pager
}

recent_errors
```

---

## 44. Check Package Installation

Checks package installation on RPM- or Debian-based systems.

```bash
#!/bin/bash

check_package() {
    local package="$1"

    if command -v rpm >/dev/null 2>&1; then
        rpm -q "$package"
    elif command -v dpkg >/dev/null 2>&1; then
        dpkg -s "$package" >/dev/null 2>&1 &&
            echo "$package is installed" ||
            echo "$package is not installed"
    fi
}

check_package curl
```

---

## 45. Install a Package

Installs a package using either `dnf` or `apt`.

```bash
#!/bin/bash

install_package() {
    local package="$1"

    if command -v dnf >/dev/null 2>&1; then
        sudo dnf install -y "$package"
    elif command -v apt >/dev/null 2>&1; then
        sudo apt update
        sudo apt install -y "$package"
    else
        echo "Unsupported package manager"
    fi
}

install_package curl
```

---

## 46. Check System Uptime

Displays uptime in a human-readable format.

```bash
#!/bin/bash

show_uptime() {
    uptime -p
}

show_uptime
```

---

## 47. Check Last Reboot

Shows when the system was last booted.

```bash
#!/bin/bash

last_reboot() {
    who -b
}

last_reboot
```

---

## 48. Check Expiring User Passwords

Displays password aging and expiry information.

```bash
#!/bin/bash

check_password_expiry() {
    local username="$1"
    sudo chage -l "$username"
}

check_password_expiry root
```

---

## 49. Write a Message to Syslog

Writes a custom message to syslog or the systemd journal.

```bash
#!/bin/bash

write_log() {
    local message="$1"
    logger -t admin-script "$message"
}

write_log "Backup completed successfully"
```

---

## 50. Complete Basic Health Check

Combines several smaller functions into one complete system health check.

```bash
#!/bin/bash

check_disk() {
    df -h /
}

check_memory() {
    free -h
}

check_load() {
    uptime
}

check_services() {
    for service in sshd cron; do
        systemctl is-active "$service"
    done
}

system_health_check() {
    echo "===== DISK ====="
    check_disk

    echo "===== MEMORY ====="
    check_memory

    echo "===== LOAD ====="
    check_load

    echo "===== SERVICES ====="
    check_services
}

system_health_check
```

---


# Function Best Practices

## Use `local` Variables

```bash
check_service() {
    local service="$1"
}
```

Without `local`, the variable becomes global and may overwrite another variable.

## Quote Variables

Use:

```bash
rm -f "$file"
```

Avoid:

```bash
rm -f $file
```

Quoting protects paths containing spaces and prevents wildcard expansion.

## Validate Parameters

```bash
backup_file() {
    if [[ $# -ne 1 ]]; then
        echo "Usage: backup_file <filename>"
        return 1
    fi
}
```

## Return an Exit Status

```bash
check_file() {
    [[ -f "$1" ]]
}
```

A function can return:

- `0` for success
- Non-zero for failure

## Call Functions from `main`

```bash
main() {
    check_disk
    check_memory
    check_services
}

main "$@"
```

This structure makes larger scripts easier to read and maintain.

---

# Interview Answer

A Bash function is a reusable block of commands grouped under a name. Functions reduce repeated code, make scripts easier to maintain, and allow parameters to be passed using `$1`, `$2`, and `$@`. System administrators commonly use functions for service checks, disk monitoring, backups, user management, log analysis, and network validation. Variables declared with `local` remain limited to the function, and the function can return an exit status to indicate success or failure.

---

# Recommended Learning Order

1. Simple functions without parameters
2. Functions using `$1` and `$2`
3. Functions using `local`
4. Functions with `if` conditions
5. Functions containing loops
6. Functions returning exit codes
7. Multiple functions called by a `main` function
8. Production scripts with logging and error handling
