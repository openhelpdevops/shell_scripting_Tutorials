# Linux Text Processing: 50 `awk`, `cut`, `uniq`, and `sed` Examples

Linux provides several powerful commands for processing text:

| Command | Main purpose |
|---|---|
| `awk` | Process columns, fields, conditions, and calculations |
| `cut` | Extract characters or delimiter-separated fields |
| `uniq` | Remove or count adjacent duplicate lines |
| `sed` | Search, replace, delete, insert, and transform text |

---

# Part 1: `awk` Examples

## Basic Syntax

```bash
awk 'pattern { action }' filename
```

Common built-in variables:

| Variable | Meaning |
|---|---|
| `$0` | Complete line |
| `$1` | First field |
| `$2` | Second field |
| `$9` | Ninth field |
| `NF` | Number of fields in the current line |
| `NR` | Current input line number |
| `FS` | Input field separator |
| `OFS` | Output field separator |
| `FILENAME` | Current filename |

---

## 1. Print the entire file

```bash
awk '{print $0}' /etc/passwd
```

`$0` represents the complete current line.

---

## 2. Print the first field

```bash
awk '{print $1}' file.txt
```

Example input:

```text
john linux admin
mary database engineer
```

Output:

```text
john
mary
```

---

## 3. Print the ninth field

```bash
ls -l | awk '{print $9}'
```

Example output:

```text
file1.txt
script.sh
notes.md
```

In common `ls -l` output, `$9` is usually the filename.

However, filenames containing spaces can cause inaccurate results. A safer command for listing names is:

```bash
find . -maxdepth 1 -printf '%f\n'
```

---

## 4. Print multiple fields

```bash
awk '{print $1, $3, $5}' file.txt
```

This prints the first, third, and fifth fields.

---

## 5. Print the last field

```bash
awk '{print $NF}' file.txt
```

`NF` is the total number of fields, so `$NF` represents the last field.

---

## 6. Print the second-last field

```bash
awk '{print $(NF-1)}' file.txt
```

Example input:

```text
server1 linux production running
```

Output:

```text
production
```

---

## 7. Print line numbers

```bash
awk '{print NR, $0}' file.txt
```

Example output:

```text
1 first line
2 second line
3 third line
```

---

## 8. Print the number of fields on every line

```bash
awk '{print "Fields:", NF, "Line:", $0}' file.txt
```

---

## 9. Use colon as the delimiter

```bash
awk -F: '{print $1}' /etc/passwd
```

`-F:` tells `awk` to use `:` as the input field separator.

Output:

```text
root
daemon
bin
sreejith
```

---

## 10. Print username and login shell

```bash
awk -F: '{print $1, $7}' /etc/passwd
```

Example output:

```text
root /bin/bash
daemon /usr/sbin/nologin
sreejith /bin/bash
```

---

## 11. Set a custom output separator

```bash
awk -F: 'BEGIN {OFS=" -> "} {print $1, $7}' /etc/passwd
```

Output:

```text
root -> /bin/bash
daemon -> /usr/sbin/nologin
```

---

## 12. Print lines containing a word

```bash
awk '/error/ {print}' application.log
```

This performs a case-sensitive regex search.

It matches:

```text
database error detected
```

It does not match:

```text
Database ERROR detected
```

---

## 13. Perform a case-insensitive search

```bash
awk 'tolower($0) ~ /error/ {print}' application.log
```

This matches:

```text
error
Error
ERROR
eRrOr
```

---

## 14. Print lines beginning with `root`

```bash
awk '/^root/ {print}' /etc/passwd
```

Regex explanation:

| Pattern | Meaning |
|---|---|
| `^root` | Line starts with `root` |

---

## 15. Print lines ending with `bash`

```bash
awk '/bash$/ {print}' /etc/passwd
```

Regex explanation:

| Pattern | Meaning |
|---|---|
| `bash$` | Line ends with `bash` |

---

## 16. Print users whose UID is greater than or equal to 1000

```bash
awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd
```

Here:

```text
$1 = username
$3 = UID
```

---

## 17. Print lines where the first field equals a value

```bash
awk '$1 == "server1" {print}' servers.txt
```

Use `==` for exact comparison.

---

## 18. Print lines where the first field does not equal a value

```bash
awk '$1 != "server1" {print}' servers.txt
```

---

## 19. Use multiple conditions with AND

```bash
awk '$2 == "production" && $3 == "running" {print}' servers.txt
```

Both conditions must be true.

---

## 20. Use multiple conditions with OR

```bash
awk '$2 == "production" || $2 == "staging" {print}' servers.txt
```

At least one condition must be true.

---

## 21. Print a range of lines

```bash
awk 'NR >= 5 && NR <= 10 {print}' file.txt
```

This prints lines 5 through 10.

Another form:

```bash
awk 'NR==5,NR==10' file.txt
```

---

## 22. Calculate the sum of a column

Example file:

```text
server1 20
server2 30
server3 50
```

Command:

```bash
awk '{sum += $2} END {print "Total:", sum}' usage.txt
```

Output:

```text
Total: 100
```

---

## 23. Calculate the average of a column

```bash
awk '{sum += $2} END {if (NR > 0) print "Average:", sum/NR}' usage.txt
```

---

## 24. Find the largest value in a column

```bash
awk 'NR == 1 || $2 > max {max=$2; line=$0} END {print line}' usage.txt
```

This prints the line containing the highest value in column 2.

---

## 25. Replace text using `gsub()`

```bash
awk '{gsub(/error/, "warning"); print}' application.log
```

`gsub()` replaces every occurrence on each line.

To replace only the first matching occurrence, use `sub()`:

```bash
awk '{sub(/error/, "warning"); print}' application.log
```

---

# Part 2: `cut` Examples

## Basic Syntax

```bash
cut OPTION filename
```

Important options:

| Option | Meaning |
|---|---|
| `-c` | Select characters |
| `-b` | Select bytes |
| `-d` | Specify delimiter |
| `-f` | Select fields |
| `--complement` | Select everything except specified fields |
| `-s` | Suppress lines without the delimiter |
| `--output-delimiter` | Set output delimiter |

---

## 26. Extract the first character

```bash
cut -c1 file.txt
```

---

## 27. Extract characters 1 through 5

```bash
cut -c1-5 file.txt
```

Example input:

```text
production
development
```

Output:

```text
produ
devel
```

---

## 28. Extract selected characters

```bash
cut -c1,3,5 file.txt
```

This extracts the first, third, and fifth characters.

---

## 29. Extract characters from position 5 to the end

```bash
cut -c5- file.txt
```

---

## 30. Extract characters from the beginning through position 8

```bash
cut -c-8 file.txt
```

---

## 31. Extract usernames from `/etc/passwd`

```bash
cut -d: -f1 /etc/passwd
```

Explanation:

```text
-d:  use colon as delimiter
-f1  print first field
```

---

## 32. Extract username and UID

```bash
cut -d: -f1,3 /etc/passwd
```

Example output:

```text
root:0
daemon:1
sreejith:1000
```

---

## 33. Extract a range of fields

```bash
cut -d: -f1-4 /etc/passwd
```

This extracts fields 1 through 4.

---

## 34. Extract the third field and everything after it

```bash
cut -d: -f3- /etc/passwd
```

---

## 35. Exclude a specific field

```bash
cut -d: -f2 --complement /etc/passwd
```

This prints every field except field 2.

---

## 36. Change the output delimiter

```bash
cut -d: -f1,3,7 --output-delimiter=' | ' /etc/passwd
```

Example output:

```text
root | 0 | /bin/bash
daemon | 1 | /usr/sbin/nologin
```

---

## 37. Extract data from comma-separated values

Example file:

```text
name,department,salary
John,IT,5000
Mary,Finance,6000
```

Command:

```bash
cut -d, -f1,3 employees.csv
```

Output:

```text
name,salary
John,5000
Mary,6000
```

---

# Part 3: `uniq` Examples

`uniq` processes **adjacent duplicate lines**.

Therefore, it is commonly used with `sort`:

```bash
sort file.txt | uniq
```

Important options:

| Option | Meaning |
|---|---|
| `-c` | Count occurrences |
| `-d` | Print duplicate lines only |
| `-u` | Print unique lines only |
| `-i` | Ignore case |
| `-f N` | Ignore the first N fields |
| `-s N` | Ignore the first N characters |

---

## 38. Remove adjacent duplicate lines

```bash
uniq names.txt
```

Example input:

```text
john
john
mary
mary
paul
```

Output:

```text
john
mary
paul
```

---

## 39. Remove duplicates from an unsorted file

```bash
sort names.txt | uniq
```

A shorter alternative is:

```bash
sort -u names.txt
```

---

## 40. Count the number of occurrences

```bash
sort names.txt | uniq -c
```

Example output:

```text
3 john
2 mary
1 paul
```

---

## 41. Display duplicate lines only

```bash
sort names.txt | uniq -d
```

Output:

```text
john
mary
```

---

## 42. Display lines appearing only once

```bash
sort names.txt | uniq -u
```

Output:

```text
paul
```

---

## 43. Ignore uppercase and lowercase differences

Example input:

```text
Linux
linux
LINUX
Windows
```

Command:

```bash
sort -f operating-systems.txt | uniq -i
```

Output:

```text
Linux
Windows
```

---

## 44. Count duplicate IP addresses in a log

Assume the first column contains the client IP address:

```bash
awk '{print $1}' access.log | sort | uniq -c
```

Sort by the highest number of requests:

```bash
awk '{print $1}' access.log |
sort |
uniq -c |
sort -nr
```

Example output:

```text
150 192.168.1.20
75 192.168.1.30
10 192.168.1.40
```

---

# Part 4: `sed` and Regex Examples

## Basic Syntax

```bash
sed 'command' filename
```

For substitution:

```bash
sed 's/old/new/' filename
```

Structure:

```text
s/pattern/replacement/flags
```

Common flags:

| Flag | Meaning |
|---|---|
| `g` | Replace all matches on a line |
| `i` | Case-insensitive match |
| `p` | Print matched or modified lines |
| `I` | GNU `sed` case-insensitive matching |

---

## 45. Replace the first occurrence on each line

```bash
sed 's/error/warning/' application.log
```

Input:

```text
error: database error
```

Output:

```text
warning: database error
```

Only the first occurrence is replaced.

---

## 46. Replace every occurrence on each line

```bash
sed 's/error/warning/g' application.log
```

Input:

```text
error: database error
```

Output:

```text
warning: database warning
```

---

## 47. Replace text directly inside a file

```bash
sed -i 's/old-server/new-server/g' configuration.conf
```

Create a backup before modifying:

```bash
sed -i.bak 's/old-server/new-server/g' configuration.conf
```

This creates:

```text
configuration.conf
configuration.conf.bak
```

---

## 48. Delete blank lines and comments

Delete empty or whitespace-only lines:

```bash
sed '/^[[:space:]]*$/d' configuration.conf
```

Delete lines beginning with `#`:

```bash
sed '/^[[:space:]]*#/d' configuration.conf
```

Delete both blank lines and comments:

```bash
sed -e '/^[[:space:]]*$/d' \
    -e '/^[[:space:]]*#/d' configuration.conf
```

Regex explanation:

| Pattern | Meaning |
|---|---|
| `^` | Beginning of line |
| `[[:space:]]*` | Zero or more whitespace characters |
| `#` | Literal hash character |
| `$` | End of line |
| `d` | Delete matching line |

---

## 49. Replace `/` inside paths

The `/` character is normally used as the `sed` separator.

To replace:

```text
/var/log/app
```

with:

```text
/opt/log/app
```

you can escape each `/`:

```bash
echo '/var/log/app' |
sed 's/\/var\/log\/app/\/opt\/log\/app/'
```

This is difficult to read.

A better method is to use another delimiter:

```bash
echo '/var/log/app' |
sed 's#/var/log/app#/opt/log/app#'
```

You can also use `|`:

```bash
echo '/var/log/app' |
sed 's|/var/log/app|/opt/log/app|'
```

Or `@`:

```bash
echo '/var/log/app' |
sed 's@/var/log/app@/opt/log/app@'
```

These commands produce:

```text
/opt/log/app
```

### Understanding `\/`

Inside a regex:

```text
\/
```

means a literal `/` when `/` is being used as the separator.

Example:

```bash
echo 'https://server1/app' |
sed 's/https:\/\//http:\/\//'
```

Output:

```text
http://server1/app
```

A cleaner version is:

```bash
echo 'https://server1/app' |
sed 's#https://#http://#'
```

---

## 50. Advanced regex and escaping examples

### Match one or more digits

```bash
echo 'Server ID is 12345' |
sed -E 's/[0-9]+/[NUMBER]/g'
```

Output:

```text
Server ID is [NUMBER]
```

### Match an IP-address-like pattern

```bash
echo 'Server IP: 192.168.1.20' |
sed -E 's/[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+/[IP]/'
```

Output:

```text
Server IP: [IP]
```

The dot must be escaped:

```text
\.
```

because an unescaped `.` means any single character.

### Capture and reuse text

```bash
echo 'server1 production' |
sed -E 's/^([^ ]+) ([^ ]+)$/Environment: \2, Server: \1/'
```

Output:

```text
Environment: production, Server: server1
```

Explanation:

| Pattern | Meaning |
|---|---|
| `()` | Capture a group |
| `\1` | First captured group |
| `\2` | Second captured group |
| `[^ ]+` | One or more non-space characters |

### Replace repeated spaces with one space

```bash
echo 'Linux     system    administrator' |
sed -E 's/[[:space:]]+/ /g'
```

Output:

```text
Linux system administrator
```

### Print only matching lines

```bash
sed -n '/ERROR/p' application.log
```

Explanation:

```text
-n  disable normal output
p   print matching lines
```

### Print a specific line

```bash
sed -n '10p' file.txt
```

### Print a range of lines

```bash
sed -n '10,20p' file.txt
```

### Delete a specific line

```bash
sed '5d' file.txt
```

### Delete a range of lines

```bash
sed '5,10d' file.txt
```

### Insert a line before a match

```bash
sed '/server1/i\# Server configuration' configuration.conf
```

### Add a line after a match

```bash
sed '/server1/a\monitoring=enabled' configuration.conf
```

### Replace text only on matching lines

```bash
sed '/production/s/enabled/disabled/' configuration.conf
```

Only lines containing `production` are modified.

---

# Important Regex Reference

## Regex Symbols

| Regex | Meaning | Example |
|---|---|---|
| `^` | Beginning of line | `^root` |
| `$` | End of line | `bash$` |
| `.` | Any one character | `r..t` |
| `*` | Zero or more | `ab*c` |
| `+` | One or more with extended regex | `[0-9]+` |
| `?` | Zero or one with extended regex | `colou?r` |
| `[abc]` | One character from the set | `[aeiou]` |
| `[^abc]` | Any character outside the set | `[^0-9]` |
| `[0-9]` | One digit | `[0-9]` |
| `[a-z]` | Lowercase character | `[a-z]` |
| `[A-Z]` | Uppercase character | `[A-Z]` |
| `\.` | Literal dot | `192\.168` |
| `\/` | Literal slash with `/` separator | `https:\/\/` |
| `\\` | Literal backslash | `C:\\Users` |
| `()` | Capture group | `(error)` |
| `\1` | First captured group | `\1` |
| `|` | OR in extended regex | `error|warning` |
| `{3}` | Exactly three occurrences | `[0-9]{3}` |
| `{2,5}` | Between two and five | `[a-z]{2,5}` |

---

# Understanding `/`, `//`, `\/`, and `\\`

## `/pattern/`

In `awk` and `sed`, text between `/` characters is normally a regex:

```bash
awk '/error/ {print}' file.txt
```

Here, `error` is the regex pattern.

---

## Empty regex `//`

An empty regex can reuse the most recently used regular expression in some tools and contexts.

Example with `sed`:

```bash
printf '%s\n' error warning error |
sed -n '/error/p; //='
```

However, this behavior can be confusing. For beginner scripts, explicitly repeat the regex instead:

```bash
sed -n '/error/p; /error/=' file.txt
```

---

## Escaped slash `\/`

When `/` is the separator, escape a literal slash:

```bash
sed 's/\/var\/log/\/opt\/log/' file.txt
```

A more readable version is:

```bash
sed 's#/var/log#/opt/log#' file.txt
```

---

## Literal backslash `\\`

Example input:

```text
C:\Users\Admin
```

Replace backslashes with forward slashes:

```bash
echo 'C:\Users\Admin' |
sed 's#\\#/#g'
```

Output:

```text
C:/Users/Admin
```

The regex:

```text
\\
```

matches one literal backslash.

---

# Useful System Administration Pipelines

## Find the ten largest files

```bash
find /var/log -type f -printf '%s %p\n' 2>/dev/null |
sort -nr |
head -10 |
awk '{
    size=$1;
    $1="";
    print size, $0
}'
```

---

## Count users by login shell

```bash
cut -d: -f7 /etc/passwd |
sort |
uniq -c |
sort -nr
```

---

## List real users with Bash access

```bash
awk -F: '$3 >= 1000 && $7 == "/bin/bash" {
    print $1, $3, $6
}' /etc/passwd
```

---

## Find the most frequently accessed URLs

For a standard web access log where the request path is field 7:

```bash
awk '{print $7}' access.log |
sort |
uniq -c |
sort -nr |
head -10
```

---

## Find HTTP error status codes

For logs where the status code is field 9:

```bash
awk '$9 >= 400 {print $9, $7}' access.log
```

Count each status code:

```bash
awk '$9 >= 400 {print $9}' access.log |
sort |
uniq -c |
sort -nr
```

---

## Show filesystem usage above 80%

```bash
df -P |
awk 'NR > 1 {
    usage=$5;
    gsub(/%/, "", usage);
    if (usage >= 80)
        print $6, usage "%"
}'
```

---

## Show processes consuming high memory

```bash
ps aux |
awk 'NR == 1 || $4 >= 10 {print}'
```

Here, `$4` from `ps aux` is normally memory percentage.

---

## Remove comments and blank lines from a configuration file

```bash
sed -e '/^[[:space:]]*#/d' \
    -e '/^[[:space:]]*$/d' configuration.conf
```

---

## Extract failed SSH login IP addresses

```bash
grep 'Failed password' /var/log/auth.log |
awk '{for (i=1; i<=NF; i++) if ($i=="from") print $(i+1)}' |
sort |
uniq -c |
sort -nr
```

---

# `awk` Versus `cut`

Use `cut` when:

```text
The delimiter is simple and fixed.
You only need to extract fields or characters.
No conditions or calculations are required.
```

Example:

```bash
cut -d: -f1,3 /etc/passwd
```

Use `awk` when:

```text
You need conditions.
You need calculations.
Input contains varying whitespace.
You need regex matching.
You need to change the output format.
```

Example:

```bash
awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd
```

---

# `grep`, `sed`, and `awk` Comparison

| Requirement | Recommended command |
|---|---|
| Search for matching lines | `grep` |
| Replace or delete text | `sed` |
| Extract columns and apply conditions | `awk` |
| Extract fixed characters or fields | `cut` |
| Count adjacent duplicate lines | `uniq` |
| Sort data | `sort` |

---

# Practice File

Create a practice file:

```bash
cat > employees.txt <<'EOF'
1001 John IT 5000 production
1002 Mary Finance 6000 production
1003 David IT 4500 development
1004 Susan HR 5500 production
1005 Paul IT 7000 production
1006 Lisa Finance 6200 development
EOF
```

Print employee names:

```bash
awk '{print $2}' employees.txt
```

Print IT employees:

```bash
awk '$3 == "IT" {print $2, $4}' employees.txt
```

Print employees earning more than 5500:

```bash
awk '$4 > 5500 {print $2, $4}' employees.txt
```

Calculate total salary:

```bash
awk '{sum += $4} END {print "Total salary:", sum}' employees.txt
```

Count employees by department:

```bash
awk '{print $3}' employees.txt |
sort |
uniq -c
```

Replace `production` with `prod`:

```bash
sed 's/production/prod/g' employees.txt
```

Extract employee ID and name:

```bash
cut -d' ' -f1,2 employees.txt
```

Because repeated spaces can affect `cut`, `awk` is generally safer for whitespace-separated data:

```bash
awk '{print $1, $2}' employees.txt
```
