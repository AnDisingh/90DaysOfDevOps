

1. Check Running Processes 
Command - ps aux
What I learned

ps aux displays currently running processes, including the user,
PID, CPU usage, memory usage, and command.

I also used top to monitor processes in real time.

2. Inspect a systemd Service

Command - systemctl status ssh



What I learned

systemctl status is used to inspect the current state of a systemd
service.

It can show whether the service is active, inactive, failed, and
provide recent log information.

3. Troubleshooting Flow

Check whether the SSH service is running.
Comman -
systemctl status ssh
ps aux | grep ssh
ss -tulnp
journalctl -u ssh --no-pager -n 20

Troubleshooting approach
Check the service status.
Check whether the related process is running.
Check listening network ports.
Check recent service logs.

This gives a basic flow for identifying whether a service,
process, network port, or logs indicate a problem.


### Process Checks
Command:
ps aux

Result:
PASS

Observation:
Processes are running normally. VS Code Server Node.js processes
are the main memory consumers.

Command:
ps aux --sort=-%mem | head

Result:
PASS

Observation:
PID 104 was the highest memory-consuming process at approximately
2.0% MEM.

### Service Checks

Command:
systemctl status ssh

Result:
NOT AVAILABLE

Observation:
WSL is not running systemd as PID 1. PID 1 is /init.

Command:
systemctl list-units --type=service --state=running

Result:
NOT AVAILABLE

Observation:
systemctl cannot communicate with systemd in this WSL environment.

### Log Checks

Command:
journalctl -u ssh -n 50

Result:
NO JOURNAL

Observation:
No journal files are available because systemd/journald is not
running.

Command:
journalctl -p err -n 50

Result:
NO JOURNAL

Observation:
No journal files were found.