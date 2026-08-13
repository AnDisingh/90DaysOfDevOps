 # The core components of Linux (kernel, user space, init/systemd)

# KERNEL
 The Kernel is the Heart of Linux.
It is the layer that communicate directly with Hardware that manages the system's resources.
Kernel resources CPU,Memory,Processes,Disk,Network,Devices,security.

# User Space
User space is the area where normal applications, commands, shells, and services execute with limited privileges and interact with the kernel through system calls.
</> Bash
ls
cd
cp
mv
cat
grep
vim
bash
python
nginx
apache

# Init
When Linux boots, the kernel starts the first user-space process.
Init it has PID = 1
PID means process Id
you can check with this command -    ps -p 1
On a modern linux system PID 1 is systemd

# Systemd
systemd is a modern Linux init and service-management system. It manages the boot process, services, processes, mounts, and system states.
Main responsibilities
systemd
   │
   ├── Boot management
   ├── Service management
   ├── Process management
   ├── Logging integration
   ├── Mount management
   ├── Network-related management
   └── System states/targets

   It manages system services:
   systemctl status nginx
   systemctl start nginx
   systemctl stop nginx

   # Why does it matter?

   Without a service manager, starting and managing many services manually would be difficult.

Kernel manages hardware and system resources.
User space runs applications and commands.
systemd runs as PID 1 and manages the boot process and services.

# How processes are created and managed

Process - A process is a running instance of a program.
python3 app.py - app.py is the program, and when it runs, Linux creates a process.
# How is a Process Created? 
Processes are usually created by another process.
Parent Process
      ↓
   fork()
      ↓
Child Process
      ↓
   exec()
      ↓
New Program

# How Processes Are Managed?
The Linux kernel manages processes by:

Scheduling CPU time
Managing memory
Changing process states
Starting and stopping processes
Usefull Command -
 ps       # View processes
top       # Monitor processes
kill PID  # Stop a process

# Process States
Running
Sleeping
Stopped
Zombie

# List **5 commands** you would use daily
ls              # Files
cd /path        # Navigate
ps -ef          # Processes
df -h           # Disk
systemctl status nginx  # Services

Happy Learning
