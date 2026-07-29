<h1>Home-Network Reconnaissance Lab</h1>


<h2>Description</h2>
A hands-on network reconnaissance exercise where I mapped and fingerprinted every device on my own home network using Nmap, then identified each host — not by the label it advertised, but by its behavior. Two of the devices were actively hiding their identity through MAC randomization; I identified them anyway using their open-port signatures. The goal wasn't just "run a scan." It was to build a complete, reasoned picture of what's on the network and prove each conclusion from evidence, the same process you'd use to inventory assets on a network you're defending.

<h2>Authorization and Scope: </h2>
Every device scanned here is on my own home network, which I own and control. No external hosts, no neighboring networks, nothing outside my authority was touched. This matters: unauthorized scanning is both unethical and illegal. Staying inside my own perimeter is the whole point.

<ul>
  <li>Target network: 10.0.0.0/24 (my LAN)</li>
  <li>Scanning host: my own Windows workstation</li>
  <li>Tool: Nmap 7.98</li>
</ul>


# Program walk-through


  
### 1. Identify the network range
Ran ipconfig on Windows to find the scan target range. Three values matter here:
<b></b>

- IPv4 Address — my machine's address on the network, confirming the 10.0.0.x range
- Subnet Mask — 255.255.255.0, telling me the network size (a /24, 256 addresses)
- Default Gateway — confirms which device is the router

This step matters because without it you're guessing at the range, wasting time scanning nothing, or worse, scanning a network that isn't yours. IPv4 address + subnet mask combine into the CIDR notation 10.0.0.0/24, which is the input Nmap needs.

<br>
<br>

### 2. Host Discovery 
nmap -sn 10.0.0.0/24

| IP | Vendor |
| --- | --- |
| 10.0.0.1 | CommScope |
| 10.0.0.12 | Unknown (Randomized MAC) |
| 10.0.0.10 | Apple |
| 10.0.0.47 | --- |

Four hosts up. 10.0.0.1 is my ISP gateway (CommScope hardware matches Xfinity). 10.0.0.47 is my workstation. The other two needed a deeper scan to confirm.

<br>
<br>

### 3. Service/Version Detection 
nmap -sn 10.0.0.0/24
|    IP     |    Device    |    Ports   | Identification |
| --- | --- | --- | --- |
| 10.0.0.47 | Windows Workstation | 135, 139, 445/tcp, 16992/tcp | Ports 135, 139, 445 are the classic Windows networking trio (RPC, NetBIOS, SMB) |
| 10.0.0.1  | Gateway/Router | 22, 23, 53, 80/tcp | Port 80 banner identifies it as Xfinity Broadband Router Service |
| 10.0.0.12 | Unidentified | 49152, 62078/tcp | Port 62078 (lockdownd) suggests an Apple device, but role unclear |
| 10.0.0.109 | Unidentified | 5000, 7000, 7100, 49152, 49153, 62078/tcp | AirTunes/RTSP signature suggests an Apple streaming device, but role unclear |

Two devices are unconfirmed at this moment and I need to dig further

<br>
<br>

### 4. mDNS/Bonjour service discovery
nmap -sU -p5353 --script=dns-service-discovery 10.0.0.109

- -sU = UDP scan (mDNS runs on UDP, not TCP)
- p5353 = the fixed, reserved port for mDNS
- -script=dns-service-discovery = NSE script that queries the device for self-announced service metadata, rather than just checking if the port is open

This confirmed 10.0.0.109 is an Apple TV (model=AppleTV6,2), identified directly from the device's own service announcement rather than inference from open ports alone.
<br>
<br>

### 5. Summary 

| IP | Device |
| --- | --- |
| 10.0.0.1 | Gateway/Router (XFinity) |
| 10.0.0.12 | Iphone |
| 10.0.0.47 | Windows Workstation |
|10.0.0.109 | Apple TV |











