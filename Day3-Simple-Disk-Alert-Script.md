
# Simple Disk Utilization Alert Script (Beginner)

This example checks disk usage using `df -P` and sends an email if any filesystem reaches **80%** or more.

## Script

```bash
#!/bin/bash

THRESHOLD=80
EMAIL="admin@example.com"
HOST=$(hostname)

df -P | awk 'NR>1 {print $5, $6}' |
while read -r USAGE MOUNT
do
    USAGE=${USAGE%\%}

    if [ "$USAGE" -ge "$THRESHOLD" ]
    then
        MESSAGE="Disk usage on $HOST:$MOUNT is ${USAGE}%"

        echo "$MESSAGE"

        echo "$MESSAGE" |
        mail -s "Disk Usage Alert - $HOST" "$EMAIL"
    fi
done
```

---

# Line-by-Line Explanation

| Line | Explanation |
|------|-------------|
| `#!/bin/bash` | Runs the script using Bash. |
| `THRESHOLD=80` | Alert when disk usage is 80% or higher. |
| `EMAIL="admin@example.com"` | Email address that receives alerts. |
| `HOST=$(hostname)` | Stores the current hostname. |
| `df -P` | Displays disk usage in POSIX format (easy for scripts). |
| `awk 'NR>1 {print $5, $6}'` | Skips the header and prints **Use%** and **Mount Point**. |
| `while read -r USAGE MOUNT` | Reads each filesystem one by one. |
| `USAGE=${USAGE%\%}` | Removes the `%` sign (e.g., `85%` → `85`). |
| `if [ "$USAGE" -ge "$THRESHOLD" ]` | Checks whether usage is greater than or equal to 80. |
| `MESSAGE="..."` | Creates the alert message. |
| `echo "$MESSAGE"` | Prints the message on the screen. |
| `mail -s ...` | Sends the email alert. |

---

# Sample Output

Suppose `df -P` returns:

```text
Filesystem 1024-blocks Used Available Capacity Mounted on
/dev/sda2 52428800 44040192 8388608 84% /
```

The script prints:

```text
Disk usage on server1:/ is 84%
```

An email is also sent with the subject:

```text
Disk Usage Alert - server1
```

---

# Run the Script

```bash
chmod +x disk_alert.sh
./disk_alert.sh
```

---

# Schedule with Cron

Run every 5 minutes:

```cron
*/5 * * * * /path/to/disk_alert.sh
```
