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


<h2>Program walk-through:</h2>

<p align="center">
Launch the utility: <br/>

  
<br />
<br />
Create the components and network for my VM:  <br/>


<br />
<br />
Connecting my VM to Log Analytics Workspace and Querying my log repository with KQL: <br/>

<br />
<br />
Uploading the Geolocation data to the SIEM:  <br/>

<br />
<br />
Creating Attack Map:  <br/>



