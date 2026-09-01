# Networking Fundamentals & Hands-on Checks

# OSI vs TCP/IP Models
Both OSI and TCP/IP models explain how data travels from one device to another over a network.

🔹 OSI Model

The OSI model has 7 layers:

OSI Layer	What it does	Examples
7️⃣ Application	Network services used by applications	HTTP, DNS, FTP
6️⃣ Presentation	Data format, encryption, compression	SSL/TLS, JPEG
5️⃣ Session	Manages communication sessions	Session management
4️⃣ Transport	Reliable delivery and ports	TCP, UDP
3️⃣ Network	Routing and IP addressing	IP, Routers
2️⃣ Data Link	Frames, MAC addresses	Ethernet, Switches
1️⃣ Physical	Sends raw bits/signals	Cables, Wi-Fi

🔹 TCP/IP Model

The TCP/IP model is more practical and is used for the Internet.

TCP/IP Layer	Matches OSI	 What it does
Application	    7, 6, 5	         Application-level network communication
Transport	     4	             End-to-end data delivery
Internet	     3	             IP addressing and routing
Network Access	 2, 1	     Frames, MAC, and physical transmission

🔄 OSI vs TCP/IP Mapping
        OSI Model                 TCP/IP Model
    ─────────────────          ─────────────────
    7. Application       ┐
    6. Presentation      ├───→  Application
    5. Session           ┘

    4. Transport         ────→  Transport

    3. Network           ────→  Internet

    2. Data Link         ┐
    1. Physical          ┘───→  Network Access
💡
OSI = A 7-layer reference model that helps us understand networking step-by-step.
TCP/IP = A 4-layer practical model that represents how networking actually works on the Internet.
OSI separates Application, Presentation, and Session into three layers.
TCP/IP combines those three into Application.
TCP/IP also combines Data Link and Physical into Network Access.
🚀 DevOps Tip
When troubleshooting a network, think in layers:

Application → Is HTTP/DNS working?
Transport   → Is TCP/UDP and the port working?
Internet    → Is the IP/routing working?
Network     → Is the network interface/link working?

# TCP/IP Stack
Application
  ├── HTTP / HTTPS
  └── DNS

Transport
  ├── TCP
  └── UDP

Internet
  └── IP

Link
  ├── Ethernet
  └── Wi-Fi

# Real Example: curl https://example.com

When we run:

curl https://example.com

url https://example.com = Application (HTTPS) → TCP → IP → Link
🧠 In simple words
curl
 ↓
HTTPS → "I want this webpage"
 ↓
TCP → "I'll reliably transport the data"
 ↓
IP → "I'll route it to the destination"
 ↓
Link → "I'll send it over the network"

1. 🆔 Identity — Find IP Address
 Command
hostname -I
Output :
192.168.112.1 192.168.1.17
Observation-
The command displays the IP addresses assigned to my WSL environment.
My system has two IP addresses: 192.168.112.1 and 192.168.1.17.

2. 📡 Reachability — Test Connectivity
Command
ping -c 4 google.com
Output
4 packets transmitted, 4 received, 0% packet loss
rtt min/avg/max/mdev = 49.091/50.105/51.204/0.776 ms
📝 Observation

Google was reachable successfully with 0% packet loss.
The average latency was approximately 50.1 ms.

3. 🛣️ Path — Trace Network Route
Command
traceroute google.com
Output
traceroute to google.com (142.250.183.142), 30 hops max, 60 byte packets
1  * * *
2  * * *
3  * * *
...
30 * * *
📝 Observation

All traceroute hops returned * * *, meaning the intermediate routers did not respond to the traceroute probes.
This can happen in WSL or when routers/firewalls block traceroute packets.

4. 🔌 Ports — Check Listening Services
Command
ss -tulpn
Output
Cannot open netlink socket: Protocol not supported
Cannot open netlink socket: Protocol not supported

Alternative Command
netstat -tulpn
Output
Active Internet connections (only servers)

Proto Recv-Q Send-Q Local Address
Foreign Address State PID/Program name
📝 Observation

ss could not access the required netlink socket in my WSL environment.
netstat worked, but no listening services were shown.

5. 🔎 Name Resolution — DNS
Command
dig google.com
Output
status: NOERROR

ANSWER SECTION:
google.com. 144 IN A 142.250.183.142

Query time: 64 msec
SERVER: 192.168.1.1#53
📝 Observation

DNS resolution was successful with status NOERROR.
google.com resolved to 142.250.183.142 using DNS server 192.168.1.1.

6. 🌍 HTTP Check
Correct Command
curl -I https://google.com
Output
HTTP/2 301
location: https://www.google.com/
server: gws
📝 Observation

Google returned HTTP 301, which means the request was redirected.
The response redirected https://google.com to https://www.google.com/.

7. 🔗 Connections Snapshot
Command
netstat -an | head
Output
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address Foreign Address State

Active UNIX domain sockets (servers and established)
Proto RefCnt Flags Type State I-Node Path
📝 Observation

The command provides a snapshot of network and UNIX socket connections.
No ESTABLISHED or LISTEN TCP connections were visible in the displayed output.

# Commands Practiced
Command	       Purpose
hostname -I	   Find IP address
ping	         Test network reachability
traceroute	   Trace network path
ss	           View sockets and listening ports
netstat	       View network connections and ports
dig	           Perform DNS lookup
curl -I	       Check HTTP response headers

## Mini Task: Port Probe & Interpret
 1. Identify Listening Port

### Command
sudo netstat -tulpn
Observation

Initially, no listening ports were displayed in my WSL environment, so I started a local Python HTTP server for testing.
2. Start a Local Web Service
Command
python3 -m http.server 8000
Output
Serving HTTP on 0.0.0.0 port 8000
Observation

A Python HTTP server was successfully started and is listening on port 8000.

3. Verify the Service
Command
ps aux | grep http.server
Output
dipesh  4506  0.2  0.1  34616  14260 pts/0  S  08:18  0:00 python3 -m http.server 8000
Observation

The process is running with PID 4506, confirming that the Python HTTP server is active.

4. Test the Port
Command
nc -zv 127.0.0.1 8000
Output
Connection to 127.0.0.1 8000 port [tcp/*] succeeded!
Observation

Port 8000 is reachable locally, confirming that the service is listening and accepting TCP connections

5. Test HTTP Response
Command
curl -I http://127.0.0.1:8000
Output
HTTP/1.0 200 OK
Server: SimpleHTTP/0.6 Python/3.12.3
Content-type: text/html; charset=utf-8
Content-Length: 1044
Observation

The server returned HTTP 200 OK, confirming that the local web service is working correctly.

✅ Final Interpretation

Port 8000 is reachable from the same machine.

The Python HTTP server is running successfully, listening on port 8000, and responding with HTTP 200 OK

🔍 Troubleshooting
If the port had returned Connection refused, the next checks would be:

Check whether the service is running
Verify the correct port
Check listening sockets
Check firewall rules

Happy Learning
