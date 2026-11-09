 # Networkwalks-week-2-projectPENETRATION TESTING REPORT
FOOTPRINTING & NETWORK SCANNING PHASES
W2-PM-FINAL | CYBERSECURITY | NETWORKWALK
Pentester Name: Uchechukwu Favour Chinenye
Program/Batch: B082-Networkwalks
Date: 9 November 2026
Modules Completed: W2-PM1 (Multiple Kali Tools) & W2-PM5 (Zenmap Scanning)
Client/Target: 1. networkwalks.com (secured written permission) | 2. My own local LAN Network
Permission Secured from Client?: Yes
Phases Covered: Phase 1: Reconnaissance & Footprinting | Phase 2: Scanning & Network Discovery | Phase 3-5: In Progress
1. Liability Disclaimer
I have performed these activities only on the systems & devices where I had secured written permission or the devices/systems that I own myself. All these materials are for education and research purpose only. Do not use anything from here to break the law. The instructor, the authors and Networkwalks are not responsible for what you do with this knowledge. Every action you take is your own responsibility. Misuse can lead to criminal charges, heavy fines, loss of your job and a permanent record. In most countries unauthorised access is a crime even when nothing is damaged.
2. Introduction
This report covers footprinting the networkwalks.com domain using multiple Kali Linux tools (W2-PM1) and scanning my own local network with Zenmap (W2-PM5). One module covers the footprinting phase and the other covers the scanning phase, so together they show how an attacker moves from gathering public information to mapping live hosts on a network. It is the Week 2 part of my ongoing internship program at Networkwalks.

All commands were run in Kali Linux (footprinting) and on a Windows PC with Zenmap installed (scanning). Every step below includes the exact command used, the result I observed, a screenshot as evidence, and a short note on why the finding matters from an attacker's point of view.
3. Tools Used
Tool
Purpose
Kali Linux & Windows
Operating systems used for performing footprinting and network scanning.
WHOIS
Query domain registration details, registrar, and name servers.
whatweb
Fingerprint web technologies, CMS versions, and web server software.
nslookup
Resolve the target domain name (networkwalks.com) to its public IP address.
curl -I
Inspect HTTP response headers and uncover backend endpoints.
wafw00f
Detect whether a Web Application Firewall (WAF) is protecting the target.
dnsrecon
Enumerate DNS records, including TXT (SPF) and SRV autodiscover records.
Zenmap (Nmap GUI)
Scan the local subnet (10.138.104.0/24) to discover active hosts and network topology.
Windows CMD
Identify local IP configuration (ipconfig /all) and MAC address.

4. Activities Performed
4.1 Footprinting & Reconnaissance
I performed reconnaissance against the networkwalks.com domain using six Kali Linux tools: WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, and DNSRecon. Each tool was used to collect a different type of information about the target.

WHOIS: Used WHOIS to obtain domain registration details. The results revealed that the domain is registered with GoDaddy (whois.godaddy.com) and uses HostGator name servers (NS6135.HOSTGATOR.COM and NS6136.HOSTGATOR.COM).
WhatWeb: Used WhatWeb to fingerprint the technologies running on the target web server. The tool identified that the site runs on Apache web server with WordPress 7.1.4, Bootstrap 7.1.4, jQuery 3.7.1, and WP Download Manager 3.3.76.
Nslookup: Used Nslookup to resolve the target domain name to its public IP address. The query resolved networkwalks.com to 192.232.216.135.
Curl: Used Curl with the -I option to inspect HTTP response headers. The output confirmed the server returned HTTP/2 200, exposed the WordPress REST API endpoint (/wp-json/), and indicated an Apache web server.
Wafw00f: Used Wafw00f to determine whether a Web Application Firewall (WAF) was protecting the site. The scan confirmed that networkwalks.com is behind ModSecurity (SpiderLabs) WAF.
DNSRecon: Used DNSRecon (dnsrecon -d networkwalks.com) to enumerate DNS records. The scan successfully identified TXT records (SPF verification) and SRV autodiscover records for email services.
4.2 Network Scanning with Zenmap
For the second activity, I used Zenmap (Nmap GUI) to perform network discovery on my local Wi-Fi network.

I first opened the Windows Command Prompt and executed ipconfig to determine my local network configuration. My laptop was assigned the IPv4 address 10.138.104.48 with a subnet mask of 255.255.255.0. Based on this, I targeted the subnet 10.138.104.0/24 in Zenmap using a Ping scan profile.

The scan completed in 7.72 seconds and identified 2 live hosts on the network:

10.138.104.48 — My local Windows host
10.138.104.161 — Default Gateway / Network Router

The MAC address discovered during the scan was:

DE:D0:A3:D3:AD:B5 (Associated with router IP 10.138.104.161)

After completing the scan, I navigated to the Topology tab in Zenmap, enabled the legend, and saved the network topology graph to my Desktop.
5. Risk Analysis / Impact
#
Risk / Finding
Evidence / Observation
Impact / Potential Risk
Risk Level
1
Web technology information exposed
WhatWeb identified WordPress 7.1.4, Bootstrap, and WP Download Manager
Attackers can target specific known vulnerabilities for these exact software versions.
 Medium
2
Target server IP address identifiable
Nslookup resolved networkwalks.com to 192.232.216.135
Exposes the host server directly for network-level probing or targeted attacks.
 Low
3
HTTP header details exposed
Curl returned HTTP response headers and exposed /wp-json/ REST API
Reveals backend structures and active API endpoints to potential attackers.
 Low
4
WAF technology identifiable
Wafw00f identified ModSecurity (SpiderLabs) WAF
Helps attackers choose specific WAF-bypass techniques or tailor exploit payloads.
 Medium
5
DNS infrastructure exposed
DNSRecon enumerated TXT, SPF, and mail autodiscover records
Gives attackers insight into email servers and external service dependencies.
 Low
6
Live hosts visible on local subnet
Zenmap discovered 2 live IP addresses (10.138.104.48 & 10.138.104.161)
An internal attacker could map active targets on the local LAN for lateral movement.
 Medium

6. Recommendations
Based on the observations from these activities, I recommend the following security improvements:

Review Publicly Exposed Technology Information: Organizations should regularly audit exposed headers and web signatures to prevent revealing specific versions of CMS software and plugins like WordPress 7.1.4.
Keep Software and Plugins Updated: CMS platforms, theme components, and plugins (such as WP Download Manager) should be updated promptly to minimize exposure to known exploits.
Minimize HTTP Header Leakage: Configure web servers (Apache) to suppress granular version tokens and disable unnecessary REST API endpoints (/wp-json/) if they are not actively required by public clients.
Maintain and Audit WAF Rules: Ensure ModSecurity WAF rules are regularly updated and tuned to actively block reconnaissance scanning and bypass techniques.
Audit Public DNS and TXT Records: Periodically review DNS zones to remove obsolete SRV autodiscover records and restrict unnecessary internal routing metadata.
Perform Regular Internal Discovery Scans: Network administrators should regularly run host discovery scans (like Zenmap/Nmap) across local subnets (10.138.104.0/24) to identify rogue or unauthorized devices.
Maintain Up-to-Date Asset Inventories: Keep accurate records of all active IP assignments and MAC addresses (DE:D0:A3:D3:AD:B5) across gateway routers and endpoints.
7. Conclusion
During Week 2 of my Cybersecurity & Ethical Hacking internship at Networkwalks, I completed practical activities covering passive footprinting and active network scanning.

In the footprinting activity (W2-PM1), I used six Kali Linux tools (whois, whatweb, nslookup, curl, wafw00f, and dnsrecon) to collect public reconnaissance data for networkwalks.com. I learned how domain registration, DNS records, web server headers, and firewall presence can be enumerated without directly attacking a system.

In the network scanning activity (W2-PM5), I used Zenmap to discover active hosts within my local subnet (10.138.104.0/24), identifying live IPs, physical MAC addresses, and generating a visual network topology graph.

These exercises demonstrated that information gathering is an essential preliminary phase in penetration testing. Even without exploitation, thorough reconnaissance reveals valuable structural details that aid security professionals in defensive hardening.
8. Evidences Collected

