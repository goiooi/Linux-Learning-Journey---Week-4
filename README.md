# Linux Learning Journey - Week 4

## Topics: Basic Scripting, Computer Hardware, and Linux Data Storage

## Introduction

During Week 4, I studied three major Linux topics: basic shell scripting, computer hardware, and where Linux stores system and process information.

I learned how to automate commands using Bash scripts, use variables and user input, apply conditionals and loops, identify computer hardware components, monitor memory and processes, understand system logs, and explore the Linux filesystem hierarchy.

These topics helped me understand how Linux commands can be combined to automate tasks and how the operating system interacts with hardware and system data.

## Module 11: Basic Scripting

### 1. Introduction to Shell Scripts

A shell script is a text file containing commands that execute sequentially when the script runs.

Shell scripts help automate repetitive tasks, reduce manual work, and make command execution more consistent.

Example:

```bash
#!/bin/bash
echo "Hello, World!"
```

The script displays:

```text
Hello, World!
```

### 2. Running Shell Scripts

A script can be executed by passing it to a shell interpreter:

```bash
sh test.sh
```

Alternatively, it can be executed directly:

```bash
./test.sh
```

To execute a script directly, it generally needs executable permission.

```bash
chmod +x test.sh
```

The `chmod +x` command adds executable permission.

### 3. The Shebang

The shebang identifies the interpreter used when a script is executed directly.

Examples:

```bash
#!/bin/sh
```

```bash
#!/bin/bash
```

The shebang must appear at the beginning of the script.

### 4. Editing Scripts with Nano

The `nano` editor can be used to create and edit shell scripts.

```bash
nano test.sh
```

Useful Nano keyboard shortcuts include:

| Shortcut | Function |
|---|---|
| `Ctrl + O` | Save the file |
| `Ctrl + X` | Exit Nano |
| `Ctrl + W` | Search text |
| `Ctrl + K` | Cut a line |
| `Ctrl + U` | Paste previously cut text |
| `Ctrl + G` | Display help |
| `Ctrl + Y` | Move one page up |
| `Ctrl + V` | Move one page down |

When saving a file, Nano may ask for confirmation of the filename.

### 5. Variables in Bash

Variables store information that can be reused within a script.

Example:

```bash
#!/bin/bash

ANIMAL="penguin"
echo "My favorite animal is a $ANIMAL"
```

Output:

```text
My favorite animal is a penguin
```

Important rules:

- Do not put spaces around the `=` sign when assigning a variable.
- Use `$` to access a variable's value.
- Variable names are case-sensitive.
- Quoting variable expansions is good practice when values may contain spaces.

Another example:

```bash
#!/bin/bash

ANIMAL="penguin"
SOMETHING="$ANIMAL"

echo "$SOMETHING"
```

### 6. Command Substitution

Command substitution stores the output of a command in a variable.

The course material demonstrates backticks:

```bash
CURRENT_DIRECTORY=`pwd`
echo "You are in $CURRENT_DIRECTORY"
```

A modern alternative is:

```bash
CURRENT_DIRECTORY=$(pwd)
echo "You are in $CURRENT_DIRECTORY"
```

Both forms capture the output of `pwd`, which displays the current working directory.

### 7. Receiving User Input

The `read` command receives input from the keyboard.

Example:

```bash
#!/bin/bash

echo -n "What is your name? "
read NAME
echo "Hello $NAME!"
```

The script asks for the user's name and displays a personalized greeting.

### 8. Script Arguments

Arguments allow information to be passed to a script when it runs.

Example:

```bash
#!/bin/bash

echo "Hello $1"
```

Run the script:

```bash
./test.sh World
```

Output:

```text
Hello World
```

Important special variables include:

| Variable | Meaning |
|---|---|
| `$0` | Script name or invocation path |
| `$1` | First argument |
| `$2` | Second argument |
| `$?` | Exit status of the previous command |

### 9. Exit Codes

Exit codes indicate whether a command completed successfully.

- `0` generally indicates success.
- A nonzero value indicates an error or another unsuccessful result, depending on the command.

Example:

```bash
grep -q root /etc/passwd
echo $?
```

The `-q` option runs `grep` quietly without displaying matching lines.

For `grep`, an exit code of `0` means a match was found, `1` means no match was found, and other nonzero codes can indicate an error.

A script can set its own exit code:

```bash
#!/bin/bash

# Something bad happened!
exit 1
```

### 10. Comments in Scripts

Comments explain what a script does. Bash ignores the comment text following `#` outside quoted strings.

Example:

```bash
#!/bin/bash

# Display a welcome message
echo "Welcome to Linux"
```

### 11. Conditionals

Conditionals allow scripts to make decisions based on command results or tests.

Basic structure:

```bash
if command; then
    # Commands executed when successful
fi
```

Example using `grep`:

```bash
#!/bin/bash

if grep -q root /etc/passwd; then
    echo "root is in the password file"
else
    echo "root is missing from the password file"
fi
```

The `if` statement checks the exit status of `grep` and chooses the appropriate branch.

#### The Test Command

The `test` command checks files, strings, and numeric values.

Examples:

```bash
test -f /etc/passwd
test -d /tmp
test 1 -eq 1
test "linux" = "linux"
```

Common operators include:

| Operator | Purpose |
|---|---|
| `-f` | Tests whether a regular file exists |
| `-d` | Tests whether a directory exists |
| `-x` | Tests whether a file is executable |
| `-eq` | Numeric equality |
| `-ne` | Numeric inequality |
| `-gt` | Numeric greater than |
| `=` | String equality |
| `!=` | String inequality |
| `!` | Negates a test |

Square brackets provide an alternative syntax for `test`.

```bash
if [ -f /etc/passwd ]; then
    echo "File exists"
fi
```

Spaces inside the test expression are important, including the space before the closing `]`.

#### Using `elif`

The `elif` keyword allows additional conditions to be tested.

```bash
#!/bin/bash

if [ "$1" = "hello" ]; then
    echo "hello yourself"
elif [ "$1" = "goodbye" ]; then
    echo "nice to have met you"
else
    echo "I didn't understand that"
fi
```

#### Using `case`

The `case` statement is useful when several possible values must be handled.

```bash
#!/bin/bash

case "$1" in
    hello|hi)
        echo "hello yourself"
        ;;
    goodbye)
        echo "nice to have met you"
        ;;
    *)
        echo "I didn't understand that"
        ;;
esac
```

The `*` pattern acts as a default when none of the preceding patterns match.

### 12. Loops

Loops allow commands to execute repeatedly.

#### For Loop

A `for` loop processes a list of values.

```bash
#!/bin/bash

SERVERS="servera serverb serverc"

for SERVER in $SERVERS; do
    echo "Processing $SERVER"
done
```

A loop can also process a list directly:

```bash
for NAME in Sean Jon Isaac David; do
    echo "Hello $NAME"
done
```

A filename pattern can be used to process entries in the current directory:

```bash
for FILE in *; do
    echo "Processing $FILE"
done
```

#### While Loop

A `while` loop repeats as long as its condition remains true.

```bash
#!/bin/bash

i=0

while [ "$i" -lt 10 ]; do
    echo "$i"
    i=$((i + 1))
done

echo "Done counting"
```

The loop displays the numbers from `0` through `9`.

### Module 11 Summary

In this module, I learned how to:

- Create and execute shell scripts.
- Use the Nano text editor.
- Store and access variables.
- Capture command output.
- Receive user input and script arguments.
- Interpret exit codes.
- Use comments, conditionals, and loops.
- Automate repetitive tasks using Bash.

---

## Module 12: Understanding Computer Hardware

### 1. Introduction to Computer Hardware

Computer hardware consists of the physical components that allow a computer to operate. Linux administration requires an understanding of hardware to install, configure, monitor, secure, and troubleshoot systems.

### 2. Motherboard

The motherboard, also called the system board, connects the CPU, RAM, storage devices, and other components.

It provides the connections and communication pathways required for these components to work together.

### 3. Processors

The Central Processing Unit (CPU) executes instructions and performs calculations.

The material discusses two common processor architectures:

- `x86`: a 32-bit architecture.
- `x86_64`: a 64-bit architecture that can also support 32-bit operation.

Useful commands:

```bash
arch
```

Displays the system architecture.

```bash
lscpu
```

Displays detailed CPU information, including architecture, CPU count, and processor configuration.

### 4. Random Access Memory

Random Access Memory (RAM) temporarily stores data used by running programs and the operating system.

Insufficient RAM can affect system performance.

Linux can use swap space as additional virtual memory when physical memory is under pressure. Swap is generally slower than RAM.

Useful commands:

```bash
free
```

Displays memory and swap statistics.

```bash
free -m
```

Displays memory information in megabytes.

```bash
free -g
```

Displays memory information in gigabytes, rounded to whole numbers.

```bash
free -s 10
```

Refreshes the memory statistics at ten-second intervals.

### 5. Buses and Peripheral Devices

A bus provides a communication pathway between computer components.

Examples include:

- PCI and PCI Express connections.
- USB connections.

Peripheral devices include keyboards, mice, monitors, printers, and storage devices.

The course material introduces these commands:

```bash
lspci
```

Lists devices connected through the PCI bus.

```bash
lsusb
```

Lists connected USB devices.

Their availability may depend on the Linux environment and installed utilities.

### 6. Storage Devices and Partitions

Storage devices preserve data when a computer is powered off.

The material discusses hard disk drives (HDDs), solid-state drives (SSDs), and optical storage devices.

Linux storage can be divided into partitions.

Two partitioning schemes covered are:

- MBR: Master Boot Record.
- GPT: GUID Partitioning Table.

GPT supports larger disks and more partitions than traditional MBR partitioning.

The course introduces tools such as `fdisk`, `gdisk`, `parted`, and `gparted` for working with partitions. Administrative privileges are generally required for partition-management operations.

**Important:** Partition changes can destroy data. This documentation covers the concepts, not instructions to modify a live disk.

### Module 12 Summary

In this module, I learned about:

- Motherboards and CPUs.
- Processor architectures.
- RAM and swap space.
- Buses and peripheral devices.
- HDDs, SSDs, and optical drives.
- MBR and GPT partitioning.
- Commands for inspecting CPU, memory, and hardware information.

---

## Module 13: Where Data Is Stored

### 1. The Linux Kernel

The Linux kernel is the core component responsible for managing system resources and coordinating communication between software and hardware.

The course material explains that Linux systems use the kernel alongside GNU software and other utilities to provide a complete operating environment.

### 2. The `/proc` and `/sys` Directories

Linux provides pseudo-filesystems that expose information about processes, the kernel, and hardware.

#### `/proc`

The `/proc` filesystem provides information about running processes, memory, and kernel configuration.

Examples:

```bash
ls /proc
```

Lists entries in `/proc`.

```bash
cat /proc/meminfo
```

Displays detailed memory information.

```bash
cat /proc/cmdline
```

Displays the kernel command-line parameters used during startup.

#### `/sys`

The `/sys` filesystem exposes information about devices and the kernel's device model.

These pseudo-filesystems are not ordinary collections of files stored on disk. Much of their content is generated dynamically by the running kernel.

### 3. Process Hierarchy

A process is a running instance of a program.

Each process has a Process ID (PID). Processes can have parent-child relationships, and the parent process ID is known as the PPID.

The course material explains that PID 1 is assigned to the system's initial process, which starts or manages other processes. On modern Linux systems, this is commonly handled by `systemd`.

Useful command:

```bash
pstree
```

Displays processes in a tree structure.

### 4. Viewing Processes with `ps`

The `ps` command displays a snapshot of processes.

```bash
ps
```

Displays processes associated with the current terminal or shell context.

```bash
ps aux
```

Displays a broad list of running processes using BSD-style options.

```bash
ps -ef
```

Displays processes using the full-format listing.

```bash
ps --forest
```

Displays process relationships in a tree-like format on systems supporting this option.

Process output can be filtered using `grep`.

```bash
ps aux | grep firefox
```

Searches the process listing for lines containing `firefox`.

### 5. Monitoring Processes with `top`

The `top` command displays a continuously updating view of running processes and system activity.

```bash
top
```

It helps administrators monitor CPU usage, memory usage, and processes that consume significant resources.

Press `q` to exit.

### 6. Monitoring Memory

The `free` command displays current memory statistics.

```bash
free
```

The output includes total, used, free, and available memory information, along with swap statistics where applicable.

```bash
free -m
```

Displays the information in megabytes.

Memory monitoring helps identify resource pressure and understand whether swap space is being used.

### 7. System Logs

Logs record events generated by the kernel and system services. They are useful for troubleshooting errors, investigating system behavior, and reviewing security-related events.

The `/var/log` directory commonly stores log files.

Examples discussed in the course material include:

| Log or Location | Purpose |
|---|---|
| `/var/log/boot.log` | Messages related to system startup |
| `/var/log/dmesg` | Kernel messages recorded during startup on systems that provide this file |
| `/var/log/messages` | General system messages on systems using this log |
| `/var/log/secure` | Security-related messages on systems using this log |
| `/var/log/journal` | Persistent systemd journal data, when configured |

Log locations and filenames vary by Linux distribution.

Useful commands:

```bash
less /var/log/boot.log
```

Views a text log one page at a time.

```bash
journalctl
```

Displays logs managed by the systemd journal.

```bash
dmesg
```

Displays kernel ring-buffer messages.

To search kernel messages for USB-related entries:

```bash
dmesg | grep -i usb
```

The `-i` option makes the search case-insensitive.

### 8. Filesystem Hierarchy Standard

The Filesystem Hierarchy Standard (FHS) describes the conventional organization of directories in Linux.

Important directories include:

| Directory | Purpose |
|---|---|
| `/` | Root of the filesystem |
| `/bin` | Essential command binaries on systems using this layout |
| `/boot` | Files needed for system boot |
| `/dev` | Device files |
| `/etc` | System configuration files |
| `/home` | Regular users' home directories |
| `/root` | Root user's home directory |
| `/lib` | Essential shared libraries on systems using this layout |
| `/media` | Common mount point for removable media |
| `/mnt` | Common mount point for temporary manual mounts |
| `/opt` | Optional or third-party software |
| `/proc` | Process and kernel information |
| `/sbin` | Essential system administration binaries on systems using this layout |
| `/sys` | Kernel and device information |
| `/tmp` | Temporary files |
| `/usr` | Applications, utilities, libraries, and shared data |
| `/usr/local` | Software installed locally by the administrator |
| `/var` | Variable data, including logs and caches |
| `/var/log` | Common location for system logs |

The exact organization may vary between distributions, especially on modern systems that merge some traditional directories.

### Module 13 Summary

In this module, I learned how Linux exposes process and hardware information, how to inspect running processes, how to monitor CPU and memory activity, where system logs are commonly stored, and how the filesystem hierarchy organizes data.

---

## Practical Exercises

The following exercises bring together the concepts covered during Week 4.

### Exercise 1: Create and Run a Script

```bash
mkdir -p ~/linux-week4
cd ~/linux-week4
nano hello.sh
```

Enter the following script:

```bash
#!/bin/bash

echo "Welcome to my Linux learning journey"
echo "Current directory: $(pwd)"
```

Save the file, then run:

```bash
chmod +x hello.sh
./hello.sh
```

### Exercise 2: Use Variables and User Input

Create a script named `greeting.sh`:

```bash
#!/bin/bash

echo -n "Enter your name: "
read NAME

echo "Hello, $NAME!"
```

Run it:

```bash
bash greeting.sh
```

### Exercise 3: Use a Conditional

```bash
#!/bin/bash

if [ -f "$1" ]; then
    echo "The file exists"
else
    echo "The file does not exist"
fi
```

Save it as `check-file.sh` and run:

```bash
bash check-file.sh hello.sh
```

This exercise demonstrates how a script can use a command-line argument and a file test to make a decision.

### Exercise 4: Inspect System Information

```bash
arch
lscpu
free -m
ps -ef
```

These commands provide information about the processor architecture, CPU configuration, memory, and running processes.

### Exercise 5: Explore Linux System Data

```bash
ls /proc
cat /proc/meminfo
ls /sys
```

These commands introduce the pseudo-filesystems used to expose kernel, memory, and device information.

## Key Lessons from Week 4

Week 4 helped me understand that Linux knowledge extends beyond individual commands.

Shell scripting allows commands to be combined into reusable programs. Variables and arguments provide flexibility, conditionals support decision-making, and loops automate repetitive operations.

The hardware module introduced the components that Linux manages, while the system-data module demonstrated how processes, memory, logs, and filesystem directories can be inspected using command-line tools.

## Reflection

This week strengthened my understanding of Linux as both an operating system and an environment for automation and system administration.

I gained a foundation in Bash scripting and learned how to use variables, user input, arguments, conditionals, and loops. I also explored how Linux interacts with hardware and how system information can be accessed through commands and pseudo-filesystems.

These skills provide a foundation for future Linux administration projects, including the development of scripts that automate file management, backup tasks, and system checks.

## Conclusion

Week 4 covered Basic Scripting, Understanding Computer Hardware, and Where Data Is Stored.

The knowledge gained will support my continued Linux learning journey and help me progress from running commands individually to building reusable scripts and understanding how Linux systems operate internally.
