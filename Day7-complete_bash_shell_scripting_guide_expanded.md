# The Complete Bash Shell Scripting Guide
**A Professional Guide from Zero to Enterprise Automation**

For Linux Administrators • DevOps Engineers • Cloud Engineers • SOC Analysts

Source author/profile: `linkedin.com/in/mudassir-hussain-malik`

---

## Topic Map

- **Automation:** Task Automation, System Administration, Process Management
- **Cyber Security:** Log Analysis, IOC Extraction, Incident Response, Threat Hunting
- **Cloud & DevOps:** AWS / Azure CLI, Docker & Kubernetes, CI/CD Automation
- **Bash Basics:** Variables & Types, Operators & Loops, Functions & Arrays, Input / Output
- **Linux Admin:** Users & Permissions, Services & Processes, Cron Jobs & Logs, Networking
- **Scripting Power:** Reusable Scripts, Error Handling, Debugging, Best Practice
- **Workflow:** Terminal → Scripts → Automate → Secure → Deploy → Scale

### How to Use This Handbook

This handbook is organized into 30 parts, progressing from absolute Bash fundamentals
through Linux administration, cybersecurity automation, DevOps and cloud scripting, and
professional-grade defensive coding practices. Each chapter follows a consistent teaching
structure — learning objectives, core concept, syntax, practical examples across
Linux/DevOps/Cloud/SOC contexts, best practices, common mistakes, a chapter
summary, practice questions, and a hands-on exercise — so you can either read cover-to-
cover or jump directly to the topic you need.

Callout boxes are used consistently throughout:

- 💡TIP — a shortcut or efficiency gain

- ✅BEST PRACTICE — the professionally recommended approach

- ⚠COMMON MISTAKE — a frequent beginner error to avoid

- 📌IMPORTANT NOTE — context worth remembering

- 🎯INTERVIEW TIP — relevant to technical interviews and certification exams

- 🔐SECURITY NOTE — a security-specific warning or consideration

- ☁CLOUD NOTE — relevant to cloud platforms and services

- 🏢ENTERPRISE NOTE — relevant to team scale and production environments

### TABLE OF CONTENTS

Serial No.
Section / Chapter Name
Page No.

01
Linux Shells & Bash Architecture
4

02
Scripts, Variables, Input & Operators
12

03
Quoting, Environment Variables & Conditionals
19

04
Loops, Flow Control & Exit Codes
26

05
Strings & Arrays
33

06
Functions & File Handling
39

07
Command-Line Arguments & Here Documents
47

08
Users & Permissions
54

09
Processes, Services & System Automation
57

10
Cron Jobs, Logs & Networking
61

11
SSH Automation
67

12
Log Analysis & IOC Extraction
71

13
Security Automation & Incident Response
74

14
File Integrity Monitoring & Threat Hunting
77

15
Docker & Kubernetes Automation
81

16
CI/CD, AWS & Azure CLI Automation
84

17
Backup Automation
88

18
Defensive Coding, Error Handling & Debugging
91

19
Performance, Logging, Templates & Bash Style Guide
95

20
Enterprise Folder Structure, Interview Questions &
Cheat Sheet

100

### Introduction to Linux Shells

What a shell is, why Bash dominates, and where you'll use it

### Learning Objectives

- Explain what a shell is and how it differs from the kernel
- Name the most common Linux shells and how Bash compares to them
- Understand why Bash is the default automation language across Linux, DevOps,
Cloud, and SOC work
- Open a terminal and run your first interactive commands

### Introduction

A shell is a program that sits between you and the Linux kernel. You type a command, the
shell interprets it, and it asks the kernel to actually do the work — start a process, open a
file, send data over the network. Without a shell, you would have to talk to the kernel
directly through system calls, which is not something a human types by hand.

Bash (Bourne Again SHell) is both an interactive command interpreter and a full scripting
language. It is the default shell on almost every Linux distribution, on macOS (for older
versions), and is available on Windows via WSL. That ubiquity is exactly why professionals
learn it: a Bash script written today will still run on a server, container, or cloud VM a
decade from now.

### 📌IMPORTANT NOTE

Analogy

Think of the kernel as the engine of a car and the shell as the steering wheel and
pedals. The engine does the actual work, but you never touch it directly — you interact
with it through an interface built for humans.

### Core Concept

Every Linux system has one or more shells installed, listed in /etc/shells. When you log
in, the system starts your configured login shell, which then waits for you to type
commands. Each command you enter is parsed, expanded, and executed as a separate
process (or a shell built-in, executed without spawning a new process).

Figure 1.1 – Shell vs Kernel Architecture

Shell
Notes

sh (POSIX shell)
The original, minimal standard; scripts written for sh are maximally
portable

bash
Default on most Linux distros; superset of sh with arrays, [[ ]],
functions, and more

zsh
Popular interactive shell (default on macOS); highly configurable,
mostly Bash-compatible

dash
A fast, minimal POSIX shell often used as /bin/sh on Debian/Ubuntu
for system scripts

fish
Friendly interactive shell with different (non-POSIX) scripting syntax

### Why Bash Instead of a 'Real' Programming Language?

Python or Go can do almost everything Bash can, but Bash has one advantage nothing
else has: it is always there. Every Linux server, every container base image, every cloud
shell, and every CI/CD runner ships with Bash (or something close to it) pre-installed. For

gluing together existing command-line tools — the actual day-to-day work of Linux
administration, DevOps pipelines, and SOC triage — Bash is faster to write and requires
no dependencies to install.

✅ BEST PRACTICE

Reach for Bash when you are orchestrating existing commands (systemctl, grep, curl,
aws, kubectl). Reach for Python when you need complex data structures, real error
handling, or portability beyond Linux/macOS.

### Where You Will Use Bash

- Linux Administration – user management, backups, service control, log rotation

- DevOps / CI-CD – build scripts, deployment pipelines, container entrypoints

- Cloud Engineering – AWS CLI / Azure CLI automation, infrastructure bootstrap

scripts

- SOC / Cybersecurity – log parsing, IOC extraction, incident response automation,

threat hunting helpers

### Practical Examples

### Real Linux Example

```bash
# Check which shell you are currently running
echo "$SHELL"
```

```bash
# List every shell registered on this system
cat /etc/shells
```

### SOC / Cybersecurity Example

```bash
# A SOC analyst's very first Bash one-liner on a suspicious box:
# confirm which shell an attacker's cron entry or script would execute under
ps -p $$ -o comm=
```

### 🎯INTERVIEW TIP

"What is the difference between a shell and a terminal?" — A terminal (terminal
emulator) is the window/program that displays text input and output. A shell is the
program running inside that terminal that actually interprets your commands. You
could run Bash, Zsh, or Python's REPL inside the very same terminal window.

### Chapter Summary

- A shell interprets commands and asks the kernel to execute them; it is not the

kernel itself.

- Bash is the default, near-universal shell across Linux, DevOps, Cloud, and SOC

environments.

- Bash's advantage over full programming languages is ubiquity and speed for

orchestrating existing tools.

### Practice Questions

1. What command shows you your current default shell?

2. Name two shells other than Bash and one key difference each has from Bash.

3. Why is Bash still relevant when languages like Python exist?

### HANDS-ON EXERCISE

Open a terminal, run echo $SHELL and cat /etc/shells. Write down which shell you
are using and how many shells are installed on your system.

### Bash Architecture, Installing & Running Bash

How Bash processes a command line, and every way to execute a script

### Learning Objectives

- Describe the stages Bash goes through to turn a typed line into a running process
- Check, install, and update Bash on major Linux distributions
- Understand the difference between interactive, login, and non-interactive shells
- Run a script four different ways and know when to use each

### Introduction

Before writing scripts, it helps to understand what actually happens between you pressing
Enter and a command running. Bash does not simply pass your text straight to the kernel
— it runs it through several well-defined stages, and understanding those stages explains
a huge share of 'weird' Bash behavior beginners run into (word splitting, globbing, quoting
surprises).

### Core Concept — The Bash Execution Pipeline

- Tokenization – the line is split into words based on unquoted whitespace

- Expansion – variables ($VAR), commands ($(...)), arithmetic ($((...))), tilde (~), and

braces ({a,b}) are expanded

- Word splitting & globbing – unquoted expansions are split on IFS and wildcard

patterns (*.log) are expanded to matching filenames

- Command lookup – Bash checks aliases, functions, built-ins, then $PATH, in that

order

- Execution – the resolved command runs, either as a built-in (no new process) or a

forked child process

Fig 2.1 Insert Bash Execution Flow Diagram

### 📌 IMPORTANT NOTE

This is exactly why unquoted variables are dangerous: rm $file will word-split and
glob-expand $file before rm ever sees it. Quoting (rm "$file") skips word splitting and
globbing entirely.

### Installing & Checking Bash

```bash
# Check installed version
bash --version

# Debian / Ubuntu
sudo apt update && sudo apt install -y bash

# RHEL / CentOS / Fedora
sudo dnf install -y bash

# macOS (Homebrew — installs a modern Bash 5.x; macOS ships an ancient 3.2)
brew install bash
```

### ☁CLOUD NOTE

Most container base images (alpine, debian-slim) either omit Bash entirely (Alpine uses
ash/BusyBox sh by default) or ship an older release. Always confirm bash --version
inside your Dockerfile before relying on Bash-only syntax like arrays or [[ ]].

### Interactive vs Login vs Non-Interactive Shells

Shell Type
When It Happens
Config Files Read

Login shell
SSH login, console login, bash -l
/etc/profile, ~/.bash_profile

Interactive non-
login
Opening a new terminal window ~/.bashrc

Non-interactive
Running a script or cron job
$BASH_ENV (if set), no rc files
by default

### Practical Examples — Four Ways to Run a Script

```bash
# 1. Execute directly (requires execute permission + shebang line)
chmod +x deploy.sh
./deploy.sh
```

```bash
# 2. Explicitly invoke the interpreter (execute permission not required)
bash deploy.sh
```

```bash
# 3. Source it into your CURRENT shell (variables/functions persist after)
source deploy.sh
. deploy.sh          # '.' is the POSIX-standard equivalent of 'source'
```

```bash
# 4. Run as a one-off background job
bash deploy.sh &
```

### ⚠COMMON MISTAKE

Confusing ./script.sh with source script.sh. Running with ./ starts a brand-new
subshell — any cd or exported variable inside the script disappears when it finishes.
source-ing runs the script in your CURRENT shell, so a script meant to set up your
environment (like a virtualenv activator) MUST be sourced, not executed.

### DevOps Example

```bash
#!/usr/bin/env bash
# Portable shebang — finds bash via $PATH rather than hardcoding /bin/bash,
# important because some CI runners install bash in non-standard locations.
set -euo pipefail
echo "Deploying build ${BUILD_NUMBER:-local} ..."
```

### ✅BEST PRACTICE

Prefer #!/usr/bin/env bash over #!/bin/bash in scripts meant to run across different
environments (macOS, containers, CI runners) — env resolves bash from $PATH instead
of assuming a fixed path.

### Chapter Summary

- Bash processes a line through tokenization, expansion, word splitting/globbing,

and execution — in that order.

- Interactive, login, and non-interactive shells read different startup files; cron jobs

get almost none of them.

- There are four ways to run a script: execute, invoke the interpreter, source, or

background — each behaves differently regarding environment persistence.

### Practice Questions

1. Which shell type reads ~/.bashrc?

2. Why does source matter for scripts that set environment variables?

3. Why is #!/usr/bin/env bash often preferred over #!/bin/bash?

### HANDS-ON EXERCISE

Write a one-line script that does export DEMO_VAR=hello. Run it once with ./script.sh
and once with source script.sh, then check echo $DEMO_VAR after each. Explain the
difference you observe.

### Your First Script & Variables

From an empty file to a working script that stores and reuses data

### Learning Objectives

- Write, save, and execute a complete Bash script from scratch
- Declare, assign, and expand variables correctly
- Understand the difference between local shell variables and exported environment
variables
- Avoid the most common variable-naming and spacing mistakes

### Introduction

A script is simply a text file containing a sequence of commands, run in order, exactly as
if you had typed them one by one at the prompt. What turns a plain text file into a Bash
script is the shebang line at the top, which tells the operating system which interpreter
to hand the rest of the file to.

### Core Concept — Anatomy of a Script

```bash
#!/usr/bin/env bash
#
# script:  hello.sh
# purpose: prints a greeting to the terminal
# author:  you
```

```bash
echo "Hello, SOC analyst!"
```

### 📌IMPORTANT NOTE

The shebang (#!) MUST be the very first two characters of the file — not even a blank
line may come before it, or the kernel will fail to recognize it as an interpreter directive.

### Variables — Syntax

Bash variables have no type declaration and no spaces around the equals sign.
Assignment and expansion use different syntax on purpose: you assign with a bare name,
and you read with a dollar sign.

```bash
name="Alice"        # assignment — NO spaces around =
echo "$name"         # expansion — always prefer "$name" over $name
echo "${name}Suffix" # braces disambiguate where the name ends
```

⚠ COMMON MISTAKE

name = "Alice" (with spaces) is NOT an assignment in Bash — it is parsed as running a
command called name with arguments = and Alice, producing a 'command not found'
error. Zero spaces around = is mandatory.

Variable Scope: Local vs Environment

Declaration
Visible To

var=value
The current shell only

export var=value
The current shell AND any child processes/scripts it launches

local var=value
Only inside the function it was declared in

readonly var=value
Current shell; cannot be reassigned afterward

### Practical Examples

### Real Linux Example

```bash
#!/usr/bin/env bash
hostname_var=$(hostname)
uptime_var=$(uptime -p)
echo "Host $hostname_var has been up: $uptime_var"
```

### DevOps Example

```bash
#!/usr/bin/env bash
export ENVIRONMENT="staging"
export BUILD_ID="1042"
# Any script called from here inherits ENVIRONMENT and BUILD_ID automatically
bash ./deploy.sh
```

### SOC / Cybersecurity Example

```bash
#!/usr/bin/env bash
# Store today's date once, reuse it for a consistent log filename
today=$(date +%F)
logfile="/var/log/soc/triage-${today}.log"
echo "Writing findings to $logfile"
```

### ✅BEST PRACTICE

Always wrap variable expansions in double quotes — "$var" — unless you specifically
need word splitting or globbing. This single habit prevents the majority of real-world
Bash bugs.

### 🎯INTERVIEW TIP

"What happens if you reference a variable that was never set?" — Bash expands it to an
empty string, silently, with no error — UNLESS set -u (nounset) is active, in which case
it exits with an 'unbound variable' error. Professional scripts almost always enable set -
u.

### Chapter Summary

- A script needs a shebang line as its very first line to be recognized as executable

by that interpreter.

- Assignment (var=value) has zero spaces; expansion always uses a leading $ and

should be quoted.

- export makes a variable visible to child processes; plain assignment keeps it local

to the current shell.

### Practice Questions

1. Why does x = 5 fail in Bash while x=5 succeeds?

2. What is the difference between a local variable and an exported variable?

3. What does set -u do, and why is it considered a best practice?

### HANDS-ON EXERCISE

Write a script sysinfo.sh that stores the hostname, current date, and logged-in
username in three variables and prints them as a single formatted status line.

### Input, Output, Comments & Operators Talking to the user, redirecting streams, and doing arithmetic

### Learning Objectives

- Read user input interactively with read
- Redirect standard output, standard error, and standard input
- Write clear comments that document intent, not syntax
- Use arithmetic, comparison, and logical operators correctly

### Introduction

Every process on Linux has three standard streams: stdin (0) for input, stdout (1) for
normal output, and stderr (2) for error output. Understanding these streams — and how
to redirect them — is essential for writing scripts that behave well in pipelines, cron jobs,
and CI/CD logs.

### Reading Input

```bash
#!/usr/bin/env bash
read -p "Enter target hostname: " host
read -s -p "Enter password (hidden): " pass; echo
echo "Connecting to $host..."
```

Flag
Meaning

-p "prompt"
Show a prompt before reading

-s
Silent mode — do not echo typed characters (passwords)

-a arr
Read words into an array

-t N
Timeout after N seconds

-r
Raw mode — do not interpret backslashes

### Redirection Operators

Operator
Effect

cmd > file
Redirect stdout to file (overwrite)

cmd >> file
Redirect stdout to file (append)

cmd 2> file
Redirect stderr only

cmd &> file
Redirect BOTH stdout and stderr

cmd 2>&1
Merge stderr into wherever stdout is going

cmd < file
Feed file contents to stdin

cmd1 | cmd2
Pipe stdout of cmd1 into stdin of cmd2

⚠ COMMON MISTAKE

Order matters: cmd > out.log 2>&1 correctly merges both streams into out.log. But cmd
2>&1 > out.log does NOT — at the moment 2>&1 runs, stdout is still the terminal, so
stderr goes to the terminal and only stdout goes to the file.

### Comments

Bash comments start with # and run to the end of the line. A good comment explains why,
not what — the code already shows what it does.

```bash
# BAD: restates the obvious
# increment counter
counter=$((counter + 1))
```

```bash
# GOOD: explains the reasoning
# retry counter — we allow 3 attempts before alerting the SOC on-call
counter=$((counter + 1))
```

### Operators

### Arithmetic

```bash
total=$(( 5 + 3 * 2 ))
((total++))
echo "$total"
```

Operator
Meaning

+  -  *  /  %
Add, subtract, multiply, divide, modulo

^
Exponent

++  --
Increment / decrement

+=  -=  *=  /=
Compound assignment

### Comparison (inside [[ ]] / test)

Numeric
String
Meaning

-eq
==
Equal

-ne
!=
Not equal

-lt
<
Less than

-gt
>
Greater than

-le / -ge
n/a
Less-or-equal / greater-or-equal

### Practical Examples

### DevOps Example

```bash
#!/usr/bin/env bash
# Run a build, capture failures without losing successful output
./build.sh > build.log 2>&1
if [[ $? -ne 0 ]]; then
  echo "Build failed — see build.log" >&2
  exit 1
fi
```

### SOC / Cybersecurity Example

```bash
#!/usr/bin/env bash
# Redirect noisy stdout to a scan log, keep errors visible on screen for triage
nmap -sV 10.0.0.0/24 > scan_results.log 2> >(tee scan_errors.log >&2)
```

✅ BEST PRACTICE

In production scripts, always send diagnostic/error messages to stderr (>&2), never
stdout. This keeps stdout clean for piping into other tools and lets log collectors
separate errors automatically.

### 🎯 INTERVIEW TIP

"What file descriptor number is stderr?" — File descriptor 2. stdin is 0, stdout is 1, stderr
is 2. This is worth memorizing cold — it comes up constantly in real troubleshooting.

### Chapter Summary

- Every process has three standard streams — stdin (0), stdout (1), stderr (2) — and

each can be redirected independently.

- read is the standard way to gather interactive input, including hidden password

entry with -s.

- Comments should explain intent; arithmetic uses $(( )), comparisons use -eq/-

lt/etc. for numbers and ==/!= for strings.

### Practice Questions

1. What is the difference between 2>&1 > file and > file 2>&1?

2. Which redirection operator appends instead of overwriting?

3. Why should error messages be sent to stderr instead of stdout?

### HANDS-ON EXERCISE

Write a script that prompts for a number, doubles it using arithmetic expansion, and
prints an error to stderr (with a non-zero exit) if the input was not a valid integer.

### Quoting & Environment Variables Controlling expansion precisely, and understanding the shell's environment

### Learning Objectives

- Explain the difference between single quotes, double quotes, and no quotes
- Use escaping correctly for special characters
- List and use the most important built-in environment variables
- Set environment variables that persist across sessions

### Introduction

Quoting is one of the most misunderstood parts of Bash — and one of the most important
for security. Getting it wrong doesn't just cause bugs; unquoted variables are a classic
source of command injection vulnerabilities in automation scripts.

### Core Concept — The Three Quoting Styles

Style
Variables Expand?
Globs Expand?
Use For

No quotes
Yes
Yes
Rarely — only when you
want splitting/globbing

"Double quotes"
Yes
No
The default choice for
almost everything

'Single quotes'
No
No
Literal text, regex
patterns, passwords

```bash
file="*.log"
echo $file      # unquoted: expands wildcard, may print multiple filenames
echo "$file"    # double-quoted: prints literally '*.log' as one string
echo '$file'    # single-quoted: prints literally '$file' — no expansion at all
```

### 🔐SECURITY NOTE

eval "rm -rf $userInput" with an unquoted, unsanitized variable is a textbook
command-injection vulnerability. If userInput is ; rm -rf / #, the consequences are
catastrophic. Always quote, and avoid eval on untrusted input entirely.

### Escaping

```bash
echo "She said \"hello\""     # backslash escapes a double quote inside double
quotes
echo 'It'\''s fine'             # escaping a single quote inside single quotes
(the classic trick)
price="\$5.00"                  # escape $ so it is not treated as a variable
sigil
```

### Environment Variables

The environment is a set of exported name=value pairs that every child process inherits.
Bash pre-populates dozens of them at login.

Variable
Meaning

$HOME
Current user's home directory

$PATH
Colon-separated list of directories searched for commands

$USER
Current username

$PWD / $OLDPWD
Current / previous working directory

$SHELL
Path to the user's login shell

$HOSTNAME
System hostname

$?
Exit status of the last command

$$
PID of the current shell

$0, $1, $2...
Script name, then positional arguments

### Practical Examples

### Real Linux Example

```bash
# Persist a custom PATH addition for future sessions
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Cloud Example

```bash
# Cloud CLIs read credentials from the environment — never hardcode them in
scripts
export AWS_ACCESS_KEY_ID="AKIA..."
export AWS_SECRET_ACCESS_KEY="..."
export AWS_DEFAULT_REGION="us-east-1"
aws s3 ls
```

### 🔐SECURITY NOTE

Never commit scripts containing hardcoded secrets in export statements. Use a secrets
manager (AWS Secrets Manager, Azure Key Vault, HashiCorp Vault) or a gitignored .env
file loaded at runtime instead.

### SOC / Cybersecurity Example

```bash
# Attackers frequently manipulate PATH to hijack command execution.
# A defensive one-liner to audit an unusual PATH:
echo "$PATH" | tr ':' '\n' | while read -r dir; do
  [[ -w "$dir" ]] && echo "WARNING: world/user-writable PATH entry: $dir"
done
```

### 🎯INTERVIEW TIP

"Why is single-quoting safer for regex patterns?" — Because single quotes prevent the
shell from expanding $, *, and backticks before the pattern ever reaches grep/sed/awk
— so the regex engine, not Bash, controls the interpretation of those special characters.

### Chapter Summary

- Double quotes allow variable expansion but block word splitting/globbing; single

quotes block everything; no quotes is dangerous by default.

- Unquoted variables are a common root cause of both bugs and security

vulnerabilities in shell scripts.

- The environment is the inherited set of exported variables every child process

receives, including $PATH, $HOME, and $USER.

### Practice Questions

1. What is the output of echo '$HOME' versus echo "$HOME"?

2. Why is an unquoted variable in a command a potential security risk?

3. Name three environment variables Bash sets automatically at login.

### HANDS-ON EXERCISE

Create a variable containing a filename with a space in it (e.g. 'my file.txt'). Try ls $var
versus ls "$var" against a real file with that name and observe the difference.

## If Statements & Case Statements Making decisions in a script

### Learning Objectives

- Write if / elif / else chains with correct syntax
- Understand the difference between [ ], [[ ]], and (( )) test constructs
- Use file test operators to check for existence, type, and permissions
- Replace long if/elif chains with a cleaner case statement

### Introduction

Conditional logic is what turns a script from a fixed sequence of commands into
something that reacts to its environment — a missing file, a failed command, a specific
argument. Bash offers three test mechanisms; knowing when to use each avoids a huge
share of beginner bugs.

### Syntax

```bash
if [[ condition ]]; then
  # runs if condition is true
elif [[ other_condition ]]; then
  # runs if the first was false and this is true
else
  # runs if nothing above matched
fi
```

Construct
Use For
Notes

[ expr ]
POSIX test, portable to /bin/sh Requires spaces; word-splits

unquoted variables

[[ expr ]]
Bash's improved test
Safer — no word splitting; supports
=~ regex, && / ||

(( expr ))
Arithmetic conditions
Use for numeric comparisons: ((
count > 5 ))

### ✅BEST PRACTICE

In Bash scripts (not portable /bin/sh scripts), always prefer [[ ]] over [ ]. It is safer with
unquoted variables, supports pattern matching and regex, and its && / || read more
naturally than -a / -o.

### File Test Operators

Operator
True When

-e file
File exists (any type)

-f file
Exists and is a regular file

-d file
Exists and is a directory

-L file
Exists and is a symbolic link

-r / -w / -x
File is readable / writable / executable

-s file
File exists and is non-empty

file1 -nt file2 file1 is newer than file2

### Practical Examples

### Real Linux Example

```bash
#!/usr/bin/env bash
config="/etc/myapp/config.yml"
if [[ -f "$config" && -r "$config" ]]; then
  echo "Config found and readable."
elif [[ -e "$config" ]]; then
  echo "Config exists but is NOT readable — check permissions." >&2
else
  echo "Config missing — creating default."
  touch "$config"
fi
```

### DevOps Example

```bash
#!/usr/bin/env bash
if [[ "$CI" == "true" ]]; then
  echo "Running inside CI — skipping interactive prompts."
else
  read -p "Deploy to production? (y/N) " ans
  [[ "$ans" =~ ^[Yy]$ ]] || exit 0
fi
```

### SOC / Cybersecurity Example

```bash
#!/usr/bin/env bash
# Flag world-writable files — a common misconfiguration finding
perm=$(stat -c '%a' "$1")
if [[ "${perm: -1}" =~ [2367] ]]; then
  echo "ALERT: $1 is world-writable (mode $perm)"
fi
```

### Case Statements

When an if/elif chain starts comparing the same variable against many possible values, a

case statement is clearer, faster to scan, and supports glob-style pattern matching
natively.

```bash
#!/usr/bin/env bash
read -p "Action (start|stop|restart|status): " action
case "$action" in
  start)           echo "Starting service..." ;;
  stop)            echo "Stopping service..." ;;
  restart)         echo "Restarting service..." ;;
  status)          echo "Checking status..." ;;
  *.log|*.txt)     echo "Looks like a log file, not an action" ;;
  *)               echo "Unknown action: $action" >&2; exit 1 ;;
esac
```

### ⚠COMMON MISTAKE

Forgetting the double semicolon ;; at the end of each case branch is one of the most
common beginner syntax errors — without it, Bash keeps falling through to the next
pattern.

### 🎯INTERVIEW TIP

"When would you use case instead of if/elif?" — When testing one variable against
many discrete values or glob patterns. case is more readable, and unlike a chain of elif,
each branch is visually self-contained.

### Chapter Summary

- if/elif/else drives branching logic; [[ ]] is the safer, Bash-native choice over [ ] for

conditions.

- File test operators (-e, -f, -d, -r, -w, -x, -s) are essential for scripts that touch the

filesystem.

- case statements replace long elif chains when comparing one variable against

many values or patterns.

### Practice Questions

1. What does -s file check, as opposed to -e file?

2. Why is [[ ]] generally preferred over [ ] in Bash-specific scripts?

3. What syntax error is most commonly made when writing a case statement?

### HANDS-ON EXERCISE

Write a script that reads a filename as an argument ($1) and uses if/elif to report
whether it's a regular file, a directory, a symlink, or doesn't exist at all.

## Loops, Break & Continue Repeating work: for, while, until, and controlling their flow

### Learning Objectives

- Choose between for, while, and until loops correctly
- Iterate over lists, ranges, files, and command output safely
- Use break and continue to control loop flow
- Avoid the classic 'reading a file with a for loop' pitfall

### Introduction

Loops are the backbone of automation — processing every log file in a directory, retrying
a failed connection, iterating over every host in an inventory. Bash gives you three loop
types; each fits a different shape of problem.

### Syntax

```bash
# for — iterate over a known list
for item in one two three; do
  echo "$item"
done
```

```bash
# while — repeat as long as a condition is true
while [[ condition ]]; do
  ...
done
```

```bash
# until — repeat as long as a condition is FALSE (opposite of while)
until [[ condition ]]; do
  ...
done
```

### Practical Examples

### Real Linux Example — C-style for loop

```bash
for (( i=1; i<=5; i++ )); do
  echo "Attempt $i"
done
```

### Real Linux Example — iterate over files safely

```bash
for file in /var/log/*.log; do
  [[ -e "$file" ]] || continue   # handle 'no matches' case
  echo "Processing $file"
done
```

### ⚠COMMON MISTAKE

for line in $(cat file.txt) looks tempting but is broken — it word-splits on
whitespace, not lines, and mangles any filename or log line containing spaces. Use while
read -r line; do ... done < file.txt instead.

### DevOps Example — retry with a while loop

```bash
attempt=0
max=5
until curl -sf https://api.internal/health; do
  ((attempt++))
  if (( attempt >= max )); then
    echo "Health check failed after $max attempts" >&2
    exit 1
  fi
  echo "Retry $attempt/$max..."
  sleep 5
done
```

### SOC / Cybersecurity Example — reading a file line by line

```bash
while IFS= read -r ip; do
  if ! ping -c1 -W1 "$ip" &>/dev/null; then
    echo "UNREACHABLE: $ip"
  fi
done < suspect_ips.txt
```

### ✅BEST PRACTICE

while IFS= read -r line; do ... done < file is THE correct, idiomatic way to
process a file line-by-line in Bash. IFS= preserves leading/trailing whitespace, and -r
prevents backslash interpretation.

### break and continue

```bash
for host in "${hosts[@]}"; do
  if [[ "$host" == "skip-me" ]]; then
    continue          # skip this iteration, move to the next host
  fi
  if ! ping -c1 -W1 "$host" &>/dev/null; then
    echo "$host is down — stopping scan"
    break              # exit the loop entirely
  fi
done
```

Keyword
Effect

break
Exit the loop immediately

break N
Exit N levels of nested loops

continue
Skip to the next iteration

continue N
Skip to the next iteration of the Nth enclosing loop

### 🎯INTERVIEW TIP

"How do you read a file line by line without a subshell losing your variables?" — Use
while read -r line; do ... done < file (redirection) rather than piping cat file |
while read ..., because a pipe runs the while loop in a subshell, so any variables set
inside it vanish once the loop ends.

### Chapter Summary

- for loops fit known lists or ranges; while loops fit condition-driven repetition; until

is while's inverse.

```bash
● Always use while IFS= read -r line; do ... done < file to process files line
```

by line, never a for loop over $(cat file).

- break and continue (with optional level numbers) give fine control over nested

loop flow.

### Practice Questions

1. Why is for line in $(cat file) considered broken for most real files?

2. What's the difference between while and until?

3. How would you break out of two nested loops at once?

### HANDS-ON EXERCISE

Write a script that reads a list of URLs from a file (one per line) and uses curl to check
each one, printing OK or FAILED next to each, retrying failed ones up to 3 times before
giving up.

## Exit Codes How Bash communicates success and failure between commands and scripts

### Learning Objectives

- Explain what an exit code is and its valid range
- Read $? correctly, immediately after the command it refers to
- Set explicit, meaningful exit codes in your own scripts
- Use exit codes to control automation pipelines and alerting logic

### Introduction

Every command that finishes on Linux returns a numeric exit code (also called exit status
or return code) to the process that launched it. This is how a script — or a human — knows
whether the previous command actually succeeded, independent of whatever text it
printed.

### Core Concept

Exit codes range from 0 to 255. Zero always means success. Any non-zero value means
failure, and by convention different non-zero values often signal different failure reasons,
though there's no universal enforced standard beyond a handful of shell conventions.

Code
Common Meaning

0
Success

1
General/catch-all error

2
Misuse of shell built-in / incorrect usage

126
Command found but not executable (permission issue)

127
Command not found

128+N
Process terminated by signal N (e.g. 130 = 128+2 = killed by
Ctrl+C/SIGINT)

### Syntax

```bash
ls /nonexistent-folder
echo $?              # prints 2 — 'ls' failed
```

```bash
# inside a script, set your own exit code:
exit 0    # success
exit 1    # generic failure
```

⚠ COMMON MISTAKE

$? only reflects the MOST RECENT command. If you run other commands (even echo)
between the one you care about and checking $?, you've overwritten it. Capture it into
a variable immediately: rc=$?.

### Practical Examples

### Real Linux Example

```bash
#!/usr/bin/env bash
cp /etc/important.conf /backup/
rc=$?
if [[ $rc -ne 0 ]]; then
  echo "Backup failed with exit code $rc" >&2
  exit "$rc"
fi
echo "Backup succeeded."
```

### DevOps Example

```bash
#!/usr/bin/env bash
# CI pipelines read the script's own exit code to decide pass/fail
./run_tests.sh
if [[ $? -ne 0 ]]; then
  echo "Tests failed — blocking merge" >&2
  exit 1
fi
echo "All tests passed"
exit 0
```

### SOC / Cybersecurity Example

```bash
#!/usr/bin/env bash
# Use distinct exit codes so a monitoring system can distinguish failure types
EXIT_OK=0
EXIT_HOST_DOWN=10
EXIT_AUTH_FAILED=11
```

```bash
ssh -o BatchMode=yes -o ConnectTimeout=5 "$1" true
case $? in
  0)   exit $EXIT_OK ;;
  255) echo "Auth or connection failure" >&2; exit $EXIT_AUTH_FAILED ;;
  *)   echo "Host unreachable" >&2; exit $EXIT_HOST_DOWN ;;
esac
```

### ✅BEST PRACTICE

Define named constants for custom exit codes (EXIT_HOST_DOWN=10, etc.) at the top
of a script. It turns exit 10 from a mystery number into self-documenting code, and lets
monitoring/alerting systems branch on specific failure types.

### 📌IMPORTANT NOTE

In a pipeline (cmd1 | cmd2), $? reflects only the LAST command's exit code by default.
Set set -o pipefail if you need the pipeline to fail when ANY command in it fails, not
just the last one.

### 🎯INTERVIEW TIP

"What exit code does a script return if it doesn't explicitly call exit?" — The exit code of
the LAST command executed in the script. This is why professional scripts end with an
explicit exit 0 (or an appropriate code) rather than relying on whatever happened to
run last.

### Chapter Summary

- Exit codes range 0–255; 0 means success, anything else means some form of

failure.

- $? must be captured immediately after the command you care about — it gets

overwritten by every subsequent command.

- set -o pipefail makes a pipeline fail if any stage fails, not just the last one.

### Practice Questions

1. What exit code does 'command not found' typically produce?

2. Why must you capture $? into a variable right away instead of checking it later?

3. What does set -o pipefail change about how $? behaves in a pipeline?

### HANDS-ON EXERCISE

Write a script that attempts to curl a URL passed as $1. Exit 0 on HTTP success, exit 2 if
the host can't be resolved, and exit 3 for any other curl failure, using curl's own exit
code to decide.

## Strings Slicing, searching, and transforming text — Bash's bread and butter

### Learning Objectives

- Use built-in parameter expansion for length, substring, and replacement operations
- Test string content with pattern matching and regex
- Change case and trim whitespace without calling external tools
- Choose between Bash built-ins and awk/sed/grep for text tasks

### Introduction

Because so much of Linux, DevOps, and SOC work is really about parsing text — log lines,
command output, file paths — Bash includes a surprisingly capable set of built-in string
operations. Using them instead of spawning external processes (like calling echo | awk)
is both faster and simpler for small tasks.

### Syntax — Parameter Expansion Cheat Sheet

Expression
Result

${#str}
Length of str

${str:5}
Substring starting at index 5

${str:5:3}
Substring: 3 characters starting at index 5

${str/foo/bar}
Replace FIRST occurrence of foo with bar

${str//foo/bar}
Replace ALL occurrences of foo with bar

${str#prefix}
Remove shortest matching prefix

${str##prefix}
Remove longest matching prefix

${str%suffix}
Remove shortest matching suffix

${str%%suffix}
Remove longest matching suffix

${str^^} / ${str,,}
Uppercase / lowercase entire string

${str:-default}
Use default if str is unset or empty

### Practical Examples

### Real Linux Example

```bash
path="/var/log/nginx/access.log"
filename=${path##*/}      # access.log — strip everything up to the last /
directory=${path%/*}      # /var/log/nginx — strip everything from the last /
extension=${filename##*.} # log
echo "$directory | $filename | $extension"
```

### DevOps Example

```bash
version="v1.4.2-rc1"
clean_version=${version#v}        # 1.4.2-rc1
major=${clean_version%%.*}        # 1
echo "Deploying major version $major"
```

### SOC / Cybersecurity Example

```bash
logline='2026-08-07T10:15:02Z host=web01 src=203.0.113.5 status=403'
src_ip=$(grep -oP 'src=\K[0-9.]+' <<< "$logline")
status=$(grep -oP 'status=\K[0-9]+' <<< "$logline")
if [[ "$status" == "403" ]]; then
  echo "Forbidden access attempt from $src_ip"
fi
```

### Pattern Matching and Regex

```bash
email="analyst@example.com"
if [[ "$email" =~ ^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$ ]]; then
  echo "Valid email format"
fi
```

```bash
# BASH_REMATCH holds capture groups after a successful =~ match
if [[ "192.168.1.10" =~ ^([0-9]+)\.([0-9]+)\.([0-9]+)\.([0-9]+)$ ]]; then
  echo "First octet: ${BASH_REMATCH[1]}"
fi
```

### ✅BEST PRACTICE

Use Bash's built-in ${str/.../...} and =~ for small, single-string operations — it avoids the
overhead of forking sed/awk. Reach for awk or sed when processing many lines or doing
complex, multi-field text transformation.

### ⚠COMMON MISTAKE

Confusing ${str#pattern} (shortest match, from the front) with ${str##pattern}
(longest match, from the front). On a path like a/b/c.txt, ${path#*/} removes just 'a/',
while ${path##*/} removes everything up to the last '/'.

### 🎯INTERVIEW TIP

"How do you get a filename without its extension in pure Bash?" —
name=${filename%.*} removes the shortest suffix matching '.*', leaving everything
before the last dot, with zero external commands.

### Chapter Summary

- Parameter expansion (${var...}) provides length, substring, replace, prefix/suffix

trimming, and case conversion without external tools.

- # / ## strip prefixes (shortest/longest match); % / %% strip suffixes

(shortest/longest match).

- The =~ operator inside [[ ]] enables full regex matching, with capture groups

available in BASH_REMATCH.

### Practice Questions

1. What does ${var,,} do?

2. How do you extract a file's extension using only parameter expansion?

3. What array holds regex capture groups after a =~ match?

### HANDS-ON EXERCISE

Given a variable holding a full log line 'level=ERROR service=payments msg=timeout',
extract and print just the level and service values using parameter expansion or grep -
oP.

## Arrays Storing and iterating over ordered collections of values

### Learning Objectives

- Declare, populate, and access indexed arrays
- Understand associative arrays (Bash 4+) as key-value stores
- Loop over array elements and indices correctly
- Use common array operations: length, slicing, appending

### Introduction

A single variable can hold one value. Real automation tasks usually deal with lists — a list
of hosts, IPs, filenames, or usernames. Bash arrays fill that gap without requiring any
external tools.

### Syntax — Indexed Arrays

```bash
hosts=("web01" "web02" "db01")
hosts+=("cache01")            # append
echo "${hosts[0]}"             # web01 — zero-indexed
echo "${hosts[@]}"             # all elements
echo "${#hosts[@]}"            # array length: 4
unset 'hosts[1]'               # remove web02
```

### ⚠COMMON MISTAKE

${hosts[@]} and ${hosts[*]} look similar but behave differently when quoted:
"${hosts[@]}" expands to separate, correctly-quoted words (safe with spaces in
elements), while "${hosts[*]}" joins everything into ONE string. Default to @ unless you
specifically want a single joined string.

### Syntax — Associative Arrays

```bash
declare -A server_role
server_role[web01]="frontend"
server_role[db01]="database"
for host in "${!server_role[@]}"; do
  echo "$host -> ${server_role[$host]}"
done
```

### 📌 IMPORTANT NOTE

Associative arrays require declare -A up front — they will not work by just assigning a
string key to a bare variable. This is a Bash 4.0+ feature; it is NOT available on macOS's
default Bash 3.2.

### Practical Examples

### Real Linux Example

```bash
#!/usr/bin/env bash
services=("nginx" "postgresql" "redis")
for svc in "${services[@]}"; do
  if systemctl is-active --quiet "$svc"; then
    echo "$svc: running"
  else
    echo "$svc: STOPPED"
  fi
done
```

### DevOps Example

```bash
#!/usr/bin/env bash
declare -A env_urls=(
  [dev]="https://dev.internal"
  [staging]="https://staging.internal"
  [prod]="https://app.example.com"
)
target="${1:-dev}"
echo "Deploying to ${env_urls[$target]}"
```

### SOC / Cybersecurity Example

```bash
#!/usr/bin/env bash
# Build an array of blocked IPs from a threat-intel feed and check against logs
mapfile -t blocked_ips < threat_intel_ips.txt
```

```bash
while IFS= read -r logline; do
  ip=$(grep -oP 'src=\K[0-9.]+' <<< "$logline")
  for bad in "${blocked_ips[@]}"; do
    [[ "$ip" == "$bad" ]] && echo "MATCH: $logline"
  done
done < access.log
```

✅ BEST PRACTICE

Use mapfile -t arr < file (or readarray -t arr < file) to load a file into an array
in one step — it's faster and cleaner than a manual while/read loop when you need the
whole file in memory as an array.

### Array Slicing

```bash
nums=(10 20 30 40 50)
echo "${nums[@]:1:3}"    # 20 30 40 — offset 1, length 3
echo "${nums[@]: -2}"    # 40 50 — last two elements (note the required space
before -2)
```

### 🎯INTERVIEW TIP

"What's the difference between "${arr[@]}" and "${arr[*]}"?" — @ expands to each
element as its own separately-quoted word (safe for loops); * joins all elements into a
single string using the first character of IFS as a separator.

### Chapter Summary

- Indexed arrays (hosts=(a b c)) store ordered lists; associative arrays (declare -A)

store key-value pairs.

- "${arr[@]}" is almost always the correct way to expand an array — it preserves

elements as separate, safely-quoted words.

- mapfile/readarray is the fastest way to load an entire file into an array.

### Practice Questions

1. How do you get the number of elements in an array?

2. What Bash version introduced associative arrays, and what command declares one?

3. Why is "${arr[@]}" generally safer to use than "${arr[*]}"?

### HANDS-ON EXERCISE

Build an associative array mapping three hostnames to their IP addresses. Loop over it
and print 'hostname resolves to ip' for each entry, sorted by hostname.

## Functions Packaging logic into reusable, named blocks

### Learning Objectives

- Define and call Bash functions with both syntax styles
- Pass arguments into functions and return values correctly
- Use local variables to avoid polluting the global scope
- Build a small library of reusable functions for a real project

### Introduction

Once a script grows past a few dozen lines, or once you find yourself copy-pasting the
same block of logic twice, it's time for a function. Functions make scripts readable,
testable, and — critically for SOC and DevOps teams — reusable across many different
scripts via sourcing.

### Syntax

```bash
# Style 1 (POSIX-compatible)
greet() {
  echo "Hello, $1"
}
```

```bash
# Style 2 (Bash-specific, functionally identical)
function greet {
  echo "Hello, $1"
}
```

```bash
greet "Analyst"     # calling a function looks just like calling a command
```

### 📌IMPORTANT NOTE

Functions do NOT receive arguments through named parameters like other languages.
Inside a function, $1, $2, ... refer to the function's OWN arguments, not the script's —
and $@/$* work the same way, scoped to the function call.

### Returning Values

Bash functions don't 'return' data the way most languages do — return only sets a
numeric exit status (0-255). To send back real data, print it to stdout and capture it with
command substitution.

```bash
is_valid_ip() {
  local ip="$1"
  [[ "$ip" =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}$ ]]
  return $?          # 0 = valid, 1 = invalid — used as a boolean check
}
```

```bash
get_timestamp() {
  echo "$(date +%Y-%m-%dT%H:%M:%S)"    # 'returns' data via stdout
}
```

```bash
if is_valid_ip "10.0.0.5"; then
  echo "Valid IP"
fi
ts=$(get_timestamp)
echo "Logged at $ts"
```

### ✅BEST PRACTICE

Always declare function-internal variables with local. Without it, every variable a
function sets leaks into the global scope and can silently overwrite variables used
elsewhere in the script — a frequent source of hard-to-debug bugs.

### Practical Examples

### Real Linux Example

```bash
log() {
  local level="$1"; shift
  echo "[$(date '+%F %T')] [$level] $*"
}
log INFO "Service starting"
log ERROR "Config file missing"
```

### DevOps Example

```bash
retry() {
  local -i attempts=$1; shift
  local -i n=0
  until "$@"; do
    ((n++))
    (( n >= attempts )) && return 1
    sleep $((n * 2))
  done
}

retry 3 curl -sf https://api.internal/health
```

### SOC / Cybersecurity Example

```bash
extract_iocs() {
  local file="$1"
  grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' "$file" | sort -u
}

extract_iocs "incident_notes.txt" > iocs.txt
echo "Extracted $(wc -l < iocs.txt) unique IPs"
```

### Function Libraries

A common professional pattern is to write reusable functions into a lib.sh file and

source it from every script in a project, so logging, error handling, and validation logic is
written once and stays consistent.

```bash
# lib.sh
die() { echo "ERROR: $*" >&2; exit 1; }

# main.sh
source ./lib.sh
[[ -f "$1" ]] || die "Input file not found: $1"
```

### 🎯INTERVIEW TIP

"How does a Bash function return a computed value, given return only allows exit codes
0-255?" — By writing the value to stdout with echo/printf and having the caller capture
it with command substitution: result=$(my_function args).

### Chapter Summary

- Functions package reusable logic; call them like any other command, and access

their own arguments via $1, $2, $@.

- return only sets a 0-255 exit status — use stdout + command substitution to

return actual data.

- Always use local for function-internal variables to prevent leaking into the global

scope.

### Practice Questions

1. Why can't a Bash function 'return' a string the way return works in Python?

2. What happens to a variable set inside a function without local?

3. What is a common professional pattern for sharing functions across multiple scripts?

### HANDS-ON EXERCISE

Write a function is_port_open that takes a host and port and returns success/failure
using /dev/tcp or nc, then use it in an if statement to print 'open' or 'closed'.

## File Handling & Reading Files Creating, testing, and processing files reliably

### Learning Objectives

- Create, copy, move, and delete files and directories safely from a script
- Understand the file permission model well enough to check and set it
programmatically
- Read files line-by-line, field-by-field, and in bulk
- Avoid destructive mistakes with rm, mv, and redirection

### Introduction

Almost every real automation script eventually touches the filesystem — writing a log,
reading a config, archiving old data. Bash gives you direct access to the same file
operations available at the command line, which is powerful but also means scripts can
do real damage if written carelessly.

### Core File Operations

```bash
mkdir -p /opt/app/logs        # -p creates parent dirs, no error if it already
exists
touch /opt/app/logs/app.log
cp -a source/ dest/           # -a preserves permissions, timestamps, and
recurses
mv old_name.txt new_name.txt
rm -i important.txt           # -i prompts before deleting — good habit in
interactive use
rm -rf /tmp/build_cache/      # -rf: force, recursive — DANGEROUS, verify the
path first
```

### 🔐SECURITY NOTE

rm -rf "$dir" where $dir is unquoted, unset, or user-controlled is one of the most
catastrophic Bash mistakes possible — if $dir expands to empty, the command can
become rm -rf /. Always quote the variable and validate it is non-empty and expected
before any destructive operation.

### File Test Operators Recap

Test
Meaning

-e
Path exists

-f / -d / -L
Is a regular file / directory / symlink

-r / -w / -x
Readable / writable / executable by current user

-s
Exists and size > 0

-O / -G
Owned by current user / current group

### Reading Files

```bash
# Line by line
while IFS= read -r line; do
  echo "Line: $line"
done < input.txt
```

```bash
# Field by field (CSV/space-delimited)
while IFS=',' read -r user host role; do
  echo "$user connects to $host as $role"
done < inventory.csv
```

```bash
# Whole file into a variable
contents=$(<config.txt)
```

```bash
# Whole file into an array, one line per element
mapfile -t lines < input.txt
```

### Practical Examples

### Real Linux Example

```bash
#!/usr/bin/env bash
# Rotate a log file once it exceeds 10MB
logfile="/var/log/myapp.log"
max_bytes=$((10 * 1024 * 1024))
if [[ -f "$logfile" ]] && (( $(stat -c%s "$logfile") > max_bytes )); then
  mv "$logfile" "${logfile}.$(date +%F).bak"
  touch "$logfile"
fi
```

### DevOps Example

```bash
#!/usr/bin/env bash
# Safely stage build artifacts, cleaning only what THIS script created
build_dir=$(mktemp -d)
trap 'rm -rf "$build_dir"' EXIT
cp -r src/* "$build_dir/"
tar czf release.tar.gz -C "$build_dir" .
```

### SOC / Cybersecurity Example

```bash
#!/usr/bin/env bash
# Preserve evidence: copy, never move, and hash before/after
evidence="/mnt/usb/suspect_file.exe"
dest="/cases/case_1042/"
mkdir -p "$dest"
sha256sum "$evidence" > "$dest/original.sha256"
cp --preserve=timestamps "$evidence" "$dest/"
sha256sum "$dest/$(basename "$evidence")" >> "$dest/original.sha256"
```

### ✅BEST PRACTICE

Use mktemp or mktemp -d to create temporary files/directories with guaranteed-unique,
safely-permissioned names instead of hardcoding paths like /tmp/mydata — hardcoded
temp paths are both a race condition and a symlink-attack risk.

### 🎯INTERVIEW TIP

"How do you safely delete a variable directory path in a script?" — Validate the variable
is non-empty and matches an expected pattern, quote it, and consider requiring an
explicit confirmation flag or a dry-run mode before any rm -rf "$var" executes in
production automation.

### Chapter Summary

- File test operators (-e, -f, -d, -r, -w, -x, -s) let scripts check the filesystem before

acting on it.

- while IFS= read -r is the safe, standard pattern for reading files line by line or

field by field.

- Destructive operations (rm -rf, mv) on unvalidated or unquoted variables are the

single most dangerous class of Bash bugs.

### Practice Questions

1. What does cp -a preserve that a plain cp -r does not?

2. Why is mktemp safer than hardcoding a path like /tmp/scratch?

3. What could go wrong with rm -rf "$dir"/* if $dir is accidentally empty?

### HANDS-ON EXERCISE

Write a script that reads a CSV of username,homedir pairs and reports any homedir path
that does not exist, without deleting or modifying anything — a safe, read-only audit
script.

## Command-Line Arguments & getopts Turning a script into a real command-line tool

### Learning Objectives

- Access positional arguments and understand $@, $*, and $#
- Shift through arguments to handle variable-length input
- Parse flags and options professionally with getopts
- Write a usage/help message following Linux CLI conventions

### Introduction

A script that only works when edited by hand isn't really a tool. Accepting command-line
arguments — like any real Linux utility — is what turns a personal script into something a
whole team, a cron job, or a CI pipeline can call safely and predictably.

### Positional Parameters

Variable
Meaning

$0
The script's own name/path

$1, $2, ...
Individual positional arguments

$#
Number of arguments passed

$@
All arguments, each as a separate word (use this one)

$*
All arguments joined into a single string

shift
Discards $1 and renumbers the rest down by one

```bash
#!/usr/bin/env bash
echo "Script: $0"
echo "Arg count: $#"
echo "All args: $@"
while [[ $# -gt 0 ]]; do
  echo "Processing: $1"
  shift
done
```

### Syntax — getopts

getopts is Bash's built-in, POSIX-standard way to parse single-letter flags (-v, -f file, -h),
including flags that take a value. It's the professional standard for anything beyond one
or two positional arguments.

```bash
#!/usr/bin/env bash
usage() {
  echo "Usage: $0 [-v] [-o output_file] -i input_file" >&2
  exit 1
}
```

```bash
verbose=0
output="result.txt"
```

```bash
while getopts ":vo:i:h" opt; do
  case "$opt" in
    v) verbose=1 ;;
    o) output="$OPTARG" ;;
    i) input="$OPTARG" ;;
    h) usage ;;
    \?) echo "Invalid option: -$OPTARG" >&2; usage ;;
    :)  echo "Option -$OPTARG requires an argument" >&2; usage ;;
  esac
done
```

```bash
[[ -z "$input" ]] && usage
echo "Input=$input Output=$output Verbose=$verbose"
```

getopts Syntax Piece
Meaning

:vo:i:h
v and h take no value; o and i require one (trailing colon)

leading :
Enables silent error mode so you can handle errors yourself

$OPTARG
Holds the value passed to an option that requires one

$OPTIND
Index of the next argument to be processed

### 📌IMPORTANT NOTE

getopts only handles single-dash, single-letter flags (-v, -f). For full GNU-style long
options (--verbose, --file=x), you need a manual case/shift loop over "$@" or the
external getopt (not getopts) command.

### Practical Examples

### DevOps Example

```bash
#!/usr/bin/env bash
# deploy.sh -e prod -v
while getopts "e:v" opt; do
  case "$opt" in
    e) env="$OPTARG" ;;
    v) set -x ;;
  esac
done
echo "Deploying to ${env:-dev}"
```

### SOC / Cybersecurity Example

```bash
#!/usr/bin/env bash
# triage.sh -t 24 -o report.txt <hostname>
hours=24
outfile="triage_report.txt"
while getopts "t:o:" opt; do
  case "$opt" in
    t) hours="$OPTARG" ;;
    o) outfile="$OPTARG" ;;
  esac
done
shift $((OPTIND - 1))
target="$1"
echo "Triaging $target for the last $hours hours -> $outfile"
```

### ✅BEST PRACTICE

Always call shift $((OPTIND - 1)) after your getopts loop. It removes the parsed flags
from $@, leaving only the remaining positional arguments (like a target hostname) in
$1, $2, etc.

### 🎯INTERVIEW TIP

"What's the difference between getopts and getopt?" — getopts is a Bash built-in
supporting short options only, safe and portable. getopt is an external command that
also supports long options (--verbose) but has less consistent behavior across systems.

### Chapter Summary

- $@ (all args as separate words), $# (count), and shift are the fundamentals of

positional argument handling.

- getopts is the standard, POSIX-compliant way to parse short flags, including those

that take a value via $OPTARG.

- Always follow a getopts loop with shift $((OPTIND - 1)) to leave only

remaining positional arguments.

### Practice Questions

1. What is the difference between $@ and $*?

2. What does the leading colon in getopts ":vo:" change about error handling?

3. How would you support both -v and --verbose in the same script?

### HANDS-ON EXERCISE

Write a script scan.sh that accepts -t TIMEOUT, -p PORT, and a required positional
hostname argument, printing a usage message and exiting 1 if the hostname is missing.

## Here Documents & Here Strings Feeding multi-line and inline text into commands cleanly

### Learning Objectives

- Write a here-document (<<) to embed multi-line text or generate files inline
- Suppress and control variable expansion inside a heredoc
- Use here-strings (<<<) for quick one-line input
- Recognize the classic real-world uses: config generation, SQL, remote SSH commands

### Introduction

Sometimes a command needs multiple lines of input — a block of SQL, a config file
template, a message body — and building it with repeated echo calls is clunky and hard
to read. Here-documents let you embed a literal block of text directly in your script.

### Syntax — Here-Document

```bash
cat <<EOF
Hello, this is a multi-line block.
Today is $(date +%F).
EOF
```

Form
Behavior

<<EOF ... EOF
Variables and command substitution ARE expanded

<<'EOF' ... EOF
Quoting the delimiter disables ALL expansion — fully literal text

<<-EOF ... EOF
The dash allows the closing EOF to be indented with TABS (not
spaces)

### ⚠COMMON MISTAKE

The closing delimiter (EOF, or whatever word you chose) must start at the very
beginning of the line — no leading spaces — unless you used the <<- form with tabs. A
stray space before the closing delimiter is one of the most common heredoc errors.

### Practical Examples

### Real Linux Example — generate a config file

```bash
cat > /etc/myapp/config.ini <<EOF
[server]
host = $(hostname)
port = 8080
env = ${ENVIRONMENT:-production}
EOF
```

### DevOps Example — run multiple commands over SSH in one connection

```bash
ssh deploy@web01 <<'REMOTE'
cd /opt/app
git pull origin main
systemctl restart app.service
REMOTE
```

### 📌IMPORTANT NOTE

Quoting the delimiter ('REMOTE' above) is essential here — it keeps $(...) and $VAR
from being expanded LOCALLY before the block is even sent to the remote host, letting
the remote shell interpret them instead.

### SOC / Cybersecurity Example — embedded query

```bash
mysql -u soc_reader -p siem <<SQL
SELECT src_ip, COUNT(*) AS attempts
FROM auth_failures
WHERE event_time > NOW() - INTERVAL 1 HOUR
GROUP BY src_ip
HAVING attempts > 10;
SQL
```

### Here-Strings

A here-string (<<<) is a lightweight one-line alternative to a heredoc — perfect for feeding
a single variable or literal string into a command's stdin without a temporary file.

```bash
grep -oP '[0-9]{1,3}(\.[0-9]{1,3}){3}' <<< "$logline"
# common pattern: read a variable as if it were a file, field by field
IFS=',' read -r user host role <<< "$csv_line"
```

✅ BEST PRACTICE

Prefer a here-string over echo "$var" | command when you only need to feed one
variable into a command's stdin — it avoids an unnecessary extra process (echo) and an
unnecessary pipe.

### 🎯INTERVIEW TIP

"When would you quote the heredoc delimiter?" — Whenever you want the embedded
text treated as fully literal — for example, sending a script block to a remote host over
SSH where you want the REMOTE shell, not your local one, to expand any $variables
inside it.

### Chapter Summary

- Heredocs (<<DELIM ... DELIM) embed multi-line text; quoting the delimiter

disables local variable/command expansion.

- <<-DELIM allows the closing delimiter to be indented, but only with actual tab

characters.

- Here-strings (<<<) are the lightweight, single-line equivalent — ideal for feeding

one variable into a command's stdin.

### Practice Questions

1. What is the effect of quoting the delimiter in a heredoc, e.g. <<'EOF'?

2. Why would you use a heredoc instead of several echo statements when running
commands over SSH?

3. What is the difference in intent between << and <<<?

### HANDS-ON EXERCISE

Write a script that uses a quoted heredoc to generate an Nginx server-block config file
for a domain passed as $1, then verify the file's contents with cat.

## Users & Permissions Automating account management and understanding the permission model

### Learning Objectives

- Create, modify, and lock user accounts from a script
- Read and interpret the Linux permission model (rwx, octal notation)
- Change ownership and permissions safely and predictably
- Recognize dangerous permission misconfigurations relevant to security audits

### Introduction

User and permission management is one of the oldest and most common reasons Bash
scripts get written — onboarding new engineers, locking compromised accounts during
an incident, or auditing a fleet of servers for misconfigured files.

### User Management Commands

Command
Purpose

useradd -m -s /bin/bash name Create a user with a home directory and shell

usermod -aG group name
Add a user to a supplementary group (-a = append,
don't replace)

passwd name
Set/change a password

userdel -r name
Delete a user and their home directory

usermod -L name / -U name
Lock / unlock a password (disable login)

chage -l name
List password aging information

### The Permission Model

Symbol
Octal
Meaning

r
4
Read

w
2
Write

x
1
Execute (or 'enter' for directories)

rwxr-xr--
754
Owner: rwx, Group: r-x, Others: r--

```bash
chmod 750 deploy.sh          # owner rwx, group r-x, others none
chmod u+x script.sh          # add execute for the owner only
chmod -R go-w /etc/app       # recursively remove group/other write access
chown appuser:appgroup file  # change owner and group together
```

### 🔐SECURITY NOTE

World-writable files (permission ending in 2, 3, 6, or 7) combined with execution by a
privileged process are a classic privilege-escalation vector. Any script owned by root but
writable by non-root users is a serious finding in a security audit.

### Practical Examples

### Real Linux Example

```bash
#!/usr/bin/env bash

#Onboard a new engineer
username="$1"
useradd -m -s /bin/bash "$username"
usermod -aG sudo,docker "$username"
passwd -e "$username"      # force password change on first login
echo "Account created for $username"
```

### SOC / Cybersecurity Example

```bash
#!/usr/bin/env bash

#Emergency account lockdown during an active incident
compromised_user="$1"
usermod -L "$compromised_user"
pkill -KILL -u "$compromised_user"
```

```bash
echo "Locked and killed all sessions for $compromised_user at $(date)" >>
/var/log/soc/lockdowns.log
```

### SOC / Cybersecurity Example — permission audit

```bash
find / -xdev -type f -perm -0002 -not -path '/proc/*' 2>/dev/null | while read -r
f; do
  echo "WORLD-WRITABLE: $f ($(stat -c '%U:%G %a' "$f"))"
done
```

### ✅BEST PRACTICE

Always use usermod -aG (append) rather than usermod -G (replace) when adding a user
to a group. Omitting -a silently REMOVES the user from every group they were not
explicitly listed in — a very common, very damaging mistake.

### 🎯INTERVIEW TIP

"What is the difference between chmod 750 and chmod -R 750?" — Plain chmod 750
changes only the permissions of the named file/directory itself. Adding -R applies the
same change recursively to every file and subdirectory inside it — dangerous on
directories containing files that legitimately need different permissions, like execute bits
on scripts only.

### Chapter Summary

- useradd/usermod/passwd/userdel are the core building blocks for scripted

account lifecycle management.

- Linux permissions are read/write/execute for owner, group, and others,

expressible as symbolic (rwx) or octal (750) notation.

- Always use usermod -aG (append) instead of -G (replace) to avoid accidentally

removing existing group memberships.

### Practice Questions

1. What does chmod 640 grant to the owner, group, and others respectively?

2. Why is usermod -G without -a considered dangerous?

3. What find command would you use to locate world-writable files on a system?

### HANDS-ON EXERCISE

Write a script that takes a username as an argument, locks the account, kills all their
active sessions, and logs the action with a timestamp to an incident log file.

## Processes & Services Inspecting, controlling, and automating what's running on a system

### Learning Objectives

- List, filter, and interpret running processes
- Send signals to processes and understand what each common signal does
- Control systemd services from a script
- Build simple process-health monitoring logic

### Introduction

Whether you're restarting a hung service, hunting for a malicious process, or writing a
health-check script, process management is a daily skill for Linux admins, DevOps
engineers, and SOC analysts alike.

### Inspecting Processes

```bash
ps aux                       # full snapshot of every process
ps -ef --forest              # process tree, showing parent/child relationships
top / htop                   # live, refreshing view
pgrep -f nginx                # find PIDs matching a name/pattern
pidof nginx
```

### Signals

Signal
Number
Effect

SIGHUP
1
Reload configuration (convention for many daemons)

SIGINT
2
Interrupt — what Ctrl+C sends

SIGKILL
9
Force-kill immediately; cannot be caught or ignored

SIGTERM
15
Polite request to terminate; the DEFAULT signal for kill

SIGSTOP /
SIGCONT
19 / 18
Pause / resume a process

```bash
kill -15 1234        # polite request — same as: kill 1234
kill -9 1234          # force kill — use only when SIGTERM fails

pkill -f 'python bad_script.py'
killall nginx
```

### ✅BEST PRACTICE

Always try SIGTERM (kill -15, the default) before SIGKILL (kill -9). SIGTERM gives the
process a chance to close files, flush buffers, and shut down cleanly; SIGKILL yanks it out
from under the kernel with no cleanup, which can corrupt data.

### Controlling Services with systemd

```bash
systemctl status nginx
systemctl start|stop|restart|reload nginx
systemctl enable nginx       # start automatically on boot
systemctl is-active --quiet nginx && echo running
```

### Practical Examples

### DevOps Example — self-healing watchdog

```bash
#!/usr/bin/env bash
service="app.service"
if ! systemctl is-active --quiet "$service"; then
  echo "$(date): $service down, restarting" >> /var/log/watchdog.log
  systemctl restart "$service"
fi
```

### SOC / Cybersecurity Example — hunting for suspicious processes

```bash
#!/usr/bin/env bash
# Flag processes running from unusual, writable locations
ps -eo pid,comm,args | while read -r pid comm args; do
  exe_path=$(readlink -f "/proc/$pid/exe" 2>/dev/null)
  [[ "$exe_path" =~ ^/tmp/|^/dev/shm/ ]] && echo "SUSPICIOUS: PID $pid running
from $exe_path ($args)"
done
```

### Real Linux Example — memory hog check

```bash
ps aux --sort=-%mem | awk 'NR<=6 {print $4"%", $11}'
```

### 🎯INTERVIEW TIP

"Why can't SIGKILL be caught or handled by a process?" — SIGKILL and SIGSTOP are
handled directly by the kernel, bypassing the target process's signal handlers entirely,
which is exactly why they always work — but also why they give the process zero chance
to clean up.

### Chapter Summary

- ps, top/htop, and pgrep/pidof are the standard tools for inspecting running

processes.

- SIGTERM (15) is the polite default for kill; SIGKILL (9) is a last resort with no

cleanup.

- systemctl is the standard interface for starting, stopping, and health-checking

services on modern Linux.

### Practice Questions

1. What is the difference between SIGTERM and SIGKILL?

2. Why would a process running from /tmp be considered suspicious?

3. How do you check if a systemd service is currently active from within a script?

### HANDS-ON EXERCISE

Write a watchdog script that checks every 30 seconds (via a loop and sleep) whether a
given process name is running, and restarts a specified systemd service if it disappears,
logging every action with a timestamp.

## Cron Jobs & Logs Scheduling automation and finding the signal inside log noise

### Learning Objectives

- Write and interpret crontab entries and cron scheduling syntax
- Understand why cron scripts behave differently than interactive ones
- Use journalctl and traditional log files to investigate issues
- Build a script safely designed to run unattended, on a schedule

### Introduction

Cron is Linux's built-in job scheduler, and it's how the vast majority of Bash automation
actually runs in production — backups at 2 AM, log rotation, health checks every five
minutes. Writing a script that works perfectly when you run it by hand but fails under cron
is one of the most common real-world Bash frustrations.

### Cron Syntax

```bash
# minute hour day-of-month month day-of-week   command

#   *      *        *          *        *
0 2 * * *   /opt/scripts/backup.sh
*/5 * * * * /opt/scripts/healthcheck.sh
0 9 * * 1   /opt/scripts/weekly_report.sh   # every Monday at 9 AM
```

Field
Range
Example

Minute
0-59
*/15 = every 15 minutes

Hour
0-23
9 = 9 AM

Day of month
1-31
1 = the 1st

Month
1-12
*/3 = every 3 months

Day of week
0-6 (0=Sunday)
1-5 = weekdays

```bash
crontab -l           # list current user's cron jobs
crontab -e           # edit them
# list another user's jobs (needs privilege)
```

```bash
crontab -u appuser -l
```

### ⚠COMMON MISTAKE

Cron jobs run with a minimal environment — often no $PATH beyond /usr/bin:/bin, no
.bashrc sourced, and a different working directory than expected. A script that works
fine interactively can fail silently under cron simply because it can't find aws or docker
on a bare $PATH.

### ✅BEST PRACTICE

In every cron script, use absolute paths for both the interpreter and any commands you
call (or explicitly set PATH at the top of the script), and cd into a known directory before
doing anything relative.

### Logs

Tool / Path
Purpose

journalctl -u nginx
View systemd-managed logs for a specific
service

journalctl -f
Follow logs live, like tail -f

journalctl --since "1 hour ago"
Filter logs by relative time

/var/log/syslog or /var/log/messages
General system log (distro-dependent)

/var/log/auth.log or /var/log/secure
Authentication events — critical for SOC
work

logrotate
Automatic log rotation/compression,
configured in /etc/logrotate.d/

### Practical Examples

### DevOps Example — cron-safe backup script

```bash
#!/usr/bin/env bash
set -euo pipefail
PATH=/usr/local/bin:/usr/bin:/bin
cd /opt/app || exit 1
/usr/bin/tar czf "/backups/app_$(date +%F).tar.gz" .
```

### SOC / Cybersecurity Example — failed login monitor

```bash
#!/usr/bin/env bash
# Run every 5 minutes via cron; alert on brute-force patterns
threshold=10
count=$(journalctl -u sshd --since "-5min" | grep -c "Failed password")
if (( count > threshold )); then
  echo "ALERT: $count failed SSH logins in the last 5 minutes" | mail -s "Brute
force alert" soc@example.com
fi
```

### 📌IMPORTANT NOTE

Cron entries should always redirect output somewhere: * * * * * /script.sh >>
/var/log/script.log 2>&1. Otherwise cron emails the local mail spool by default,
which usually goes unread and unnoticed forever.

### 🎯INTERVIEW TIP

"A script works when you run it manually but fails under cron — what do you check
first?" — $PATH and environment differences, the working directory (cron doesn't
inherit your shell's cwd), and whether the script relies on any interactive-only shell
config (.bashrc) that cron never sources.

### Chapter Summary

- Cron syntax is five time fields (minute, hour, day-of-month, month, day-of-week)

plus the command to run.

- Cron's minimal environment is the #1 cause of 'works manually, fails scheduled'

bugs — always use absolute paths.

- journalctl is the modern systemd log viewer; auth.log/secure is essential reading

for SOC investigations.

### Practice Questions

1. What cron expression runs a job every 15 minutes, only on weekdays?

2. Why might a script that calls aws work interactively but fail under cron?

3. How do you view logs for only the last hour of a specific systemd service?

### HANDS-ON EXERCISE

Write a cron-safe script that checks disk usage on / and emails an alert if usage exceeds
85%, then add a crontab entry to run it every 30 minutes with output properly
redirected to a log file.

## Packages & Networking Commands Managing software and diagnosing connectivity from the shell

### Learning Objectives

- Install, update, and query packages across major package managers
- Use core networking commands to diagnose connectivity and inspect traffic
- Check open ports and listening services on a host
- Combine package and network commands into a basic system-audit script

### Introduction

Package management and network diagnostics are two of the most frequently scripted
admin tasks — patch automation, dependency installation in CI pipelines, and
connectivity troubleshooting during an incident all rest on the same small set of
commands.

### Package Managers

Distro Family
Manager
Common Commands

Debian/Ubuntu
apt
apt update && apt install -y pkg /
apt list --installed

RHEL/CentOS/Fedora
dnf / yum
dnf install -y pkg / dnf check-update

Alpine
apk
apk add pkg / apk update

Any (source)
compile from source
./configure && make && make
install

```bash
#!/usr/bin/env bash
# Portable-ish package check: install only if missing
if ! command -v jq &>/dev/null; then
  if command -v apt &>/dev/null; then sudo apt install -y jq;
  elif command -v dnf &>/dev/null; then sudo dnf install -y jq;
  fi
fi
```

### ✅BEST PRACTICE

Use command -v tool (not which tool) to check whether a command exists in a script
— it's a POSIX built-in, works identically across shells, and correctly reports shell
functions and built-ins too, not just files on $PATH.

### Networking Commands

Command
Purpose

ping -c4 host
Test basic reachability

curl -sSf url
Test HTTP endpoints; -f fails on HTTP errors, -s silences
progress

ss -tulnp
List listening TCP/UDP ports and owning processes (replaces
netstat)

dig / nslookup host
DNS resolution lookups

traceroute host
Show the network path/hops to a destination

nc -zv host port
Quick TCP port check ('is anything listening here')

### Practical Examples

### DevOps Example

```bash
#!/usr/bin/env bash
# Wait for a dependent service to be reachable before starting the app
while ! nc -z db.internal 5432; do
  echo "Waiting for database..."
  sleep 2
done
echo "Database is up — starting app"
exec ./app
```

### SOC / Cybersecurity Example

```bash
#!/usr/bin/env bash
# Audit unexpected listening ports against an approved list
approved=(22 80 443)
ss -tulnp | awk 'NR>1 {print $5}' | grep -oE '[0-9]+$' | sort -u | while read -r
port; do
  if [[ ! " ${approved[*]} " =~ " ${port} " ]]; then
    echo "UNEXPECTED LISTENING PORT: $port"
  fi
done
```

### Cloud Example

```bash
#!/usr/bin/env bash
# Confirm outbound connectivity to a required cloud endpoint before a deploy
if ! curl -sSf -o /dev/null https://api.github.com; then
  echo "Cannot reach GitHub API — check egress/firewall rules" >&2
  exit 1
fi
```

### 🔐SECURITY NOTE

ss -tulnp (or netstat -tulnp on older systems) is one of the very first commands a SOC
analyst runs during host triage — any unfamiliar listening port is a strong signal of a
backdoor, C2 beacon, or unauthorized service.

### 🎯INTERVIEW TIP

"How would you check if a remote port is open without a full network scan?" — nc -zv
host port or timeout 3 bash -c "</dev/tcp/host/port" — both attempt a
lightweight TCP connection and report success/failure without sending any application-
layer data.

### Chapter Summary

- apt/dnf/apk are the dominant package managers; command -v is the portable way

to check if a tool is installed.

- ss, curl, dig, and nc form the core toolkit for scripted network diagnostics.

- Auditing listening ports against an approved list is a standard, easily automated

SOC hardening check.

### Practice Questions

1. Why is command -v tool preferred over which tool inside scripts?

2. What does nc -z do differently from a normal nc connection?

3. Name a command that lists listening TCP/UDP ports along with the owning process.

### HANDS-ON EXERCISE

Write a script that checks whether jq, curl, and nc are installed, installing any missing
ones via the detected package manager, then confirms outbound HTTPS connectivity to
a URL passed as an argument.

## SSH Automation Running commands, copying files, and orchestrating fleets of remote hosts

### Learning Objectives

- Set up key-based SSH authentication for unattended scripts
- Run remote commands and transfer files non-interactively
- Loop a script across a fleet of hosts safely
- Understand the security implications of automated SSH access

### Introduction

SSH is how almost all real Linux automation reaches beyond a single machine — deploying
code to a fleet of web servers, pulling logs from a dozen hosts during an incident, or
running the same audit script across an entire environment. Doing this reliably in a script
means removing every interactive prompt from the equation.

### Key-Based Authentication

```bash
ssh-keygen -t ed25519 -f ~/.ssh/deploy_key -N ""    # generate a passwordless key
pair
ssh-copy-id -i ~/.ssh/deploy_key.pub deploy@web01     # install the public key
remotely
ssh -i ~/.ssh/deploy_key deploy@web01 'uptime'         # test it
```

### 🔐SECURITY NOTE

Never generate SSH keys with an empty passphrase for interactive/human use —
reserve passphrase-less keys strictly for automation accounts with tightly scoped
permissions, and protect them with restrictive file permissions (chmod 600) and, ideally,
an ssh-agent or secrets manager rather than sitting on disk in plaintext.

### Non-Interactive SSH Options

Flag
Purpose

-o BatchMode=yes
Fail instead of prompting for a password if
key auth fails

-o StrictHostKeyChecking=accept-new
Auto-accept new host keys without a
manual prompt

-o ConnectTimeout=5
Don't hang forever on an unreachable host

-i keyfile
Specify which private key to use

### Practical Examples

### DevOps Example — deploy to multiple hosts

```bash
#!/usr/bin/env bash
set -euo pipefail
hosts=("web01" "web02" "web03")
for host in "${hosts[@]}"; do
  echo "Deploying to $host..."
  ssh -o BatchMode=yes -o ConnectTimeout=5 "deploy@$host" <<'REMOTE'
cd /opt/app
git pull origin main
systemctl restart app.service
REMOTE
done
```

### SOC / Cybersecurity Example — collect logs from a fleet during an incident

```bash
#!/usr/bin/env bash
mapfile -t hosts < incident_hosts.txt
for host in "${hosts[@]}"; do
  scp -o BatchMode=yes "soc@$host:/var/log/auth.log"
"./evidence/${host}_auth.log" \
    && echo "Collected auth.log from $host" \
    || echo "FAILED to collect from $host" >&2
done
```

### Real Linux Example — parallel execution

```bash
#!/usr/bin/env bash
# Run a health check on every host at once, wait for all to finish
for host in web01 web02 web03; do
  ssh -o BatchMode=yes "$host" 'systemctl is-active nginx' &
done
wait
echo "All health checks completed"
```

### ✅BEST PRACTICE

Use wait after backgrounding several SSH commands with & to run them in parallel
across a fleet while still letting your script know when every single one has finished —
critical for accurate pass/fail reporting.

### Copying Files

```bash
scp -i ~/.ssh/deploy_key report.txt deploy@web01:/tmp/
rsync -avz --delete ./build/ deploy@web01:/opt/app/     # efficient, incremental
sync
```

### 🎯INTERVIEW TIP

"Why use BatchMode=yes in automated SSH scripts?" — Without it, if key
authentication fails for any reason, SSH silently falls back to an interactive password
prompt, which will hang a script or cron job forever with no visible error.
BatchMode=yes forces an immediate, script-friendly failure instead.

### Chapter Summary

- Key-based auth with BatchMode=yes is the foundation of any script that talks to

remote hosts unattended.

- Heredocs let you send a whole block of remote commands over a single SSH

connection cleanly.

- Backgrounding SSH calls with & and then calling wait lets a script fan out across a

fleet in parallel.

### Practice Questions

1. Why should automation SSH keys be scoped to a dedicated, restricted account rather
than a personal one?

2. What does BatchMode=yes change about SSH's behavior when key auth fails?

3. How would you run the same command on 20 hosts in parallel and know when
they've all finished?

### HANDS-ON EXERCISE

Write a script that reads a list of hostnames from a file and, for each one, uses SSH to
fetch the value of uptime, printing 'host: OK (uptime)' or 'host: UNREACHABLE' for each
— running all checks in parallel.

## Log Analysis & IOC Extraction Turning raw log noise into structured, actionable findings

### Learning Objectives

- Filter, count, and summarize log data using grep, awk, sort, and uniq together
- Extract IOCs (IPs, domains, hashes, URLs) with regular expressions
- Build frequency-based detections directly from log files
- Chain Linux text tools into a single, fast triage pipeline

### Introduction

Long before a SOC team has a SIEM query language available, or when a SIEM simply
doesn't cover a host, raw log analysis with core Linux tools is often the fastest path to an
answer. grep, awk, sort, uniq, and cut — chained together in a pipeline — can outperform
a slow dashboard for a focused question.

### Core Concept — The Filter-Extract-Aggregate Pattern

Nearly every log analysis one-liner follows the same shape: filter to relevant lines, extract
the field you care about, then aggregate (count, sort, dedupe) to surface outliers.

```bash
grep "Failed password" /var/log/auth.log \
  | grep -oP 'from \K[0-9.]+' \
  | sort | uniq -c | sort -rn | head -10
```

### IOC Extraction Patterns

IOC Type
Regex Pattern (grep -oP / -E)

IPv4 address
([0-9]{1,3}\.){3}[0-9]{1,3}

Domain name
\b([a-z0-9-]+\.)+[a-z]{2,}\b

MD5 hash
\b[a-f0-9]{32}\b

SHA256 hash
\b[a-f0-9]{64}\b

Email address
[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}

URL
https?://[^\s"']+

### Practical Examples

### Real Linux Example

```bash
# Top 10 IPs by request count in an nginx access log
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -10
```

### SOC / Cybersecurity Example — extract all IOCs from a report

```bash
#!/usr/bin/env bash
report="$1"
echo "== IPs =="; grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' "$report" | sort -u
echo "== SHA256 hashes =="; grep -oE '\b[a-f0-9]{64}\b' "$report" | sort -u
echo "== URLs =="; grep -oE 'https?://[^[:space:]"]+' "$report" | sort -u
```

### SOC / Cybersecurity Example — timeline of failed logins per hour

```bash
grep "Failed password" /var/log/auth.log \
  | awk '{print $1, $2, $3}' \
  | uniq -c
```

### 💡TIP

awk '{print $N}' is the fastest way to pull a whitespace-delimited column out of
structured log output — far quicker to write than a full regex when the field position is
fixed.

### ✅BEST PRACTICE

Always pipe extraction results through sort -u (or sort | uniq) before acting on them.
Raw log extraction almost always contains duplicates, and deduplicating early keeps
every downstream step (correlation, reporting, blocking) both faster and more accurate.

### ⚠COMMON MISTAKE

Using grep without -o when you only want the MATCHED portion returns the entire
line, not just the IP/hash/URL you extracted a pattern for — a very common beginner
mix-up that pollutes downstream processing.

### 🎯INTERVIEW TIP

"How would you find the top 10 most frequent values in a log column using only core
Linux tools?" — Extract the column (awk/cut/grep -o), then sort | uniq -c | sort -
rn | head -10 — sort groups identical lines together so uniq -c can count them, and the
second sort -rn orders by that count descending.

### Chapter Summary

- The filter → extract → aggregate pipeline (grep, awk/cut, sort | uniq -c | sort -rn)

is the backbone of manual log analysis.

- Regex patterns for IPs, hashes, domains, and URLs let you pull IOCs out of any

unstructured report or log file.

- grep -o is essential when you want only the matched substring, not the entire

containing line.

### Practice Questions

1. What's the correct sort/uniq chain to find the most frequent value in a column?

2. Write a regex that matches a SHA256 hash.

3. Why is grep -oE preferred over plain grep -E when extracting IOCs?

### HANDS-ON EXERCISE

Given a sample auth.log file, write a script that outputs the top 5 source IPs by failed
login attempts, and separately lists any IP with more than 20 attempts as a 'brute force
suspect'.

## Security Automation & Incident Response Scripts Codifying playbooks so response is fast, consistent, and repeatable

### Learning Objectives

- Automate the repetitive first steps of incident triage
- Build a basic host isolation / containment script
- Collect volatile evidence before it disappears
- Understand why speed and consistency matter more than cleverness during IR

### Introduction

During a live incident, every minute spent typing the same commands by hand is a minute
an attacker has to move laterally, exfiltrate data, or cover their tracks. Turning your team's
IR playbook into Bash scripts means the very first response is fast, consistent, and doesn't
depend on which analyst is on call.

### Core Concept — The First Five Minutes

Most IR playbooks agree on the same early priorities: identify what's running, what's
connected, who's logged in, and what changed recently — captured BEFORE containment
actions (which can destroy volatile evidence).

### Practical Examples

### SOC / Cybersecurity Example — rapid triage snapshot

```bash
#!/usr/bin/env bash
set -uo pipefail
case_dir="/cases/case_$(date +%Y%m%d_%H%M%S)"
mkdir -p "$case_dir"
ps auxww                > "$case_dir/processes.txt"
ss -tulnp                > "$case_dir/network_connections.txt"
who -a                   > "$case_dir/logged_in_users.txt"
last -20                 > "$case_dir/recent_logins.txt"
find / -xdev -mmin -60 -type f 2>/dev/null > "$case_dir/recently_modified.txt"
 echo "Triage snapshot saved to $case_dir"
```

### SOC / Cybersecurity Example — host isolation

```bash
#!/usr/bin/env bash
# Emergency network isolation — allow only the SOC's own management IP
mgmt_ip="$1"
iptables -F
iptables -A INPUT -s "$mgmt_ip" -j ACCEPT
iptables -A OUTPUT -d "$mgmt_ip" -j ACCEPT
iptables -A INPUT -j DROP
iptables -A OUTPUT -j DROP
echo "Host isolated at $(date). Only $mgmt_ip permitted." | tee -a
/var/log/soc/isolation.log
```

### 🔐SECURITY NOTE

Test isolation scripts in a lab BEFORE an actual incident. A misconfigured iptables rule
can lock out the SOC's own management access, turning a contained incident into a host
you can no longer reach without physical/console access.

### SOC / Cybersecurity Example — collect evidence, then contain

```bash
#!/usr/bin/env bash
host="$1"
case_dir="/cases/${host}_$(date +%s)"
mkdir -p "$case_dir"
# 1. Collect volatile state FIRST — containment can wipe active connections
ssh "soc@$host" 'ps auxww; ss -tulnp; who -a' > "$case_dir/volatile_state.txt"
# 2. THEN contain
ssh "soc@$host" 'sudo /opt/scripts/isolate.sh 10.0.0.5'
echo "Evidence collected and host isolated: $host"
```

### ✅BEST PRACTICE

Always order IR scripts as: collect volatile evidence, THEN contain/isolate. Reversing the
order (isolating first) can kill active network connections or processes that were about to
reveal the attacker's C2 infrastructure or lateral movement path.

### Playbook Automation Pattern

```bash
#!/usr/bin/env bash
# A minimal, extensible playbook runner
playbook="$1"; target="$2"
case "$playbook" in
  triage)    ./playbooks/triage.sh "$target" ;;
  isolate)   ./playbooks/isolate.sh "$target" ;;
  collect)   ./playbooks/collect_evidence.sh "$target" ;;
  *)         echo "Unknown playbook: $playbook" >&2; exit 1 ;;
esac
```

### 🎯INTERVIEW TIP

"Why automate IR playbooks instead of relying on analyst memory during an incident?" — Consistency and speed under pressure: a scripted playbook executes the same proven steps every time regardless of analyst experience or stress level, and it eliminates the delay of typing commands manually while an attacker is still active.

### Chapter Summary

- IR automation should capture volatile evidence (processes, connections, logged-in

users) before any containment step runs.

- A simple case()-based playbook runner turns ad-hoc IR commands into a

consistent, repeatable, team-shared tool.

- Containment scripts (like network isolation) must be tested in a lab first — a bad

rule can lock out the responder too.

### Practice Questions

1. Why should evidence collection happen before host isolation, not after?

2. What volatile information disappears the moment a host is powered off or isolated?

3. How would you structure a script to support multiple named playbooks (triage,
isolate, collect)?

🔧 HANDS-ON EXERCISE

Extend the triage snapshot script to also capture the last 100 lines of auth.log and a
listing of all cron jobs for every user, saving everything into the same case directory.

## File Integrity Monitoring & Threat Hunting Helpers Detecting unauthorized change and proactively searching for compromise

### Learning Objectives

- Build a simple file integrity monitor using checksums
- Detect newly created, modified, or deleted files across a baseline
- Write proactive threat-hunting one-liners for common attacker techniques
- Understand the limits of Bash-based FIM versus dedicated tools

### Introduction

File Integrity Monitoring (FIM) answers a simple but critical question: has anything
changed that shouldn't have? Dedicated tools like AIDE or Tripwire exist for production
use, but understanding the underlying mechanism — hashing files and comparing against
a known-good baseline — is exactly the kind of thing Bash is well suited to prototype or
run in a lightweight environment.

### Core Concept — Baseline and Compare

FIM has two phases: first, record a cryptographic hash of every file you care about while
the system is known-good. Second, on every subsequent run, re-hash the same files and
diff against the baseline — any mismatch means the file changed.

### Syntax

```bash
sha256sum /etc/passwd /etc/shadow /etc/sudoers > baseline.sha256
sha256sum -c baseline.sha256    # verify — reports OK or FAILED per file
```

### Practical Examples

### SOC / Cybersecurity Example — build and check a baseline

```bash
#!/usr/bin/env bash
# fim.sh baseline   -> creates baseline
# fim.sh check      -> compares against it
watch_dirs=(/etc /usr/bin /usr/sbin)
baseline="/var/lib/fim/baseline.sha256"
```

```bash
case "$1" in
  baseline)
    mkdir -p "$(dirname "$baseline")"
    find "${watch_dirs[@]}" -type f -exec sha256sum {} + > "$baseline"
    echo "Baseline created: $(wc -l < "$baseline") files"
    ;;
  check)
    sha256sum -c "$baseline" 2>/dev/null | grep -v ': OK$'
    ;;
  *)
    echo "Usage: $0 {baseline|check}" >&2; exit 1 ;;
esac
```

### 📌IMPORTANT NOTE

This approach only detects CONTENT changes to files already in the baseline. It will not,
by itself, notice brand-new files added after baselining — pair it with a find -newer
sweep (see below) for full coverage.

### SOC / Cybersecurity Example — detect new/changed files since a baseline timestamp

```bash
find /etc /usr/bin -newer /var/lib/fim/baseline_timestamp -type f 2>/dev/null
```

### Threat Hunting Helpers

### Hunt: SUID/SGID binaries (privilege escalation surface)

```bash
find / -xdev -perm /6000 -type f 2>/dev/null
```

### Hunt: hidden files outside expected locations

```bash
find /home /tmp /var/tmp -name ".*" -type f 2>/dev/null
```

### Hunt: recently modified binaries in system paths

```bash
find /usr/bin /usr/sbin /bin /sbin -mtime -7 -type f 2>/dev/null
```

### Hunt: unauthorized cron persistence

```bash
for user in $(cut -f1 -d: /etc/passwd); do
  crontab -l -u "$user" 2>/dev/null | grep -v '^#' | sed "s/^/$user: /"
done
```

### ✅BEST PRACTICE

Run threat-hunting sweeps like these on a regular schedule (via cron) and diff the
output against the previous run — a NEW SUID binary or a NEW cron entry that
appeared since yesterday is a far stronger signal than the raw list alone.

### 🏢ENTERPRISE NOTE

For production FIM at scale, use a purpose-built tool (AIDE, Tripwire, osquery, or an EDR
agent's built-in FIM) rather than a homegrown Bash script — they handle exclusions,
performance at scale, tamper-resistant baselines, and centralized alerting far better
than a shell script can.

### 🎯INTERVIEW TIP

"What's the fundamental weakness of hash-based FIM?" — It only detects that a file
changed, not what changed or why, and it's blind to anything outside its watched
paths/baseline. A sophisticated attacker who also updates the baseline (if they gain the
same access the FIM tool has) can hide their tracks entirely.

### Chapter Summary

- FIM works by hashing a known-good baseline and comparing future hashes

against it to detect unauthorized change.

- sha256sum -c is a simple, effective Bash-native way to prototype file integrity

checks.

- Threat-hunting one-liners (SUID binaries, recent file changes, cron audits) are

cheap, fast ways to proactively look for compromise indicators.

### Practice Questions

1. What does hash-based FIM fail to detect, by design?

2. How would you find every SUID binary on a system in one command?

3. Why should hunting sweeps be diffed against previous runs rather than reviewed in
isolation each time?

### HANDS-ON EXERCISE

Extend the fim.sh script with a diff mode that shows exactly which files were added,
removed, or modified compared to the baseline, using comm or diff against a freshly
generated file list.

## Docker & Kubernetes Automation Scripting containers and orchestrated workloads from the command line

### Learning Objectives

- Automate common Docker lifecycle operations from a script
- Write a solid container entrypoint script
- Run and parse kubectl commands for automation and monitoring
- Understand the shell's role inside a container versus outside it

### Introduction

Bash and containers are deeply intertwined: Bash (or a minimal shell like ash) is almost
always PID 1's neighbor inside a container, entrypoint scripts are usually Bash, and kubectl
output is routinely parsed with the same text tools covered earlier in this handbook.

### Docker Automation

```bash
docker build -t myapp:latest .
docker run -d --name myapp -p 8080:8080 myapp:latest
docker ps --filter status=running --format '{{.Names}}: {{.Status}}'
docker logs -f myapp
docker exec -it myapp bash
```

### DevOps Example — cleanup script

```bash
#!/usr/bin/env bash
set -euo pipefail
# Remove stopped containers and dangling images older than 24h
docker container prune -f --filter "until=24h"
docker image prune -f --filter "until=24h"
echo "Docker cleanup complete: $(date)"
```

### Writing a Container Entrypoint Script

```bash
#!/usr/bin/env bash
set -e
# Wait for a dependency before starting the main process
until nc -z "${DB_HOST:-db}" "${DB_PORT:-5432}"; do
  echo "Waiting for database..."
  sleep 1
done
```

```bash
# exec replaces the shell process with the app — critical for signal handling
exec "$@"
```

### ✅BEST PRACTICE

Always end a container entrypoint with exec "$@" rather than just running the
command normally. Without exec, the app runs as a CHILD of the shell, meaning
SIGTERM sent by docker stop goes to the shell, not the app, causing slow or forced
shutdowns instead of graceful ones.

### Kubernetes Automation

```bash
kubectl get pods -n prod --field-selector=status.phase!=Running
kubectl logs -f deployment/api -n prod
kubectl exec -it pod/api-7d9f -n prod -- bash
kubectl rollout status deployment/api -n prod
```

### DevOps Example — restart any pod stuck in CrashLoopBackOff

```bash
#!/usr/bin/env bash
namespace="${1:-default}"
kubectl get pods -n "$namespace" --no-headers | awk '$3=="CrashLoopBackOff"{print
$1}' | \
while read -r pod; do
  echo "Restarting crash-looping pod: $pod"
  kubectl delete pod "$pod" -n "$namespace"
done
```

### SOC / Cybersecurity Example — audit privileged containers

```bash
kubectl get pods --all-namespaces -o json | \
  jq -r '.items[] | select(.spec.containers[].securityContext.privileged==true) |
.metadata.namespace + "/" + .metadata.name'
```

### 🔐SECURITY NOTE

Privileged containers effectively have root access to the underlying node. Scripting a
regular audit for privileged: true pods across every namespace is one of the highest-
value Kubernetes security checks a small script can perform.

### ☁CLOUD NOTE

kubectl and docker CLI output is designed for human eyes by default. Always use --
format (Docker's Go templates) or -o json piped to jq (Kubernetes) when a script
needs to reliably parse output — never scrape the default table formatting, which can
change between versions.

### 🎯INTERVIEW TIP

"Why does a Dockerfile's CMD/ENTRYPOINT often end with exec "$@" in a wrapper
script?" — So the application process becomes PID 1 (or a direct child receiving signals

properly) instead of a grandchild of the shell — ensuring docker stop's SIGTERM
reaches the app directly for a clean, fast shutdown.

### Chapter Summary

- Docker entrypoint scripts should end with exec "$@" so signals reach the

application process directly.

- kubectl combined with jq and JSON output (-o json) is the reliable way to script

against Kubernetes, avoiding fragile table-scraping.

- Regularly auditing for privileged containers and pods with excessive permissions is

a cheap, high-value security script.

### Practice Questions

1. Why is exec "$@" important at the end of a container entrypoint script?

2. What's wrong with parsing the default kubectl get pods table output in a
production script?

3. How would you find every privileged pod across all namespaces?

### HANDS-ON EXERCISE

Write an entrypoint script that waits for a Redis dependency to become reachable
(using nc -z in a loop), prints a ready message, then execs the actual application
command passed as arguments.

## CI/CD Scripts, AWS & Azure CLI Automation Bash as the glue language of modern pipelines and cloud automation

### Learning Objectives

- Write build/test/deploy stages as clean, reusable Bash scripts
- Automate common AWS CLI and Azure CLI tasks
- Parse cloud CLI JSON output correctly with jq
- Apply defensive scripting practices specifically for pipeline environments

### Introduction

Underneath almost every CI/CD platform's fancy YAML syntax, the actual work — running
tests, building artifacts, deploying — is executed by Bash. And every major cloud
provider's CLI (aws, az, gcloud) is itself designed to be scripted from Bash. This chapter
treats both as one skill: gluing pipeline steps and cloud APIs together reliably.

### CI/CD Script Patterns

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "::group::Install dependencies"
npm ci
echo "::endgroup::"

echo "::group::Run tests"
npm test -- --ci
echo "::endgroup::"

echo "::group::Build"
npm run build
echo "::endgroup::"
```

### ✅BEST PRACTICE

Every CI script should start with set -euo pipefail: -e stops on the first failing
command, -u catches typo'd/unset variables, and pipefail makes a failing step inside a
pipe actually fail the pipeline, instead of being silently masked by the last command's
success.

### AWS CLI Automation

```bash
# List running EC2 instances with name tags, parsed cleanly with jq
aws ec2 describe-instances \
  --filters "Name=instance-state-name,Values=running" \
  --query
'Reservations[].Instances[].{ID:InstanceId,Name:Tags[?Key==`Name`]|[0].Value}' \
  --output json | jq -r '.[] | "\(.ID)  \(.Name)"'
```

### Cloud Example — rotate an S3 access key

```bash
#!/usr/bin/env bash
set -euo pipefail
user="$1"
old_key=$(aws iam list-access-keys --user-name "$user" --query
'AccessKeyMetadata[0].AccessKeyId' --output text)
new_key=$(aws iam create-access-key --user-name "$user" --output json)
echo "$new_key" | jq -r '.AccessKey | "AccessKeyId=\(.AccessKeyId)
SecretAccessKey=\(.SecretAccessKey)"'
aws iam delete-access-key --user-name "$user" --access-key-id "$old_key"
echo "Rotated key for $user"
```

### Azure CLI Automation

```bash
# List all stopped VMs across a subscription
az vm list -d --query "[?powerState=='VM deallocated'].{Name:name,
RG:resourceGroup}" -o table
```

```bash
# Deploy a resource group from a Bicep/ARM template
az deployment group create \
  --resource-group prod-rg \
  --template-file main.bicep \
  --parameters environment=prod
```

### ☁CLOUD NOTE

Both aws and az support --output json (or -o json) plus a native query language (--
query JMESPath for AWS, --query JMESPath for Azure too). Combine that with jq for
anything the query language can't express — this avoids fragile text-scraping of table
output entirely.

### Practical Examples

### SOC / Cybersecurity Example — audit public S3 buckets

```bash
#!/usr/bin/env bash
for bucket in $(aws s3api list-buckets --query 'Buckets[].Name' --output text);
do
  status=$(aws s3api get-public-access-block --bucket "$bucket" 2>/dev/null | jq
-r '.PublicAccessBlockConfiguration.BlockPublicAcls')
  if [[ "$status" != "true" ]]; then
    echo "WARNING: $bucket may allow public ACLs"
  fi
done
```

### 🏢ENTERPRISE NOTE

In real pipelines, never hardcode cloud credentials in the script itself. Use the CI
platform's native secret store (GitHub Actions secrets, GitLab CI/CD variables) or
workload identity/OIDC federation so the pipeline assumes a scoped role without any
long-lived key ever touching the repository.

### 🎯INTERVIEW TIP

"How would you safely rotate a cloud access key with zero downtime?" — Create the
NEW key first, update all consumers to use it, verify it works, and only THEN delete the
old key — never delete-then-create, which creates a window where nothing has valid
credentials.

### Chapter Summary

- set -euo pipefail is the standard defensive header for any CI/CD Bash script.

- aws/az CLI JSON output combined with --query (JMESPath) and jq is the reliable

way to script against cloud APIs.

- Credential handling in pipelines should use the platform's secret store or OIDC

federation — never hardcoded keys in scripts.

### Practice Questions

1. What three things does set -euo pipefail protect against?

2. Why should you create a new access key before deleting the old one during rotation?

3. What tool is commonly paired with aws/az JSON output to extract specific fields in a
script?

### HANDS-ON EXERCISE

Write a script that lists every AWS security group with a rule allowing inbound traffic
from 0.0.0.0/0 on port 22, using --query and jq to format the output as 'group-id:
description'.

## Backup Automation Reliable, verifiable backups scripted from first principles

### Learning Objectives

- Write a backup script covering compression, naming, and rotation
- Verify backup integrity instead of assuming success
- Implement a retention policy that deletes old backups safely
- Extend local backups to cloud storage destinations

### Introduction

A backup script that nobody has verified is not actually a backup — it's a hope. This
chapter builds a complete, defensible backup pattern: compress, timestamp, verify,
rotate, and (optionally) ship offsite.

### Core Pattern

```bash
#!/usr/bin/env bash
set -euo pipefail
```

```bash
src="/opt/app/data"
dest_dir="/backups"
timestamp=$(date +%Y%m%d_%H%M%S)
archive="${dest_dir}/app_data_${timestamp}.tar.gz"
```

```bash
mkdir -p "$dest_dir"
tar czf "$archive" -C "$(dirname "$src")" "$(basename "$src")"
```

```bash
# Verify: a corrupt or truncated archive fails this test
if ! tar tzf "$archive" &>/dev/null; then
  echo "BACKUP VERIFICATION FAILED: $archive" >&2
  exit 1
fi
sha256sum "$archive" > "${archive}.sha256"
echo "Backup complete and verified: $archive"
```

### ✅BEST PRACTICE

Never trust a backup you haven't tested. tar tzf (list contents without extracting) is a
cheap, fast integrity check that catches truncated or corrupted archives immediately,
before you discover the problem during an actual restore.

### Retention / Rotation

```bash
#!/usr/bin/env bash
# Keep the most recent 14 daily backups, delete anything older
find /backups -name 'app_data_*.tar.gz' -mtime +14 -print -delete
find /backups -name 'app_data_*.tar.gz.sha256' -mtime +14 -print -delete
```

### ⚠COMMON MISTAKE

Running find /backups -mtime +14 -delete WITHOUT first testing the same
command with -print (no -delete) instead is how backup directories get wiped by an
off-by-one path or pattern bug. Always dry-run destructive find commands first.

### Practical Examples

### Cloud Example — ship backups to S3

```bash
#!/usr/bin/env bash
set -euo pipefail
archive="$1"
aws s3 cp "$archive" "s3://company-backups/$(hostname)/$(basename "$archive")" \
  --storage-class STANDARD_IA
aws s3api head-object --bucket company-backups --key "$(hostname)/$(basename
"$archive")" &>/dev/null \
  && echo "Confirmed uploaded to S3" \
  || { echo "S3 upload verification FAILED" >&2; exit 1; }
```

### Real Linux Example — database dump with rotation

```bash
#!/usr/bin/env bash
set -euo pipefail
timestamp=$(date +%F)
pg_dump mydb | gzip > "/backups/mydb_${timestamp}.sql.gz"
find /backups -name 'mydb_*.sql.gz' -mtime +30 -delete
```

### SOC / Cybersecurity Example — pre-remediation snapshot

```bash
#!/usr/bin/env bash
# Snapshot a config directory before applying a security fix, in case rollback is
needed
cfg="/etc/myapp"
tar czf "/rollback/myapp_config_$(date +%s).tar.gz" -C / "${cfg#/}"
echo "Pre-change snapshot saved — safe to proceed with remediation"
```

### 🏢ENTERPRISE NOTE

The 3-2-1 backup rule remains the industry standard: at least 3 copies of data, on 2
different types of media, with 1 copy stored offsite (or in a different cloud
region/account). A single local tarball satisfies none of these on its own.

### 🎯INTERVIEW TIP

"How do you verify a tar.gz backup is actually restorable without doing a full restore?"
— tar tzf archive.tar.gz lists and decompresses the archive's table of contents
without extracting files to disk — it will fail immediately on truncation or corruption,
giving a fast, low-cost integrity signal.

### Chapter Summary

- A trustworthy backup script compresses, timestamps, and VERIFIES the archive —

never assumes success from a zero exit code alone.

- Retention/rotation with find -mtime +N -delete should always be dry-run with -

print first.

- The 3-2-1 rule (3 copies, 2 media types, 1 offsite) is the standard a single local

backup script does not satisfy alone.

### Practice Questions

1. How can you verify a tar.gz archive's integrity without extracting it?

2. Why should a destructive find -delete command be tested with -print first?

3. What does the 3-2-1 backup rule require?

### HANDS-ON EXERCISE

Extend the core backup script to also generate a SHA256 checksum, upload both the
archive and checksum to S3, and delete local backups older than 7 days — logging every
step with timestamps to a backup log file.

## Defensive & Secure Bash Coding, Error Handling Writing scripts that fail loudly, safely, and predictably

### Learning Objectives

- Apply the 'strict mode' header to every production script
- Trap errors and clean up resources reliably, even on failure
- Sanitize and validate all external input before acting on it
- Avoid the classic Bash security anti-patterns

### Introduction

The gap between a script that works on your laptop and one that's safe to run in
production, unattended, against real infrastructure, is defensive coding. This chapter
collects the practices that separate hobbyist scripts from professional automation.

### Strict Mode

```bash
#!/usr/bin/env bash
set -euo pipefail
IFS=$'\n\t'   # restrict word splitting to newlines/tabs, not every space
```

Setting
Effect

set -e
Exit immediately if any command fails (returns non-zero)

set -u
Error on any reference to an unset variable

set -o pipefail
A pipeline fails if ANY stage fails, not just the last one

set -x
Print every command before executing it — invaluable for
debugging

IFS=$'\n\t'
Prevents accidental word-splitting on spaces inside
filenames/variables

### 📌IMPORTANT NOTE

set -e has real, well-documented gotchas: it does NOT trigger inside functions called in
an if condition, inside && chains, or for commands whose failure is intentionally
checked. Understand these edge cases rather than blindly trusting -e to catch
everything.

### Trapping Errors and Cleaning Up

```bash
#!/usr/bin/env bash
set -euo pipefail
tmpfile=$(mktemp)
cleanup() { rm -f "$tmpfile"; }
trap cleanup EXIT              # runs on ANY exit — normal, error, or signal
```

```bash
trap 'echo "Error on line $LINENO" >&2' ERR
```

```bash
curl -s https://example.com -o "$tmpfile"
process_data "$tmpfile"
```

### ✅BEST PRACTICE

trap cleanup EXIT is the single most valuable defensive-scripting habit to build. It
guarantees temp files are removed and locks are released whether the script finishes
normally, hits set -e, or is killed by Ctrl+C — a plain 'cleanup at the bottom of the
script' does none of that.

### Input Validation and Sanitization

```bash
validate_ip() {
  local ip="$1"
  [[ "$ip" =~ ^([0-9]{1,3}\.){3}[0-9]{1,3}$ ]] || { echo "Invalid IP: $ip" >&2;
return 1; }
  IFS='.' read -r a b c d <<< "$ip"
  for octet in "$a" "$b" "$c" "$d"; do
    (( octet >= 0 && octet <= 255 )) || { echo "Invalid octet: $octet" >&2;
return 1; }
  done
}
```

```bash
target="$1"
validate_ip "$target" || exit 1
ping -c1 "$target"
```

### 🔐SECURITY NOTE

NEVER pass unsanitized user input directly to eval, or interpolate it unquoted into a
command that will be executed by another shell (like an SSH command string). Both are
classic command-injection vectors. Validate against an explicit allow-list pattern before
using external input in any command.

### Practical Examples

### DevOps Example — safe temp workspace

```bash
#!/usr/bin/env bash
set -euo pipefail
workdir=$(mktemp -d)
trap 'rm -rf "$workdir"' EXIT
cd "$workdir"
git clone --depth 1 https://github.com/org/repo .
./build.sh
```

### SOC / Cybersecurity Example — safe use of an argument in a query

```bash
#!/usr/bin/env bash
username="$1"
[[ "$username" =~ ^[a-zA-Z0-9_.-]+$ ]] || { echo "Invalid username format" >&2;
exit 1; }
grep -F -- "$username" /var/log/auth.log
```

### 🎯INTERVIEW TIP

"What's the difference between set -e and manually checking $? after every
command?" — set -e automatically stops the script on ANY unchecked failing command
with zero extra code, but has subtle exceptions (inside conditionals, && chains). Manual
$? checks are more explicit and predictable but require discipline to add after every
single command — most professional scripts use set -e as a safety net AND explicit
checks for critical operations.

### Chapter Summary

- set -euo pipefail plus a restrictive IFS is the standard defensive header for

professional Bash scripts.

- trap cleanup EXIT guarantees resource cleanup regardless of how or why the

script exits.

- All external input — arguments, environment variables, file contents — must be

validated before being used in a command, especially before eval or remote
execution.

### Practice Questions

1. What are two documented situations where set -e does NOT trigger, even on a failing
command?

2. Why is trap cleanup EXIT better than putting cleanup code at the bottom of a
script?

3. What is the security risk of passing unsanitized input to eval?

### HANDS-ON EXERCISE

Take any script from an earlier chapter and add: a strict-mode header, an EXIT trap for
cleanup, input validation for its arguments, and an ERR trap that logs the failing line
number.

## Debugging, Performance & Logging Framework Finding bugs fast, writing efficient scripts, and building consistent logs

### Learning Objectives

- Use Bash's built-in debugging tools to trace execution
- Identify and fix common performance bottlenecks in scripts
- Build a reusable logging function with severity levels
- Use shellcheck as a standard part of the development workflow

### Introduction

As scripts grow into real tools relied on by a team, two things start to matter a lot more:
how fast they run, and how quickly you (or a teammate) can diagnose a failure at 2 AM.
This chapter covers the practical toolkit for both.

### Debugging Tools

```bash
bash -x script.sh              # trace every command as it executes
set -x                          # turn tracing on mid-script
set +x                          # turn it back off
```

```bash
PS4='+ ${BASH_SOURCE}:${LINENO}: '   # customize trace output to show file & line
number
```

```bash
#!/usr/bin/env bash
set -euo pipefail
trap 'echo "FAILED at line $LINENO: $BASH_COMMAND" >&2' ERR
```

### 💡TIP

shellcheck script.sh is the single highest-value tool a Bash developer can run. It
catches quoting bugs, unreachable code, and dozens of subtle mistakes covered in this
handbook, automatically, before the script ever runs.

### Performance Optimization

Slow Pattern
Faster Alternative

cat file | grep pattern
grep pattern file (skip the useless cat)

Calling external tools in a tight loop
(e.g. echo | grep per line)

Use Bash built-ins (${var/x/y}, [[ =~ ]]) inside
the loop instead

for line in $(cat file)
while IFS= read -r line; do ... done < file

Repeated date calls for the same
instant
Call date once, store in a variable, reuse it

Spawning a subshell for simple
arithmetic: $(( )) via expr

Use native $(( )) — expr forks an external
process

### ✅BEST PRACTICE

Every external command inside a loop (grep, awk, date, expr) forks a new process —
expensive when repeated thousands of times. Prefer Bash built-ins and parameter
expansion inside hot loops, and reserve external tools for operations they're uniquely
suited to (regex-heavy parsing, full-file processing).

### A Reusable Logging Framework

```bash
# lib/logging.sh
LOG_FILE="${LOG_FILE:-/var/log/myapp/script.log}"
log() {
  local level="$1"; shift
  local ts; ts=$(date '+%Y-%m-%d %H:%M:%S')
  local line="[$ts] [$level] $*"
  echo "$line" | tee -a "$LOG_FILE"
}
```

```bash
log_info()  { log "INFO"  "$@"; }
log_warn()  { log "WARN"  "$@"; }
log_error() { log "ERROR" "$@" >&2; }
```

```bash
#!/usr/bin/env bash
source ./lib/logging.sh
log_info "Backup job starting"
if ! tar czf backup.tar.gz /data; then
  log_error "Backup failed"
  exit 1
fi
log_info "Backup job completed successfully"
```

### Practical Examples

### DevOps Example — timing a script for optimization

```bash
time ./deploy.sh          # real/user/sys time breakdown
```

```bash
# Or measure just one section:
start=$(date +%s%N)
heavy_operation
end=$(date +%s%N)
echo "Took $(( (end - start) / 1000000 )) ms"
```

### SOC / Cybersecurity Example — structured JSON logging for SIEM ingestion

```bash
log_json() {
  local level="$1" msg="$2"
  printf '{"timestamp":"%s","level":"%s","message":"%s","host":"%s"}\n' \
    "$(date -Iseconds)" "$level" "$msg" "$(hostname)" >>
/var/log/soc/events.jsonl
}
log_json "ALERT" "Brute force detected from 203.0.113.5"
```

### 🎯INTERVIEW TIP

"How would you debug a script that fails intermittently in production but not locally?"
— Add a PS4-customized set -x trace behind an environment-variable flag (so it can be
toggled without redeploying), an ERR trap logging $LINENO and $BASH_COMMAND, and
structured logging around suspect sections — then compare production's environment
(PATH, locale, resource limits) against your local one, since that's the most common
source of 'works here, not there' bugs.

### Chapter Summary

- bash -x, a customized PS4, and an ERR trap logging $LINENO/$BASH_COMMAND

form the core debugging toolkit.

- Avoid forking external processes inside hot loops; prefer Bash built-ins for

arithmetic and simple string operations.

- A small logging library (log_info/log_warn/log_error) applied consistently across

all scripts makes production issues far easier to diagnose.

### Practice Questions

1. What does PS4 control, and why would you customize it?

2. Why is for line in $(cat file) both a correctness bug and a performance
problem?

3. What tool should be run on every Bash script before it's considered production-
ready?

### HANDS-ON EXERCISE

Build the logging.sh library shown in this chapter, source it into a script of your choice,
and replace every echo with the appropriate log_info/log_warn/log_error call.

## Script Templates, Bash Style Guide & Enterprise Folder Structure Standardizing how a team writes, organizes, and maintains Bash

### Learning Objectives

- Use a consistent, production-ready script template as a starting point
- Follow a documented style guide for naming, formatting, and structure
- Organize a script library the way an engineering team can maintain long-term
- Understand why standardization matters more as team size grows

### Introduction

Every practice covered in this handbook — strict mode, functions, logging, error handling
— comes together here into a reusable template and a set of conventions. A team that
agrees on structure once spends far less time relearning each other's scripts later.

### A Production Script Template

```bash
#!/usr/bin/env bash
#
# script:  template.sh
# purpose: <one-line description>
# usage:   ./template.sh [-v] -i <input>
# author:  <name>
```

```bash
set -euo pipefail
IFS=$'\n\t'
```

```bash
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
LOG_FILE="${LOG_FILE:-/var/log/${0##*/}.log}"
```

```bash
usage() {
  echo "Usage: $0 [-v] -i <input>" >&2
  exit 1
}
```

```bash
log() { echo "[$(date '+%F %T')] $*" | tee -a "$LOG_FILE"; }
cleanup() { log "Cleaning up temporary resources"; }
trap cleanup EXIT
trap 'log "ERROR at line $LINENO: $BASH_COMMAND"' ERR
```

```bash
main() {
  local input="" verbose=0
  while getopts ":vi:" opt; do
    case "$opt" in
      v) verbose=1 ;;
      i) input="$OPTARG" ;;
      *) usage ;;
    esac
  done
  [[ -z "$input" ]] && usage
```

```bash
log "Starting with input=$input verbose=$verbose"
  # ... actual work goes here ...
  log "Completed successfully"
}
main "$@"
```

### ✅BEST PRACTICE

Wrapping a script's logic in a main "$@" function called at the very bottom (rather than
running top-to-bottom loose) makes the script easier to read top-down, easier to source
for testing without side effects, and keeps all variables function-scoped by default.

### Bash Style Guide

Element
Convention

Variable names
lower_snake_case for local/script vars; UPPER_SNAKE_CASE for
exported/env/constants

Function names
lower_snake_case, verb-first (validate_input, send_alert)

Indentation
2 spaces, never tabs (except heredoc <<- closing delimiters)

Quoting
Always double-quote variable expansions unless
splitting/globbing is intended

Comparisons
[[ ]] over [ ] in Bash-only scripts; (( )) for arithmetic

File extension
*.sh for standalone scripts; no extension for commands installed
on PATH

Shebang
#!/usr/bin/env bash unless the environment guarantees
/bin/bash

### Enterprise Folder Structure

```bash
automation/
├── bin/                  # entrypoint scripts users/cron actually call
│   ├── backup.sh
│   └── deploy.sh
├── lib/                  # shared, sourced function libraries
│   ├── logging.sh
│   └── validation.sh
├── config/               # environment-specific configuration, not secrets
│   └── prod.env
├── tests/                # bats or shellcheck-driven test scripts
│   └── test_backup.bats
├── docs/
│   └── README.md
└── .shellcheckrc         # shared lint configuration for the whole team
```

### 🏢ENTERPRISE NOTE

Separating bin/ (things you run) from lib/ (things you source) is the single most
impactful organizational decision for a growing script collection — it makes dependency
direction obvious and prevents accidental circular sourcing between scripts.

### Practical Examples

### DevOps Example — team-wide shellcheck enforcement

```bash
# .shellcheckrc (repo root)
shell=bash
disable=SC1091   # allow sourcing files shellcheck can't statically resolve
```

```bash
# CI step
find . -name '*.sh' -print0 | xargs -0 shellcheck
```

### 🎯 INTERVIEW TIP

"What makes a Bash script 'enterprise-ready' versus a personal script?" — Consistent
structure (bin/lib separation), a shared style guide, automated linting (shellcheck) in CI,
centralized logging, defensive error handling, and documentation — the same logic
works either way, but enterprise-ready scripts are safe for someone OTHER than the
author to run, modify, and debug.

### Chapter Summary

- A standard script template (strict mode, logging, traps, main() wrapper, getopts)

removes the blank-page problem and enforces best practices by default.

- A documented style guide for naming, quoting, and indentation keeps a growing

script library consistent and readable across contributors.

- Separating bin/ (executables) from lib/ (sourced libraries) and running shellcheck

in CI are the hallmarks of an enterprise-grade automation repository.

### Practice Questions

1. Why wrap script logic in a main() function instead of running commands top-to-
bottom?

2. What is the recommended separation between bin/ and lib/ in a script repository?

3. What tool should be run automatically in CI against every .sh file in a repository?

### HANDS-ON EXERCISE

Copy the production script template into a new file, rename it, and adapt it into a real
working script (of your choice) that validates an input file and logs its progress —
keeping the strict-mode header, traps, and main() structure intact.

## Interview Questions & Cheat Sheet Beginner through scenario-based, with model answers, plus a fast reference

### Learning Objectives

- Answer common Bash interview questions across difficulty levels confidently
- Reason through scenario-based, whiteboard-style Bash problems
- Use the consolidated cheat sheet as a fast day-to-day reference

### Beginner

- What's the difference between a shell and a script? — A shell is the interactive

interpreter program; a script is a text file of commands that a shell executes non-
interactively.

- How do you make a script executable? — chmod +x script.sh, then run it with

./script.sh.

- What does $? represent? — The exit code of the most recently executed

command.

- What's the difference between single and double quotes? — Single quotes

prevent ALL expansion; double quotes allow variable and command substitution
but block word splitting/globbing.

- How do you comment a line in Bash? — Start it with #.

### Intermediate

- Explain the difference between [ ] and [[ ]]. — [[ ]] is a Bash keyword that avoids

word splitting/globbing on unquoted variables and supports =~ regex and pattern
matching; [ ] is the older, POSIX-portable test command with none of those
protections.

- How do you loop through a file line by line safely? — while IFS= read -r line; do ...

done < file — never a for loop over $(cat file).

- What does set -euo pipefail do? — Exits on any failing command (-e), errors on

unset variables (-u), and makes a pipeline fail if any stage fails, not just the last (-o
pipefail).

- How do you pass arguments to a function? — They become $1, $2, ... and $@

inside the function, scoped to that call, just like script arguments.

- What's the difference between local and global variables in a function? —

Without local, a variable set inside a function is global and persists/overwrites
outside it; local scopes it to the function only.

### Advanced

- How would you debug a script that behaves differently under cron than

interactively? — Check $PATH and environment differences (cron's is minimal),
the working directory, and whether the script relies on .bashrc/.bash_profile,
which cron never sources.

- Why use trap for cleanup instead of code at the end of the script? — trap ... EXIT

fires on ANY exit path — normal completion, set -e triggering, or a signal like
Ctrl+C — while code at the bottom only runs if execution reaches it normally.

- How do you safely handle a variable that might be empty in an rm -rf

command? — Validate it's non-empty and matches an expected pattern before
use, quote it, and consider a --dry-run flag or explicit confirmation before any
destructive operation.

- What's the danger of parsing `ls` output in scripts? — Filenames can contain

spaces, newlines, or glob-special characters that break naive parsing; use globs
(for f in *.txt) or find -print0 | xargs -0 instead.

### Scenario-Based

- A production deploy script occasionally deletes more files than intended. Walk

through your triage. — Check whether the delete step uses an unquoted or
unvalidated variable, add -print before any -delete during testing, and confirm the
script validates its working directory/target path before any destructive action.

- You need to detect brute-force SSH attempts across 50 servers efficiently.

What's your approach? — Loop over hosts with SSH in parallel (backgrounded +
wait), grep 'Failed password' from each host's auth.log, aggregate counts by
source IP with sort | uniq -c | sort -rn, and flag any IP crossing a threshold.

## Cheat Sheet

Category
Quick Reference

Variables
var=val (no spaces) · "$var" to expand · export to inherit

Conditionals
[[ ]] preferred · -eq/-lt for numbers · == / != for strings

Loops
for x in list · while [[ ]] · until [[ ]] · break/continue

Functions
name() { ...; } · local var · return sets exit code only

Arrays
arr=(a b c) · "${arr[@]}" · declare -A for key-value

Redirection
> overwrite · >> append · 2>&1 merge · &> both

Strict mode
set -euo pipefail

File tests
-e exists · -f file · -d dir · -r/-w/-x perms · -s non-empty

Exit codes
0 = success · 1-255 = failure · $? checks last command

### Chapter Summary

- Interview questions cluster around quoting, [[ ]] vs [ ], safe file iteration, strict

mode, and function scoping.

- Scenario questions test debugging methodology as much as raw syntax

knowledge.

- The cheat sheet consolidates the fastest day-to-day reference for variables,

conditionals, loops, and redirection.

### Practice Questions

1. Why is [[ ]] generally safer than [ ] for conditions involving variables?

2. What's the standard, safe pattern for reading a file line by line?

3. What does trap ... EXIT guarantee that end-of-script cleanup code does not?

## Bash One-Liners & Learning Roadmap A grab-bag reference library, and a structured path from zero to expert

### Learning Objectives

- Build a personal library of high-value Bash one-liners
- Follow a structured, week-by-week roadmap from beginner to expert
- Identify real projects that consolidate everything this handbook covered

### Bash One-Liners — A Curated Reference

### System

```bash
df -h | awk '$5+0 > 80 {print $6, $5}'                 # partitions over 80% full
du -sh /* 2>/dev/null | sort -rh | head -10             # biggest top-level
directories
free -h                                                   # memory summary
uptime -p
```

### Files & Text

```bash
find . -type f -name '*.tmp' -mtime +7 -delete           # clean old temp files
find . -type f -exec md5sum {} + | sort                  # hash every file,
sorted
sort file.txt | uniq -c | sort -rn                        # frequency count of
lines
grep -rl 'TODO' --include='*.sh' .                        # files containing TODO
```

### Networking

```bash
curl -s -o /dev/null -w '%{http_code}\n' https://example.com   # just the HTTP
status
nc -zv host 22 2>&1 | grep succeeded                              # is a port
open
dig +short example.com                                            # quick DNS
lookup
```

### SOC / Security

```bash
last -20                                                   # recent login history
awk '{print $1}' access.log | sort | uniq -c | sort -rn | head  # top requesters
grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' file.txt | sort -u        # extract unique
IPs
find / -perm -4000 -type f 2>/dev/null                     # SUID binaries
ps aux --sort=-%cpu | head -6                               # top CPU consumers
```

### DevOps / Cloud

```bash
docker ps -q | xargs -r docker stats --no-stream            # live stats for
running containers
kubectl get pods -A | grep -v Running                        # pods not in
Running state
aws s3 ls s3://bucket --recursive --summarize | tail -2       # bucket size
summary
```

### Learning Roadmap

Phase
Weeks
Focus

Beginner
1-2
Shell basics, variables, if/case, loops, exit
codes

Intermediate
3-5
Strings, arrays, functions, file handling,
getopts, quoting mastery

Linux/DevOps Core
6-8
Users/permissions, processes, cron,
networking, SSH automation

Security Track
9-10
Log analysis, IOC extraction, IR scripts, FIM,
threat hunting

Cloud/DevOps Track
11-12
Docker/Kubernetes, CI/CD, AWS/Azure CLI,
backups

Professional
13+
Strict mode, error handling, debugging, style
guide, real projects

### Projects to Build

- A personal library of 15-20 saved functions (logging, retry, validation) sourced

across every script you write

- A full log-analysis triage script combining grep/awk/sort into a single 'top

offenders' report

- A backup script with verification, rotation, and S3 upload, scheduled via cron

- A basic IR playbook runner supporting triage, isolate, and collect subcommands

- A CI pipeline script for a small project, using set -euo pipefail and shellcheck in CI

## Recommended Practice Resources

- Bash manual: man bash and the official GNU Bash Reference Manual

- ShellCheck (shellcheck.net) — run against every script you write, without

exception

- Explainshell.com — paste any confusing command to see it broken down piece by

piece

- Practice environments for RHCSA / Linux+ / LPIC exam objectives

- Real logs from your own lab environment — nothing teaches log analysis like real,

messy data

### Closing Note

Bash fluency, like KQL fluency, comes from repetition against real problems — not from
memorizing syntax tables. Revisit Part 20 through Part 22 and Part 26 regularly, rebuild
the scripts from scratch without looking, and gradually replace every manual, repetitive
task in your day-to-day work with a script you understand line by line.

End of the Document

---

# Architecture Figures — Text Reference

## Figure 1.1 — Shell vs Kernel Architecture

1. **User** — interacts with the system using a terminal and types commands such as `ls`, `cd`, and `grep`.
2. **Terminal Emulator** — displays output and sends user input to the shell.
3. **Bash Shell** — parses commands, handles expansions, redirections, pipes, built-ins, environment variables, and starts/stops programs.
4. **Linux Kernel** — manages processes, memory, filesystems, networking, and device drivers, and talks to hardware resources.
5. **Hardware** — CPU, RAM, storage, network, and devices execute the work.

**Flow of interaction:** commands travel from user → terminal → Bash → kernel → hardware, and results travel back upward. The shell is the intermediary; users do not normally interact with the kernel directly.

## Figure 2.1 — Bash Command Execution Flow

1. **User input** — a command is entered.
2. **Tokenization** — Bash separates the command into tokens.
3. **Variable expansion** — variables are replaced with their values.
4. **Word splitting** — eligible unquoted expanded text is split according to shell rules/IFS.
5. **Globbing (pattern expansion)** — wildcard patterns are expanded to matching filenames.
6. **Command lookup** — Bash resolves the command as a builtin, alias/function where applicable, or executable.
7. **Builtin decision** — builtins execute inside the shell; external executables are located and launched.
8. **Execute process** — the command runs and produces output.
9. **Exit status** — the command returns an exit status; `0` indicates success and non-zero indicates failure.


---

# Extended Practical Examples

## 01 — Linux Shells & Bash Architecture

### Example — Confirm the interpreter actually running

```bash
printf 'Login shell: %s\n' "$SHELL"
printf 'Current shell process: '
ps -p "$$" -o comm=
printf 'Bash version: %s\n' "${BASH_VERSION:-not-bash}"
```

### Example — Check whether a command is a builtin, function, alias, or executable

```bash
type cd
type printf
type grep
command -V kubectl
```

## 02 — Scripts, Variables, Input & Operators

### Example — Validate numeric input before arithmetic

```bash
#!/usr/bin/env bash
read -r -p "Enter replica count: " replicas

if [[ ! "$replicas" =~ ^[0-9]+$ ]]; then
  printf 'ERROR: replica count must be an integer\n' >&2
  exit 2
fi

printf 'Double capacity: %d\n' "$((replicas * 2))"
```

### Example — Keep stdout machine-readable and errors on stderr

```bash
if ! output=$(some_command 2>error.log); then
  printf 'Command failed; see error.log\n' >&2
  exit 1
fi
printf '%s\n' "$output"
```

## 03 — Quoting, Environment Variables & Conditionals

### Example — Safe filename handling

```bash
file='Quarterly Report 2026.txt'
if [[ -f "$file" ]]; then
  printf 'Found: %s\n' "$file"
fi
```

### Example — Parameter defaults

```bash
environment=${ENVIRONMENT:-dev}
timeout=${TIMEOUT_SECONDS:-15}
printf 'env=%s timeout=%ss\n' "$environment" "$timeout"
```

## 04 — Loops, Flow Control & Exit Codes

### Example — Retry with exponential backoff

```bash
max_attempts=5
for ((attempt=1; attempt<=max_attempts; attempt++)); do
  if curl -fsS --max-time 5 https://example.com/health >/dev/null; then
    echo "Healthy"
    break
  fi

  if (( attempt == max_attempts )); then
    echo "Health check failed" >&2
    exit 1
  fi

  sleep_seconds=$((2 ** (attempt - 1)))
  echo "Retrying in ${sleep_seconds}s..." >&2
  sleep "$sleep_seconds"
done
```

## 05 — Strings & Arrays

### Example — Parse an image reference without external tools

```bash
image='registry.example.com/team/frontend:1.8.3'
repo=${image%:*}
tag=${image##*:}
name=${repo##*/}
printf 'repo=%s name=%s tag=%s\n' "$repo" "$name" "$tag"
```

### Example — Associative array for service ports

```bash
declare -A ports=(
  [frontend]=8080
  [payments]=8081
  [catalog]=8082
)

for service in "${!ports[@]}"; do
  printf '%-12s %s\n' "$service" "${ports[$service]}"
done
```

## 06 — Functions & File Handling

### Example — Reusable dependency check

```bash
require_command() {
  local cmd=$1
  command -v "$cmd" >/dev/null 2>&1 || {
    printf 'ERROR: required command missing: %s\n' "$cmd" >&2
    return 1
  }
}

require_command curl
require_command jq
```

### Example — Safe temporary workspace

```bash
workdir=$(mktemp -d)
cleanup() { rm -rf -- "$workdir"; }
trap cleanup EXIT INT TERM
printf 'workspace=%s\n' "$workdir"
```

## 07 — Command-Line Arguments & Here Documents

### Example — Long options with a case/shift loop

```bash
verbose=0
environment=dev

while (($#)); do
  case "$1" in
    --verbose) verbose=1; shift ;;
    --environment) environment=${2:?missing value}; shift 2 ;;
    --environment=*) environment=${1#*=}; shift ;;
    --) shift; break ;;
    *) printf 'Unknown option: %s\n' "$1" >&2; exit 2 ;;
  esac
done
```

### Example — Generate JSON safely with `jq`

```bash
name='frontend'
tag='42'
jq -n --arg name "$name" --arg tag "$tag" '{service:$name, tag:$tag}'
```

## 08 — Users & Permissions

### Example — Read-only user audit

```bash
while IFS=: read -r user _ uid gid _ home shell; do
  if (( uid >= 1000 )); then
    printf '%-20s uid=%s gid=%s home=%s shell=%s\n' "$user" "$uid" "$gid" "$home" "$shell"
  fi
done < /etc/passwd
```

### Example — Audit files writable by others

```bash
find /opt/app -xdev -type f -perm -0002 -print
```

## 09 — Processes, Services & System Automation

### Example — Service state with journal context

```bash
service=nginx.service
if ! systemctl is-active --quiet "$service"; then
  echo "$service is not active" >&2
  journalctl -u "$service" -n 50 --no-pager >&2
  exit 1
fi
```

### Example — Top CPU consumers

```bash
ps -eo pid,user,%cpu,%mem,etime,args --sort=-%cpu | head -n 11
```

## 10 — Cron Jobs, Logs & Networking

### Example — Avoid overlapping cron runs with `flock`

```cron
*/5 * * * * /usr/bin/flock -n /run/lock/api-check.lock /opt/bin/api-check.sh >> /var/log/api-check.log 2>&1
```

### Example — Network dependency check

```bash
host=database.internal
port=5432
if timeout 3 bash -c "</dev/tcp/$host/$port" 2>/dev/null; then
  echo "$host:$port reachable"
else
  echo "$host:$port unreachable" >&2
fi
```

## 11 — SSH Automation

### Example — Fleet command with safe non-interactive behavior

```bash
while IFS= read -r host; do
  [[ -n "$host" ]] || continue
  ssh -o BatchMode=yes -o ConnectTimeout=5 "$host" 'hostname; uptime -p'
done < hosts.txt
```

### Example — Copy and verify a deployment artifact

```bash
scp -o BatchMode=yes release.tar.gz deploy@web01:/tmp/
ssh deploy@web01 'sha256sum /tmp/release.tar.gz'
```

## 12 — Log Analysis & IOC Extraction

### Example — HTTP status distribution

```bash
awk '{count[$9]++} END {for (code in count) print code, count[code]}' access.log | sort -n
```

### Example — Top source IPs generating 5xx responses

```bash
awk '$9 ~ /^5/ {print $1}' access.log | sort | uniq -c | sort -rn | head -20
```

## 13 — Security Automation & Incident Response

### Example — Collect volatile host triage data

```bash
case_dir="triage-$(hostname)-$(date +%Y%m%d_%H%M%S)"
mkdir -p "$case_dir"
ps auxww > "$case_dir/processes.txt"
ss -plant > "$case_dir/network.txt"
who -a > "$case_dir/sessions.txt"
last -n 100 > "$case_dir/last.txt"
sha256sum "$case_dir"/* > "$case_dir/SHA256SUMS"
```

Use incident-response automation only under appropriate authorization and preserve evidence-handling requirements.

## 14 — File Integrity Monitoring & Threat Hunting

### Example — Build and compare checksums

```bash
find /etc/myapp -type f -print0 | sort -z | xargs -0 sha256sum > baseline.sha256
sha256sum -c baseline.sha256
```

### Example — Recent executable files in temporary directories

```bash
find /tmp /var/tmp /dev/shm -xdev -type f -perm /111 -mtime -2 -ls 2>/dev/null
```

## 15 — Docker & Kubernetes Automation

### Example — Graceful Docker entrypoint

```bash
#!/usr/bin/env bash
set -e

if [[ -n "${APP_CONFIG:-}" ]]; then
  echo "Using configuration: $APP_CONFIG"
fi

exec "$@"
```

### Example — Kubernetes rollout validation

```bash
namespace=production
deployment=frontend

kubectl -n "$namespace" apply -f deployment.yaml
kubectl -n "$namespace" rollout status "deployment/$deployment" --timeout=5m
kubectl -n "$namespace" get pods -l app="$deployment" -o wide
```

## 16 — CI/CD, AWS & Azure CLI Automation

### Example — Fail a CI job if a Kubernetes rollout fails

```bash
set -euo pipefail
kubectl apply -f k8s/
kubectl rollout status deployment/frontend --timeout=300s
```

### Example — AWS output without parsing display text

```bash
aws ec2 describe-instances \
  --filters Name=instance-state-name,Values=running \
  --query 'Reservations[].Instances[].[InstanceId,PrivateIpAddress]' \
  --output text
```

### Example — Azure resource group inventory

```bash
az group list --query '[].{name:name,location:location}' -o table
```

## 17 — Backup Automation

### Example — Backup, checksum, verify, rotate

```bash
set -euo pipefail
src=/srv/app
dest=/backup
stamp=$(date +%Y%m%d_%H%M%S)
archive="$dest/app_${stamp}.tar.gz"

mkdir -p "$dest"
tar -czf "$archive" -C "$src" .
sha256sum "$archive" > "$archive.sha256"
tar -tzf "$archive" >/dev/null
sha256sum -c "$archive.sha256"
find "$dest" -type f -name 'app_*.tar.gz*' -mtime +14 -delete
```

## 18 — Defensive Coding, Error Handling & Debugging

### Example — Production-oriented script skeleton

```bash
#!/usr/bin/env bash
set -Eeuo pipefail

trap 'printf "ERROR line=%s command=%q status=%s\\n" "$LINENO" "$BASH_COMMAND" "$?" >&2' ERR

main() {
  : "${CONFIG_FILE:?CONFIG_FILE must be set}"
  [[ -r "$CONFIG_FILE" ]] || {
    echo "Cannot read $CONFIG_FILE" >&2
    return 1
  }
}

main "$@"
```

### Example — Syntax and static checks

```bash
bash -n script.sh
shellcheck script.sh
```

## 19 — Performance, Logging, Templates & Bash Style Guide

### Example — Structured key=value logging

```bash
log() {
  local level=$1; shift
  printf 'ts=%q level=%q msg=%q\n' "$(date --iso-8601=seconds)" "$level" "$*"
}

log INFO "deployment started"
```

### Example — Avoid repeated external commands in a hot loop

```bash
# Better: compute constant data once before the loop.
hostname_value=$(hostname)
for file in *.log; do
  printf '%s %s\n' "$hostname_value" "$file"
done
```

## 20 — Enterprise Folder Structure, Interview Questions & Cheat Sheet

### Example — Team-ready repository layout

```text
automation/
├── bin/
│   ├── deploy.sh
│   └── backup.sh
├── lib/
│   ├── logging.sh
│   └── validation.sh
├── config/
│   ├── dev.env.example
│   └── prod.env.example
├── tests/
│   └── smoke.sh
├── docs/
│   └── operations.md
├── .shellcheckrc
├── Makefile
└── README.md
```

### Example — Minimal validation pipeline

```bash
find bin lib tests -type f -name '*.sh' -print0 |
  xargs -0 -n1 bash -n

find bin lib tests -type f -name '*.sh' -print0 |
  xargs -0 shellcheck
```

---

# Technical Validation Notes

- In Bash arithmetic, `**` is exponentiation. `^` is bitwise XOR. Example: `echo $((2 ** 8))` prints `256`; `echo $((5 ^ 3))` performs XOR.
- A `case` branch normally ends with `;;`. Omitting the required terminator is generally a syntax error; Bash also supports explicit `;&` and `;;&` terminators for fall-through-style behavior.
- Bash is widespread but is not guaranteed in every container image. Minimal images such as Alpine commonly provide BusyBox `sh`/`ash` unless Bash is installed.
- Prefer `command -v name` in scripts when checking command availability; it handles shell builtins/functions as well as executable lookup more reliably than assumptions about `which`.
- `set -e` has contextual exceptions. Treat `set -Eeuo pipefail` as a useful safety baseline, not a substitute for explicit error checks around critical operations.
- `grep -P` is a GNU grep feature and is not universally portable. For highly portable scripts, prefer POSIX-compatible patterns or use tools available on the target platform.
- Linux command options differ across GNU/BSD/BusyBox implementations. Validate commands such as `stat`, `date`, `sed -i`, `find`, and `timeout` on the target operating system/container image.

---

# Research References

- GNU Bash Reference Manual: https://www.gnu.org/software/bash/manual/bash.html
- GNU Coreutils Manual: https://www.gnu.org/software/coreutils/manual/coreutils.html
- GNU grep Manual: https://www.gnu.org/software/grep/manual/grep.html
- GNU sed Manual: https://www.gnu.org/software/sed/manual/sed.html
- GNU gawk User's Guide: https://www.gnu.org/software/gawk/manual/gawk.html
- systemd `systemctl`: https://www.freedesktop.org/software/systemd/man/systemctl.html
- systemd `journalctl`: https://www.freedesktop.org/software/systemd/man/journalctl.html
- Kubernetes kubectl reference: https://kubernetes.io/docs/reference/kubectl/
- Kubernetes Deployments: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- Dockerfile reference: https://docs.docker.com/reference/dockerfile/
- AWS CLI User Guide: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html
- Azure CLI documentation: https://learn.microsoft.com/cli/azure/
- ShellCheck: https://www.shellcheck.net/
