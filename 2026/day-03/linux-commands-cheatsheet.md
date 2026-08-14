# Linux Commands Practice
- Process management
ps	    Show running processes
ps aux	Show all running processes with details
top	    Real-time process monitoring
htop	Interactive process monitoring
pgrep <name>	Find a process by name
kill <PID>	    Terminate a process
kill -9 <PID>	Forcefully terminate a process
pkill <name>	Kill processes by name

# File System
Command	      Purpose
pwd	         Show current directory
ls	         List files
ls -la	     List all files with details
cd <dir>	 Change directory
mkdir <dir>	 Create directory
touch <file>	Create empty file
cp <src> <dest>	Copy files
mv <src> <dest>	Move/rename files
rm <file>	   Remove file
rm -r <dir>	   Remove directory
cat <file>	  Display file contents
less <file>	  View file page by page
find	      Search for files
du -sh <dir>  Check directory size
df -h	       Check filesystem disk usage
chmod	      Change permissions
chown	      Change ownership

# Networking Troubleshooting
Command	        Purpose
ping <host>	    Test connectivity
ip addr	        Show IP addresses
ip route	    Show routing table
ss -tulpn	    Show listening ports/connections
curl <URL>	    Test HTTP/HTTPS connectivity
nslookup <domain>	Check DNS resolution
dig <domain>	 Detailed DNS troubleshooting
traceroute <host>	Trace network path
hostname -I	      Show system IP address

Happy Learning