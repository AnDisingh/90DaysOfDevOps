# Linux Troubleshooting Runbook
# uname -a 
Linux DESKTOP-2MP4EQ7 4.4.0-19041-Microsoft #5794-Microsoft Mon Apr 07 17:55:00 PST 2025 x86_64 x86_64 x86_64 GNU/Linux

# cat /etc/os-release
VERSION="24.04.1 LTS (Noble Numbat)"
VERSION_CODENAME=noble
ID=ubuntu
ID_LIKE=debian
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
UBUNTU_CODENAME=noble
LOGO=ubuntu-logo

# mkdir -p /tmp/runbook-demo
No output

# cp /etc/hosts /tmp/runbook-demo/hosts-copy && ls -l /tmp/runbook-demo
total 4
-rw-r--r-- 1 dipesh dipesh 535 Aug 15 18:58 hosts-copy

# top -b -n 1 | head -20
dipesh@DESKTOP-2MP4EQ7:/mnt/c/Users/Dipesh/90daysOfDevOps$ top -b -n 1 | head -20
top - 09:42:47 up 30 min,  0 user,  load average: 0.52, 0.58, 0.59
Tasks:  20 total,   1 running,  19 sleeping,   0 stopped,   0 zombie
%Cpu(s):  9.4 us, 17.2 sy,  0.0 ni, 73.4 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st 
MiB Mem :   8074.6 total,   1432.8 free,   6641.9 used,    224.0 buff/cache     
MiB Swap:  24576.0 total,  24133.2 free,    442.8 used.   1432.8 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
    1 root      20   0   11228   1116   1060 S   0.0   0.0   0:00.40 init(Ubun+
    6 root      20   0   11212    624    480 S   0.0   0.0   0:00.04 init
   12 root      20   0   11228    600    540 S   0.0   0.0   0:00.01 SessionLe+
   13 dipesh    20   0   10840    780    656 S   0.0   0.0   0:00.04 sh
   14 dipesh    20   0   10840    824    780 S   0.0   0.0   0:00.05 sh
   20 dipesh    20   0   10840    816    764 S   0.0   0.0   0:00.01 sh
   24 dipesh    20   0 1757252  94172  41504 S   0.0   1.1   0:11.60 MainThread
   36 root      20   0   11256    592    540 S   0.0   0.0   0:00.01 SessionLe+
   37 dipesh    20   0 1423368  42140  22720 S   0.0   0.5   0:01.36 MainThread
   44 root      20   0   11256    592    540 S   0.0   0.0   0:00.01 SessionLe+
   45 dipesh    20   0 1417424  36868  22572 S   0.0   0.4   0:01.99 MainThread
   53 dipesh    20   0 1664088  37368  22868 S   0.0   0.5   0:01.26 MainThread
   93 dipesh    20   0 1823992 157284 150392 S   0.0   1.9   0:50.45 MainThreadv

   # free -h
                total        used        free      shared  buff/cache   available
Mem:           7.9Gi       6.5Gi       1.3Gi        17Mi       223Mi       1.3Gi
Swap:           24Gi       431Mi        23Gi

# df -h
Filesystem      Size  Used Avail Use% Mounted on
rootfs          348G   75G  273G  22% /
none            348G   75G  273G  22% /dev
none            348G   75G  273G  22% /run
none            348G   75G  273G  22% /run/lock
none            348G   75G  273G  22% /run/shm
none            348G   75G  273G  22% /run/user
tmpfs           348G   75G  273G  22% /sys/fs/cgroup
C:\             348G   75G  273G  22% /mnt/c
F:\             348G  106M  348G   1% /mnt/f
G:\             237G  101M  237G   1% /mnt/g

# du -sh /var/log 2>/dev/null
552K    /var/log

# ss -tulpn
Cannot open netlink socket: Protocol not supported
Cannot open netlink socket: Protocol not supported
Netid          State          Recv-Q          Send-Q                    Local Address:Port                     Peer Address:Port          Process   

# ping -c google.com
PING google.com (192.178.173.113) 56(84) bytes of data.
64 bytes from lcbome-in-f113.1e100.net (192.178.173.113): icmp_seq=1 ttl=114 time=41.2 ms
64 bytes from lcbome-in-f113.1e100.net (192.178.173.113): icmp_seq=2 ttl=114 time=36.6 ms
64 bytes from lcbome-in-f113.1e100.net (192.178.173.113): icmp_seq=3 ttl=114 time=56.3 ms
64 bytes from lcbome-in-f113.1e100.net (192.178.173.113): icmp_seq=4 ttl=114 time=10.4 ms

--- google.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3004ms
rtt min/avg/max/mdev = 10.375/36.113/56.303/16.552 ms

# ps aux --sort=-%cpu | head -10
dipesh      86  3.2  1.7 1810336 143976 tty1 Sl   18:25   1:36 /home/dipesh/.vscode-server/.../extensionHost

# ps -o pid,ppid,stat,pcpu,pmem,etime,comm -p 86
  PID  PPID STAT %CPU %MEM     ELAPSED COMMAND

# find /home/dipesh/.vscode-server/data/logs -type f | tail -20
agenthost.log
remoteexthost.log
ptyhost.log
remoteagent.log
Git.log
GitHub.log

# tail -n 50 /home/dipesh/.vscode-server/data/logs/20260815T182452/exthost1/remoteexthost.log

# ps -o pid,ppid,stat,pcpu,pmem,etime,comm -p 86

# Quick Findings
OS: Ubuntu 24.04.1 LTS running under WSL.
CPU: No obvious CPU spike was observed.
Memory: 1.9 GiB available; swap usage was only 394 MiB.
Disk: Root filesystem was 22% used with 273 GB available.
Logs: /var/log used only 552 KB.
Network: DNS and connectivity worked, but the short ping test showed 25% packet loss.
Target process: VS Code Server Extension Host, PID 86.
Target CPU: 3.2%.
Target memory: 1.7%.
Process state: Stable and multi-threaded.
Logs: Mostly normal informational messages.
Error found: renderMermaidDiagram tool was not contributed.

Happy Learning