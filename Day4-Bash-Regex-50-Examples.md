# Bash Regular Expressions - 50 Common Examples

These examples use Bash regex with `[[ string =~ regex ]]`.

## Basic Syntax

```bash
if [[ "$text" =~ REGEX ]]; then
    echo "Matched"
fi
```

| # | Regex | Example | Purpose |
|---|-------|---------|---------|
| 1 | `^a` | `apple` | Starts with a |
| 2 | `z$` | `quiz` | Ends with z |
| 3 | `^.$` | `A` | Single character |
| 4 | `^[0-9]+$` | `12345` | Numbers only |
| 5 | `^[A-Za-z]+$` | `Linux` | Letters only |
| 6 | `^[A-Za-z0-9]+$` | `abc123` | Alphanumeric |
| 7 | `^[a-z]+$` | `bash` | Lowercase |
| 8 | `^[A-Z]+$` | `HELLO` | Uppercase |
| 9 | `^.{8,}$` | `password1` | Min 8 chars |
| 10 | `^...$` | `cat` | Exactly 3 chars |
| 11 | `^[0-9]{4}$` | `2026` | Exactly 4 digits |
| 12 | `^[0-9]{2,4}$` | `123` | 2-4 digits |
| 13 | `^root$` | `root` | Exact word |
| 14 | `dev` | `devops` | Contains dev |
| 15 | `.*log$` | `syslog` | Ends with log |
| 16 | `^/home/` | `/home/user` | Starts /home |
| 17 | `^/var/log` | `/var/log/messages` | Path |
| 18 | `\.txt$` | `file.txt` | .txt file |
| 19 | `\.sh$` | `backup.sh` | .sh file |
| 20 | `^[^0-9]+$` | `Linux` | No digits |
| 21 | `[0-9]` | `abc1` | Contains digit |
| 22 | `[A-Z]` | `abcD` | Contains uppercase |
| 23 | `[a-z]` | `ABCd` | Contains lowercase |
| 24 | `[aeiou]` | `linux` | Contains vowel |
| 25 | `[^aeiou]` | `linux` | Contains non-vowel |
| 26 | `^[^ ]+$` | `hello` | No spaces |
| 27 | ` ` | `hello world` | Contains space |
| 28 | `^#` | `#!/bin/bash` | Comment/shebang |
| 29 | `^192\.168\.` | `192.168.1.5` | Private IP |
| 30 | `^[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+$` | `10.1.1.1` | IPv4 format |
| 31 | `^[a-z0-9._%+-]+@` | `user@test.com` | Email prefix |
| 32 | `@.*\.` | `user@test.com` | Email domain |
| 33 | `^https?://` | `https://site` | HTTP/HTTPS |
| 34 | `^ftp://` | `ftp://server` | FTP |
| 35 | `^www\.` | `www.google.com` | WWW |
| 36 | `^kube` | `kube1` | Hostname |
| 37 | `[13579]$` | `123` | Odd ending |
| 38 | `[02468]$` | `124` | Even ending |
| 39 | `^.{1,5}$` | `test` | Length<=5 |
| 40 | `^.{6,10}$` | `testing` | Length6-10 |
| 41 | `error` | `error found` | Contains error |
| 42 | `failed` | `task failed` | Contains failed |
| 43 | `success` | `build success` | Contains success |
| 44 | `^yes|no$` | `yes` | Yes/no |
| 45 | `^[ynYN]$` | `Y` | Y/N |
| 46 | `^[01]$` | `1` | Binary digit |
| 47 | `^[01]+$` | `10101` | Binary number |
| 48 | `^[0-7]+$` | `755` | Octal |
| 49 | `^[0-9A-Fa-f]+$` | `FFAA10` | Hex |
| 50 | `^$` | `` | Empty string |


## Practical Bash Example

```bash
read -p "Enter username: " user
if [[ "$user" =~ ^[a-z][a-z0-9_]{2,15}$ ]]; then
    echo "Valid username"
else
    echo "Invalid username"
fi
```

## Common Operators

- `^` Start of string
- `$` End of string
- `.` Any character
- `*` Zero or more
- `+` One or more
- `?` Zero or one
- `[]` Character class
- `[^]` Negated class
- `{n}` Exact count
- `{m,n}` Range
