# Internship-week-2
Week 2 Cybersecurity Internship -- Footprinting & Network Scanning
Student: Pradhik Nair
Batch: B084
Organization: Networkwalks
Operating System: Kali Linux
Modules: W2-PM4, W2-PM5 and W2-PM-FINAL
Overview
This repository documents the Week 2 cybersecurity internship practical
work. It covers:
- W2-PM4 -- Footprinting with theHarvester: Passive OSINT
  collection for networkwalks.com using CertSpotter.
- W2-PM5 -- Network Scanning with Zenmap/Nmap: Host discovery on
  the Kali virtual machine's own VirtualBox NAT subnet.
- W2-PM-FINAL -- Detailed Report: Documentation of the methods,
  observations, results and limitations.
The two practical activities used separate targets and environments. The
Zenmap scan was performed on the local VirtualBox NAT subnet; it was
not a scan of Networkwalks' production network.
Objectives
- Understand footprinting and Open-Source Intelligence (OSINT).
- Use theHarvester to query a public Certificate Transparency data
  source.
- Generate and validate JSON and XML output.
- Understand virtual network configuration and host discovery.
- Use Zenmap to view scan output, discovered hosts and network
  topology.
- Document findings accurately and responsibly.
Tools and Technologies
  Tool / Component      Purpose
  Kali Linux            Operating system for the practical work
theHarvester 4.11.1   Passive OSINT and domain footprinting
CertSpotter           Public Certificate Transparency data source
Zenmap                Graphical interface for Nmap
Nmap 7.99             Network host discovery
Oracle VirtualBox     Virtual machine environment using NAT
Python 3              Inspecting JSON and XML output
W2-PM4 -- theHarvester-Based Footprinting
Objective
Perform passive footprinting for networkwalks.com using theHarvester
and CertSpotter, then generate machine-readable reports.
Command
theHarvester -d networkwalks.com -b certspotter -l 50 -f W2PM4_Networkwalks
Command explanation
- -d networkwalks.com -- specifies the target domain.
- -b certspotter -- selects CertSpotter as the data source.
- -l 50 -- sets the result limit.
- -f W2PM4_Networkwalks -- sets the output filename prefix.
Results
The CertSpotter query reported:

  Data              Result
  IP addresses      None found
Email addresses   None found
People            None found
Hostnames         None found
JSON output       Generated
XML output        Generated

The JSON output was inspected with:
python3 -m json.tool ~/W2PM4_Networkwalks.json
The XML output was also parsed successfully. Its root element was
theHarvester, and its command metadata matched the command that was
executed.
Interpretation
No results were returned by CertSpotter for this particular query. This
does not prove that the domain has no publicly available
information. The result is limited to the selected source, query and
time of execution.
Generated evidence
- W2PM4_Networkwalks.json
- W2PM4_Networkwalks.xml
W2-PM5 -- Zenmap-Based Network Scanning
Objective
Discover active hosts in the student's own VirtualBox NAT subnet using
Zenmap/Nmap, inspect the topology and save the scan output.
Network configuration
The Kali virtual machine's network adapter was configured to use
VirtualBox NAT. The following configuration was observed:

  Property                  Observed value
  Network interface         eth0
Kali IPv4 address         10.0.2.15/24
Default gateway           10.0.2.2
Subnet                    10.0.2.0/24
VirtualBox network mode   NAT

Connectivity was checked using ping tests to 8.8.8.8 and google.com.
Both four-packet tests received replies with 0% packet loss.
Ping Scan
Zenmap was configured with:
- Target: 10.0.2.0/24
- Profile: Ping scan
The generated Nmap command was:
nmap -sn 10.0.2.0/24
The -sn option performs host discovery without a port scan.
Results
  Item                         Observed result
  Target subnet                10.0.2.0/24
Total IP addresses scanned   256
Live hosts discovered        3
Hosts reported down          253
Scan duration                Approximately 3.02 seconds
Nmap version                 7.99

The three discovered addresses were:
  IP address                          Notes
  10.0.2.2                          VirtualBox NAT gateway, based on
the observed network configuration

  10.0.2.3                          VirtualBox NAT DNS service, based
on the observed NAT environment
10.0.2.15                         Kali Linux virtual machine
Zenmap's Topology tab displayed the three discovered hosts. The
Host Details tab for 10.0.2.15 showed the host as up.

A Ping Scan does not perform port scanning. Therefore, the host details
showed zero scanned ports; this should not be interpreted as a port scan
finding.
Supplementary localhost scan
An earlier scan was performed against 127.0.0.1. Its saved XML output
showed:
- Host status: up
- 1,000 TCP ports scanned
- 1,000 ports closed
- No open or filtered ports
This was a separate localhost scan, not the subnet Ping Scan.
Generated evidence
- W2PM5_Report.xml -- localhost scan output
- W2PM5_PingScan.xml -- subnet Ping Scan output
- W2PM5_Topology.png -- saved topology diagram
Key Findings
1. TheHarvester successfully ran against networkwalks.com using
   CertSpotter and generated JSON and XML reports.
2. CertSpotter returned no IP addresses, email addresses, people or
   hostnames in this execution.
3. The VirtualBox NAT subnet scan discovered three live hosts out of
   256 addresses.
4. Zenmap's topology view helped visualize the discovered hosts.
5. JSON and XML outputs were generated and checked.
Limitations
- A single OSINT source returning no results does not establish that
  no public information exists.
- The Zenmap scan covered the local VirtualBox NAT subnet only. It did
  not scan Networkwalks' production network or the physical LAN.
- A Ping Scan identifies responsive hosts but does not identify open
  services or vulnerabilities.
- Results represent the exact commands and environment at the time of
  execution.
Responsible Use and Authorization
The passive footprinting activity for networkwalks.com was performed
based on authorization communicated by the student's mentor. Network
scanning was restricted to the student's own VirtualBox NAT environment
(10.0.2.0/24).

No exploitation, password attacks, denial-of-service testing,
unauthorized access or data modification were performed. Any further
testing should be carried out only with explicit authorization and a
clearly defined scope.
Repository Evidence
Add your actual screenshots and report files to this repository if you
intend to publish them. Suggested structure:
.
├── README.md
├── reports/
│   ├── W2PM4_Networkwalks.json
│   ├── W2PM4_Networkwalks.xml
│   ├── W2PM5_Report.xml
│   └── W2PM5_PingScan.xml
└── screenshots/
    ├── PM4-theHarvester-output.png
    ├── PM4-JSON-validation.png
    ├── PM4-XML-validation.png
    ├── PM5-Zenmap-ping-scan.png
    ├── PM5-topology.png
    └── PM5-host-details.png
Only include files and screenshots that you actually have. Do not add
fabricated evidence.
Conclusion
This practical work provided hands-on experience with passive OSINT,
domain footprinting, JSON/XML report validation, virtual networking and
network host discovery. theHarvester was used to query CertSpotter for
networkwalks.com, while Zenmap was used to discover hosts in the local
VirtualBox NAT subnet. The results and limitations were recorded to
maintain an accurate and responsible technical report.
Author
Pradhik Nair
Batch: B084
