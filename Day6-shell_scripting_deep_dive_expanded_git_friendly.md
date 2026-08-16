# DEVOPS INTERVIEW PREP SERIES

# Shell Scripting Deep Dive



# 4.1 Bash Scripting Basics


```bash
#!/bin/bash
# Shebang - specifies the interpreter

# Script metadata
# Author: ...
# Date: ...
# Purpose: Monitor disk usage

# Variables
THRESHOLD=80
EMAIL="admin@example.com"

# Functions
check_disk() {
  local mount_point=$1
  usage=$(df -h "$mount_point" | awk 'NR==2 {print $5}' | sed 's/%//')
  echo "$usage"
}

# Main logic
main() {
  usage=$(check_disk "/")
  if [ "$usage" -gt "$THRESHOLD" ]; then
      echo "Disk usage is ${usage}% - exceeds threshold!"
  fi
}

# Execute
main
```

Good structure: shebang first, then metadata comments, then variables, then functions, then a clear `main` that ties everything together and is called at the very end.

### Why this structure is useful

A predictable script layout makes troubleshooting easier. During an interview, explain that a good Bash script normally separates:

1. Interpreter declaration.
2. Script metadata/comments.
3. Constants and configuration.
4. Functions.
5. Main execution logic.
6. Final `main` call.

### Additional Example — Deployment Readiness Script

```bash
#!/bin/bash

# Purpose: Verify that required commands exist before deployment.

REQUIRED_COMMANDS=("docker" "kubectl" "git")

check_command() {
  local command_name=$1

  if command -v "$command_name" >/dev/null 2>&1; then
    echo "OK: $command_name is installed"
    return 0
  else
    echo "ERROR: $command_name is missing"
    return 1
  fi
}

main() {
  local failed=0

  for command_name in "${REQUIRED_COMMANDS[@]}"; do
    if ! check_command "$command_name"; then
      failed=1
    fi
  done

  return "$failed"
}

main
```

### Possible Output

```text
OK: docker is installed
OK: kubectl is installed
ERROR: git is missing
```

### Interview Note

A strong short answer is:

> I keep Bash scripts modular: shebang, comments, configuration, reusable functions, a `main` function, and a clear exit path. This makes the script easier to test, review, and maintain.

### Production Note

For operational scripts, commonly used safety options are:

```bash
set -euo pipefail
```

Use them deliberately, not blindly:

- `-e` exits when an unhandled command fails.
- `-u` treats an unset variable as an error.
- `pipefail` makes a pipeline fail if any important command in the pipeline fails.

---

## Variables, command substitution, and special variables

### Original PDF Content

```bash
NAME="sreejith"
AGE=23
readonly PI=3.14159       # constant, cannot be changed later

echo "Hello ${NAME}"      # preferred - clear variable boundary

CURRENT_DATE=$(date +%Y-%m-%d)      # command substitution (modern)
FILES_COUNT=`ls -1 | wc -l`         # old-style backticks

export PATH=$PATH:/opt/bin
echo $HOME; echo $USER; echo $PWD
```

### Special Variables

| Special Variable | Meaning |
|---|---|
| `$0` | Script name |
| `$1, $2, ...` | Positional arguments |
| `$#` | Number of arguments |
| `$@` | All arguments, as separate words |
| `$*` | All arguments, as one single word |
| `$$` | Current process's PID |
| `$?` | Exit status of the last command |
| `$!` | PID of the last background process |

### Additional Example — Positional Arguments

Create a script called `user_info.sh`:

```bash
#!/bin/bash

echo "Script name : $0"
echo "First arg   : $1"
echo "Second arg  : $2"
echo "Arg count   : $#"

echo "All arguments:"
for arg in "$@"; do
  echo "- $arg"
done
```

Run:

```bash
./user_info.sh nginx production
```

Possible output:

```text
Script name : ./user_info.sh
First arg   : nginx
Second arg  : production
Arg count   : 2
All arguments:
- nginx
- production
```

### Additional Example — Exit Status

```bash
ping -c 1 8.8.8.8 >/dev/null 2>&1
status=$?

if [ "$status" -eq 0 ]; then
  echo "Network reachable"
else
  echo "Network check failed"
fi
```

### Additional Example — Background Process PID

```bash
sleep 60 &
background_pid=$!

echo "Started background process with PID: $background_pid"
```

### Interview Note

Prefer:

```bash
CURRENT_DATE=$(date +%Y-%m-%d)
```

instead of old-style backticks because `$(...)` is easier to read and nest.

When iterating arguments, `"$@"` is usually preferred because each original argument remains a separate argument.

---

## Conditional statements and comparison operators



```bash
if [ "$AGE" -gt 18 ]; then
     echo "Adult"
elif [ "$AGE" -eq 18 ]; then
     echo "Just turned adult"
else
     echo "Minor"
fi
```

### Comparison Operators

| Numeric | String | File Tests |
|---|---|---|
| `-eq`, `-ne`, `-gt`, `-ge`, `-lt`, `-le` | `=`, `!=`, `-z` (empty), `-n` (non-empty) | `-f` (file), `-d` (dir), `-e` (exists), `-r/-w/-x` (permissions), `-s` (not empty) |

```bash
if [ -f "/etc/passwd" ]; then echo "File exists"; fi
if [ -z "$VAR" ]; then echo "Variable is empty"; fi

# Logical operators
if [ "$AGE" -gt 18 ] && [ "$COUNTRY" = "USA" ]; then echo "Can vote"; fi
if [ "$AGE" -lt 18 ] || [ "$STUDENT" = "yes" ]; then echo "Discount"; fi

# [[ ]] - extended test, supports regex directly
if [[ "$EMAIL" =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]; then
    echo "Valid email"
fi

# case statement
case "$OS" in
  linux) echo "Linux system" ;;
  darwin) echo "macOS" ;;
  *)      echo "Unknown OS" ;;
esac
```

### Additional Example — Check Directory and Permissions

```bash
CONFIG_DIR="/etc/myapp"

if [ -d "$CONFIG_DIR" ] && [ -r "$CONFIG_DIR" ]; then
  echo "$CONFIG_DIR exists and is readable"
else
  echo "$CONFIG_DIR is missing or not readable"
fi
```

### Additional Example — Dev/Test/Prod Using `case`

```bash
ENVIRONMENT="$1"

case "$ENVIRONMENT" in
  dev)
    echo "Using development settings"
    ;;
  test)
    echo "Using test settings"
    ;;
  prod)
    echo "Using production settings"
    ;;
  *)
    echo "Usage: $0 {dev|test|prod}"
    exit 1
    ;;
esac
```

### Additional Example — Validate an Integer

```bash
VALUE="123"

if [[ "$VALUE" =~ ^[0-9]+$ ]]; then
  echo "Numeric value"
else
  echo "Not a valid positive integer"
fi
```

### Interview Note

Use `[ ... ]` for traditional test expressions and `[[ ... ]]` when you specifically need Bash features such as safer pattern matching or regular expressions.

---

## Loop types in bash



```bash
# For loop - iterate a list
for i in 1 2 3 4 5; do echo "Number: $i"; done

# For loop - C style
for ((i=1; i<=5; i++)); do echo "Count: $i"; done

# For loop - files
for file in /var/log/*.log; do echo "Processing $file"; done

# For loop - command output
for user in $(cat /etc/passwd | cut -d: -f1); do echo "User: $user"; done

# While loop
count=1
while [ $count -le 5 ]; do echo "Count: $count"; ((count++)); done

# While - read a file line by line
while IFS= read -r line; do echo "Line: $line"; done < input.txt

# Until loop
count=1
until [ $count -gt 5 ]; do echo "Count: $count"; ((count++)); done

# break and continue
for i in {1..10}; do
   if [ $i -eq 5 ]; then continue; fi      # skip 5
   if [ $i -eq 8 ]; then break; fi         # stop at 8
   echo $i
done
```

### Additional Example — Check Multiple Services

```bash
SERVICES=("nginx" "ssh" "cron")

for service in "${SERVICES[@]}"; do
  if systemctl is-active --quiet "$service"; then
    echo "$service: running"
  else
    echo "$service: stopped"
  fi
done
```

### Additional Example — Retry Until an Endpoint Responds

```bash
attempt=1

until curl -fsS http://localhost:8080/health >/dev/null 2>&1; do
  echo "Attempt $attempt: service not ready"
  ((attempt++))

  if [ "$attempt" -gt 5 ]; then
    echo "Service did not become ready"
    exit 1
  fi

  sleep 5
done

echo "Service is ready"
```

### Additional Example — Safer File-by-File Processing

```bash
while IFS= read -r -d '' file; do
  echo "Processing: $file"
done < <(find /var/log -type f -name '*.log' -print0)
```

This style safely handles filenames containing spaces.

### Interview Note

For reading a file line-by-line, this is the classic safe form:

```bash
while IFS= read -r line; do
  echo "$line"
done < input.txt
```

`IFS=` prevents unwanted trimming, and `-r` prevents backslash escaping.

---

## Functions, return values, and local vs global variables



```bash
greet() {
  echo "Hello $1"
}

# Function "returning" a value via echo + capture
add() {
  local sum=$(($1 + $2))
  echo $sum
}

result=$(add 5 3)

# Function with a return CODE (0-255, not a real value)
check_file() {
  if [ -f "$1" ]; then return 0; else return 1; fi
}

if check_file "/etc/passwd"; then echo "File exists"; fi

# Local vs global
global_var="I'm global"

my_function() {
  local local_var="I'm local"     # only visible inside this function
  global_var="Modified global"    # modifies the outer variable
}
```

### Additional Example — Function Output vs Return Code

```bash
get_hostname() {
  hostname
}

check_root() {
  if [ "$EUID" -eq 0 ]; then
    return 0
  fi

  return 1
}

server_name=$(get_hostname)
echo "Server: $server_name"

if check_root; then
  echo "Running as root"
else
  echo "Not running as root"
fi
```

### Additional Example — Local Variables

```bash
APP="frontend"

show_app() {
  local APP="paymentservice"
  echo "Inside function: $APP"
}

show_app
echo "Outside function: $APP"
```

Possible output:

```text
Inside function: paymentservice
Outside function: frontend
```

### Interview Note

A Bash function's `return` statement is mainly for a status code. A normal successful command returns `0`; non-zero indicates failure.

If you want a function to produce a value such as a hostname, count, or calculated number, print it and capture it with command substitution:

```bash
value=$(my_function)
```

---

# 4.2 Text Processing

## `grep` — pattern matching reference

### Original PDF Content

```bash
grep "error" /var/log/syslog
grep -i "error" file.txt              # case insensitive
grep -v "debug" file.txt              # invert match
grep -c "error" file.txt              # count matches
grep -n "error" file.txt              # show line numbers
grep -r "TODO" /path/to/code          # recursive
grep -l "error" *.log                 # show only filenames with matches

grep -A 3 "error" file.txt            # 3 lines after
grep -B 3 "error" file.txt            # 3 lines before
grep -C 3 "error" file.txt            # 3 lines before and after

grep -E "error|warning|critical" file.txt   # extended regex
grep -w "log" file.txt                     # word boundary - matches "log" not "login"
grep -f patterns.txt input.txt              # multiple patterns from a file
```

### Additional Example — Search Kubernetes Logs

```bash
kubectl logs deployment/frontend | grep -Ei "error|failed|timeout"
```

### Additional Example — Exclude Noise

```bash
grep -i "error" application.log | grep -v "known harmless error"
```

### Additional Example — Show a Failure with Context

```bash
grep -n -C 5 "OutOfMemoryError" application.log
```

### Additional Example — Count HTTP 500 Entries

```bash
grep -c ' 500 ' access.log
```

### Interview Note

Common production usage:

```text
grep  -> find matching lines
-v    -> exclude lines
-i    -> ignore case
-n    -> show line numbers
-r    -> recursive search
-c    -> count
-E    -> extended regular expressions
-A/B/C -> show surrounding context
```

---

## `sed` — stream editor reference

### Original PDF Content

```bash
sed 's/old/new/' file.txt             # first occurrence per line
sed 's/old/new/g' file.txt            # all occurrences
sed 's/old/new/gi' file.txt           # global, case-insensitive

sed -i 's/old/new/g' file.txt              # edit in-place
sed -i.bak 's/old/new/g' file.txt          # edit in-place with backup

sed '/pattern/d' file.txt             # delete matching lines
sed '2,5d' file.txt                   # delete lines 2-5

sed -n '10p' file.txt                 # print only line 10
sed -n '/pattern/p' file.txt          # print only matching lines

sed '5i\New line' file.txt            # insert before line 5
sed '5a\New line' file.txt            # append after line 5

sed 's/foo/bar/g; s/baz/qux/g' file.txt     # multiple operations

# Replace using regex capture groups
echo "2024-01-15" | sed 's/\([0-9]\{4\}\)-\([0-9]\{2\}\)-\([0-9]\{2\}\)/\3\/\2\/\1/'

# Output: 15/01/2024
```

### Additional Example — Update a Docker Image Tag

Input:

```yaml
image: myregistry/frontend:42
```

Command:

```bash
sed -i.bak 's#myregistry/frontend:[0-9][0-9]*#myregistry/frontend:43#g' deployment.yaml
```

Using `#` as the delimiter can make paths and URLs easier to read than `/`.

### Additional Example — Remove Comment Lines

```bash
sed '/^[[:space:]]*#/d' config.txt
```

### Additional Example — Print a Range

```bash
sed -n '20,30p' application.log
```

### Additional Example — Prefix Every Line

```bash
sed 's/^/[APP] /' application.log
```

### Interview Note

`sed` is very useful in CI/CD for small controlled text substitutions, but for complex YAML/JSON editing, use structure-aware tools such as `yq` or `jq` rather than relying on fragile text replacement.

---

## `awk` — pattern scanning and processing reference



```bash
awk '{print $1}' file.txt                         # first column
awk '{print $1, $3}' file.txt                     # first and third
awk '{print $NF}' file.txt                        # last column

awk -F: '{print $1}' /etc/passwd                  # custom field separator

awk '/error/ {print $0}' file.txt                 # lines matching a pattern
awk '$3 > 100 {print $1, $3}' file.txt            # numeric comparison

# Built-ins: NR (line number), NF (field count), FS, OFS, RS

awk 'NR==5' file.txt                              # print line 5
awk 'END {print NR}' file.txt                     # total line count

awk '{sum += $3} END {print sum}' file.txt        # sum a column
awk '{sum += $3} END {print sum/NR}' file.txt     # average
awk '{if ($3 > max) max = $3} END {print max}' file.txt   # maximum

awk '{printf "%-10s %5d\n", $1, $2}' file.txt    # formatted output

awk '/error/ {count++} END {print "Errors:", count}' /var/log/syslog
```

### Additional Example — Disk Usage Above 80%

```bash
df -P | awk 'NR>1 && $5+0 >= 80 {print $1, $5, $6}'
```

Possible output:

```text
/dev/xvda1 86% /
```

### Additional Example — Count HTTP Status Codes

Assume the HTTP status is field 9 in an access log:

```bash
awk '{count[$9]++} END {for (code in count) print code, count[code]}' access.log
```

Possible output:

```text
200 10542
404 217
500 31
```

### Additional Example — Sum File Sizes

```bash
ls -l *.log | awk '{sum += $5} END {print "Total bytes:", sum}'
```

### Additional Example — Extract Kubernetes Pod Name and Status

```bash
kubectl get pods --no-headers | awk '{print $1, $3}'
```

### Interview Note

Useful built-ins to remember:

| Variable | Meaning |
|---|---|
| `NR` | Current input record/line number |
| `NF` | Number of fields in current record |
| `FS` | Input field separator |
| `OFS` | Output field separator |
| `RS` | Input record separator |
| `$0` | Entire current record |
| `$1...$NF` | Individual fields |

---

## `cut`, `sort`/`uniq`, and `tr` — quick reference

### Original PDF Content

```bash
# cut - extract columns
cut -c1-5 file.txt                  # characters 1-5
cut -d: -f1,3 /etc/passwd          # fields 1 and 3, colon-delimited

# sort & uniq
sort file.txt                       # alphabetical
sort -n file.txt                    # numeric
sort -k 2 file.txt                  # sort by column 2
sort file.txt | uniq -c             # count occurrences of each line
sort file.txt | uniq -d             # only lines that appear more than once

# tr - translate characters
echo "Hello" | tr '[:lower:]' '[:upper:]'        # HELLO
echo "Hello123" | tr -d '[:digit:]'              # Hello
echo "Hello   World" | tr -s ' '                 # squeeze repeated spaces
```

### Additional Example — List Login Names

```bash
cut -d: -f1 /etc/passwd
```

### Additional Example — Most Common Error

```bash
grep "ERROR" application.log |
  sort |
  uniq -c |
  sort -nr |
  head
```

### Additional Example — Find Duplicate Lines

```bash
sort file.txt | uniq -d
```

### Additional Example — Normalize Text to Uppercase

```bash
echo "production" | tr '[:lower:]' '[:upper:]'
```

Output:

```text
PRODUCTION
```

### Interview Note

`uniq` normally works on adjacent duplicate lines, which is why it is commonly paired with `sort`.

---

# 4.3 Practical SRE Scripts

## 1. Disk Usage Monitor

### Original PDF Content

```bash
#!/bin/bash
# disk_monitor.sh - Alert when disk usage exceeds threshold

THRESHOLD=80
EMAIL="admin@example.com"
HOSTNAME=$(hostname)

check_disk() {
  df -H | grep -vE '^Filesystem|tmpfs|cdrom' | awk '{print $5 " " $1}' | while read output; do
       usage=$(echo $output | awk '{print $1}' | sed 's/%//g')
       partition=$(echo $output | awk '{print $2}')
       if [ $usage -ge $THRESHOLD ]; then
           echo "ALERT: Disk usage on $partition is ${usage}% on $HOSTNAME"
       fi
  done
}

check_disk
```

### What the script does

1. Runs `df -H`.
2. Removes header/tmpfs/cdrom entries.
3. Extracts percentage and filesystem.
4. Removes `%`.
5. Compares usage with the threshold.
6. Prints an alert when the threshold is reached.

### Additional Example — Include Mount Point and Logging

```bash
#!/bin/bash

THRESHOLD=80
LOGFILE="/tmp/disk_monitor.log"

df -P | awk 'NR > 1 {print $1, $5, $6}' |
while read -r filesystem usage mountpoint; do
  usage_number=${usage%\%}

  if [ "$usage_number" -ge "$THRESHOLD" ]; then
    echo "$(date '+%F %T') ALERT filesystem=$filesystem mount=$mountpoint usage=$usage" |
      tee -a "$LOGFILE"
  fi
done
```

Possible output:

```text
2026-08-16 08:15:10 ALERT filesystem=/dev/xvda1 mount=/ usage=86%
```

### Interview Note

In production, I would normally alert through the monitoring platform rather than only printing to terminal. For example: Prometheus/Alertmanager, CloudWatch, Datadog, Zabbix, PagerDuty, or an incident webhook.

---

## 2. Log Analyzer

### Original PDF Content

```bash
#!/bin/bash
# log_analyzer.sh - Analyze log files for errors

LOGFILE="/var/log/application.log"
REPORT="/tmp/log_report_$(date +%Y%m%d).txt"

analyze_log() {
  echo "Log Analysis Report - $(date)" > $REPORT
  error_count=$(grep -c "ERROR" $LOGFILE)
  echo "Total Errors: $error_count" >> $REPORT

  echo "Top 10 Error Messages:" >> $REPORT
  grep "ERROR" $LOGFILE | awk '{print $5,$6,$7,$8,$9}' | sort | uniq -c | sort -rn |
  head -10 >> $REPORT

  echo "Errors by Hour:" >> $REPORT
  grep "ERROR" $LOGFILE | awk '{print $2}' | cut -d: -f1 | sort | uniq -c >> $REPORT

  cat $REPORT
}

analyze_log
```

### What the script does

- Creates a daily report filename.
- Counts `ERROR` lines.
- Groups repeated error messages.
- Shows the top 10.
- Groups errors by hour.
- Displays the final report.

### Additional Example — Analyze ERROR/WARN/CRITICAL

```bash
#!/bin/bash

LOGFILE="/var/log/application.log"

echo "Severity summary:"
grep -E "ERROR|WARN|CRITICAL" "$LOGFILE" |
awk '
/CRITICAL/ {critical++}
/ERROR/    {error++}
/WARN/     {warn++}
END {
  print "CRITICAL:", critical+0
  print "ERROR   :", error+0
  print "WARN    :", warn+0
}'
```

Possible output:

```text
Severity summary:
CRITICAL: 3
ERROR   : 47
WARN    : 122
```

### Additional Example — Find Recent Authentication Failures

```bash
grep -Ei "failed password|authentication failure" /var/log/auth.log | tail -20
```

### Interview Note

A good log-analysis interview answer should mention that log format matters. Fixed field positions such as `$5,$6,$7...` work only if the log format is consistent. JSON logs should usually be parsed with `jq`.

---

## 3. Service Health Check



```bash
#!/bin/bash
# health_check.sh - Check service health and restart if needed

SERVICES=("nginx" "mysql" "redis")
LOG="/var/log/health_check.log"

check_service() {
  local service=$1

  if systemctl is-active --quiet $service; then
     echo "$(date): $service is running" >> $LOG
     return 0
  else
     echo "$(date): $service is DOWN - Attempting restart" >> $LOG
     systemctl restart $service
     sleep 5

     if systemctl is-active --quiet $service; then
         echo "$(date): $service restarted successfully" >> $LOG
     else
         echo "$(date): CRITICAL - $service failed to restart" >> $LOG
     fi

     return 1
  fi
}

for service in "${SERVICES[@]}"; do
   check_service $service
done
```

### What the script does

1. Checks each service with `systemctl is-active`.
2. Logs healthy services.
3. Restarts unhealthy services.
4. Waits 5 seconds.
5. Checks again.
6. Logs success or critical failure.

### Additional Example — HTTP Health Check

```bash
#!/bin/bash

URL="http://localhost:8080/health"

if curl -fsS --max-time 5 "$URL" >/dev/null; then
  echo "$(date): application health endpoint is OK"
else
  echo "$(date): application health endpoint FAILED"
  exit 1
fi
```

### Additional Example — Check Both Process and Port

```bash
SERVICE="nginx"
PORT=80

if systemctl is-active --quiet "$SERVICE" && ss -lnt | awk '{print $4}' | grep -q ":${PORT}$"; then
  echo "$SERVICE is active and port $PORT is listening"
else
  echo "$SERVICE health check failed"
fi
```

### Interview Note

Restarting a service automatically can hide recurring failures. In production, add:

- restart-attempt limits,
- alerting,
- logs/journal collection,
- dependency checks,
- root-cause investigation.

---

## 4. Backup Script (with rotation)

### Original PDF Content

```bash
#!/bin/bash
# backup.sh - Automated backup with rotation

SOURCE="/var/www"
DEST="/backup"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_NAME="backup_${DATE}.tar.gz"
RETENTION_DAYS=7

tar -czf ${DEST}/${BACKUP_NAME} $SOURCE 2>/dev/null

if [ $? -eq 0 ]; then
     echo "Backup completed: ${DEST}/${BACKUP_NAME}"
     find $DEST -name "backup_*.tar.gz" -type f -mtime +$RETENTION_DAYS -delete
     echo "Old backups cleaned up (retention: $RETENTION_DAYS days)"
else
     echo "ERROR: Backup failed"
     exit 1
fi

if [ -f "${DEST}/${BACKUP_NAME}" ]; then
    size=$(du -h "${DEST}/${BACKUP_NAME}" | cut -f1)
    echo "Backup size: $size"
fi
```

### What the script does

- Compresses `/var/www`.
- Stores the archive under `/backup`.
- Includes a timestamp in the filename.
- Checks the result of `tar`.
- Deletes backup files older than the retention period.
- Prints the final archive size.

### Additional Example — Safer Backup with Explicit Error Handling

```bash
#!/bin/bash

SOURCE="/var/www"
DEST="/backup"
RETENTION_DAYS=7
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$DEST/backup_${DATE}.tar.gz"

mkdir -p "$DEST"

if tar -czf "$BACKUP_FILE" "$SOURCE"; then
  echo "Backup created: $BACKUP_FILE"

  find "$DEST" \
    -type f \
    -name 'backup_*.tar.gz' \
    -mtime "+$RETENTION_DAYS" \
    -delete

  du -h "$BACKUP_FILE"
else
  echo "Backup failed"
  exit 1
fi
```

### Additional Example — Verify Archive Readability

```bash
if tar -tzf "$BACKUP_FILE" >/dev/null 2>&1; then
  echo "Archive verification successful"
else
  echo "Archive verification failed"
  exit 1
fi
```

### Interview Note

A production backup plan should discuss restore testing, encryption, off-host/off-site storage, retention policy, monitoring, and recovery objectives. A backup that has never been restored is not fully proven.

---

## 5. Process Monitor

### Original PDF Content

```bash
#!/bin/bash
# process_monitor.sh - Monitor CPU/Memory usage of processes

THRESHOLD_CPU=80
THRESHOLD_MEM=80

monitor_processes() {
  echo "Process Monitoring - $(date)"

  echo -e "\nTop 5 CPU consumers:"
  ps aux --sort=-%cpu | head -6 | awk '{printf "%-10s %5s %5s %s\n", $1, $3, $4, $11}'

  echo -e "\nTop 5 Memory consumers:"
  ps aux --sort=-%mem | head -6 | awk '{printf "%-10s %5s %5s %s\n", $1, $3, $4, $11}'

  echo -e "\nProcesses exceeding thresholds:"
  ps aux | awk -v cpu=$THRESHOLD_CPU -v mem=$THRESHOLD_MEM '
    NR>1 {
       if ($3 > cpu || $4 > mem) {
           printf "PID: %-6s CPU: %5s%% MEM: %5s%% CMD: %s\n", $2, $3, $4, $11
       }
    }'
}

monitor_processes
```

### What the script does

- Shows the top CPU-consuming processes.
- Shows the top memory-consuming processes.
- Searches all processes for CPU or memory percentages above configured thresholds.

### Additional Example — Include Full Command Line

```bash
ps -eo pid,user,%cpu,%mem,args --sort=-%cpu | head
```

### Additional Example — Watch a Specific Process

```bash
PROCESS_NAME="java"

ps -C "$PROCESS_NAME" -o pid,%cpu,%mem,etime,args
```

### Additional Example — Alert for High CPU

```bash
CPU_THRESHOLD=80

ps -eo pid,%cpu,%mem,args --no-headers |
awk -v threshold="$CPU_THRESHOLD" '
$2 >= threshold {
  printf "HIGH CPU: PID=%s CPU=%s%% MEM=%s%% CMD=%s\n", $1, $2, $3, $4
}'
```

### Interview Note

High CPU is not automatically a fault. Always correlate process utilization with load average, application latency, throughput, GC behavior, disk I/O, memory pressure, and recent deployments.

---

# 4.4 Cron Jobs

## Cron syntax and common scheduling examples

### Original PDF Content

```text
* * * * * command
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, 0 and 7 are Sunday)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

```bash
crontab -e     # edit crontab
crontab -l     # view crontab
crontab -r     # remove crontab

*/5 * * * * /path/to/script.sh            # every 5 minutes
0 * * * * /path/to/script.sh              # every hour
30 2 * * * /path/to/backup.sh             # every day at 2:30 AM
0 0 * * 0 /path/to/weekly_task.sh         # every Sunday at midnight
0 0 1 * * /path/to/monthly_task.sh        # 1st of every month
0 9 * * 1-5 /path/to/workday_task.sh      # Monday-Friday at 9 AM
*/15 9-17 * * 1-5 /path/to/script.sh      # every 15 min, business hours
0 6,12,18 * * * /path/to/script.sh        # multiple times a day

# Redirect output to a log
0 0 * * * /path/to/script.sh >> /var/log/cron.log 2>&1
```

### Field Meaning

| Position | Field | Range |
|---|---|---|
| 1 | Minute | `0-59` |
| 2 | Hour | `0-23` |
| 3 | Day of month | `1-31` |
| 4 | Month | `1-12` |
| 5 | Day of week | `0-7`, where 0 and 7 are Sunday |

### Additional Example — Every 10 Minutes

```cron
*/10 * * * * /opt/scripts/check_api.sh
```

### Additional Example — Daily at 01:15

```cron
15 1 * * * /opt/scripts/database_backup.sh
```

### Additional Example — Weekdays at 18:30

```cron
30 18 * * 1-5 /opt/scripts/report.sh
```

### Additional Example — First Day of Every Month at 06:00

```cron
0 6 1 * * /opt/scripts/monthly_cleanup.sh
```

### Additional Example — Capture stdout and stderr

```cron
*/5 * * * * /opt/scripts/health_check.sh >> /var/log/health_check.log 2>&1
```

### Production Notes for Cron

Cron often runs with a much smaller environment than your interactive shell. Therefore:

- Use absolute paths where possible.
- Define `PATH` if required.
- Redirect stdout/stderr.
- Confirm the script is executable.
- Confirm the cron user has permissions.
- Be aware of timezone.
- Add locking when overlapping executions would be dangerous.

Example:

```cron
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

*/5 * * * * /usr/bin/flock -n /tmp/health_check.lock /opt/scripts/health_check.sh >> /var/log/health_check.log 2>&1
```

---

## Cron special strings

### Original PDF Content

| Shortcut | Equivalent |
|---|---|
| `@reboot` | Run once at startup |
| `@yearly` | `0 0 1 1 *` |
| `@monthly` | `0 0 1 * *` |
| `@weekly` | `0 0 * * 0` |
| `@daily` | `0 0 * * *` |
| `@hourly` | `0 * * * *` |

```cron
@daily /path/to/daily_backup.sh
@reboot /path/to/startup_script.sh
```

### Additional Example — Run Monitoring Bootstrap After Reboot

```cron
@reboot /opt/scripts/start_monitoring_agent.sh >> /var/log/startup.log 2>&1
```

### Additional Example — Hourly Log Cleanup

```cron
@hourly /opt/scripts/archive_logs.sh
```

### Interview Note

`@reboot` means the command is scheduled when the cron daemon starts. It is useful for startup scripts, but for long-running production services, systemd service units are normally easier to supervise and restart reliably.

---

# Additional Interview Practice

## Question 1 — What is the difference between `$@` and `$*`?

A concise interview answer:

- `"$@"` expands positional parameters as separate arguments.
- `"$*"` inside double quotes combines them into a single string using the first character of `IFS`.
- For forwarding script arguments, `"$@"` is normally the correct choice.

Example:

```bash
show_args() {
  for arg in "$@"; do
    echo "[$arg]"
  done
}

show_args "hello world" "production"
```

Output:

```text
[hello world]
[production]
```

---

## Question 2 — What is the difference between `>` and `>>`?

```bash
echo "new report" > report.txt
```

`>` overwrites the file.

```bash
echo "another line" >> report.txt
```

`>>` appends to the file.

---

## Question 3 — What does `2>&1` mean?

File descriptors:

- `0` = stdin
- `1` = stdout
- `2` = stderr

Example:

```bash
command >> application.log 2>&1
```

This appends standard output to `application.log` and sends standard error to the same destination.

---

## Question 4 — How do you debug a Bash script?

Common methods:

```bash
bash -n script.sh
```

Checks syntax without executing the commands.

```bash
bash -x script.sh
```

Runs with command tracing.

You can also add:

```bash
set -x
```

and later:

```bash
set +x
```

Be careful: command tracing can reveal secrets if variables contain passwords, tokens, or credentials.

---

## Question 5 — How do you check a command before using it?

```bash
if command -v kubectl >/dev/null 2>&1; then
  echo "kubectl is installed"
else
  echo "kubectl is missing"
fi
```

---

## Question 6 — How do you check whether a variable is empty?

```bash
if [ -z "$MY_VAR" ]; then
  echo "MY_VAR is empty"
fi
```

To check that it is non-empty:

```bash
if [ -n "$MY_VAR" ]; then
  echo "MY_VAR has a value"
fi
```

---

## Question 7 — How do you stop a script when a critical command fails?

Explicit handling:

```bash
if ! kubectl apply -f deployment.yaml; then
  echo "Deployment failed"
  exit 1
fi
```

Or, when appropriate:

```bash
set -e
```

Explicit checks are often clearer around critical operational steps.

---

## Question 8 — What is a good shell-scripting example from DevOps work?

A strong answer:

> I use shell scripts for lightweight operational automation such as disk monitoring, log parsing, service health checks, pre-deployment validation, backup rotation, Kubernetes checks, and CI/CD helper tasks. For larger or highly structured automation, I move the logic into a more maintainable tool or language rather than letting one Bash script grow indefinitely.

---

# Quick Revision Cheat Sheet

| Requirement | Typical Tool |
|---|---|
| Search text | `grep` |
| Replace/delete/print text | `sed` |
| Process fields/columns/calculations | `awk` |
| Extract delimited fields | `cut` |
| Sort text | `sort` |
| Count adjacent duplicate lines | `uniq` |
| Translate/delete characters | `tr` |
| Find files | `find` |
| Check service state | `systemctl is-active` |
| Restart a systemd service | `systemctl restart` |
| Schedule recurring jobs | `cron` / `crontab` |
| Capture command output | `$(command)` |
| Last command status | `$?` |
| All positional args | `"$@"` |
| Last background PID | `$!` |

---

# Practical DevOps Example — End-to-End Server Check

```bash
#!/bin/bash

DISK_THRESHOLD=80
SERVICES=("nginx" "ssh")

echo "=== Server Health Check ==="
echo "Host: $(hostname)"
echo "Date: $(date)"

echo
echo "=== Disk Check ==="

df -P | awk 'NR>1 {print $1, $5, $6}' |
while read -r fs usage mount; do
  percent=${usage%\%}

  if [ "$percent" -ge "$DISK_THRESHOLD" ]; then
    echo "WARNING: $fs on $mount is ${usage}"
  fi
done

echo
echo "=== Service Check ==="

for service in "${SERVICES[@]}"; do
  if systemctl is-active --quiet "$service"; then
    echo "OK: $service"
  else
    echo "FAILED: $service"
  fi
done

echo
echo "=== Top CPU Processes ==="

ps -eo pid,user,%cpu,%mem,args --sort=-%cpu | head -6
```

Example output:

```text
=== Server Health Check ===
Host: app01
Date: Sun Aug 16 08:30:00 CEST 2026

=== Disk Check ===
WARNING: /dev/xvda1 on / is 87%

=== Service Check ===
OK: nginx
OK: ssh

=== Top CPU Processes ===
PID USER %CPU %MEM COMMAND
...
```

---

# Research and Validation Notes

The additional explanations and examples were checked against official/primary technical documentation:

1. GNU Bash Reference Manual  
   https://www.gnu.org/software/bash/manual/bash.html

2. GNU Bash — Looping Constructs  
   https://www.gnu.org/software/bash/manual/html_node/Looping-Constructs.html

3. GNU grep documentation  
   https://www.gnu.org/software/grep/manual/

4. GNU sed documentation  
   https://www.gnu.org/software/sed/manual/sed.html

5. GNU awk / gawk User's Guide  
   https://www.gnu.org/software/gawk/manual/gawk.html

6. GNU Coreutils documentation  
   https://www.gnu.org/software/coreutils/manual/

7. GNU Findutils documentation  
   https://www.gnu.org/software/findutils/manual/

8. systemd `systemctl` documentation  
   https://www.freedesktop.org/software/systemd/man/systemctl.html

9. Cronie project documentation  
   https://github.com/cronie-crond/cronie

---

# Final Interview Summary

The most important shell-scripting points to remember are:

- Start scripts with a clear interpreter/shebang.
- Keep configuration, functions, and main logic organized.
- Quote variables unless you intentionally require word splitting or glob expansion.
- Prefer `$(...)` over legacy backticks.
- Know `$?`, `$#`, `"$@"`, `$!`, and `$$`.
- Know numeric, string, and file tests.
- Understand `for`, `while`, `until`, `break`, and `continue`.
- Functions return status codes; use command substitution to capture printed values.
- Learn `grep`, `sed`, `awk`, `cut`, `sort`, `uniq`, and `tr`.
- Build SRE scripts around clear exit codes and useful logs.
- Treat automatic restarts carefully and add monitoring/alerting.
- Backups must be restorable and should have retention/verification.
- Cron jobs need correct permissions, paths, environment, logging, and overlap protection.

