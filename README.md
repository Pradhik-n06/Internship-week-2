# Week 2 Cybersecurity Internship: Footprinting and Network Scanning

This README documents the Week 2 practical exercises completed in Kali Linux. It separates passive domain footprinting from local network host discovery, records the observed results, and states the limits of what those results show.

> **Scope note:** The footprinting exercise queried public Certificate Transparency data for `networkwalks.com`, with authorization communicated by the student's mentor. The Zenmap scan was limited to the student's own VirtualBox NAT subnet (`10.0.2.0/24`). It was not a scan of Networkwalks' production network or the physical LAN.

## Student and Environment

| Item | Details |
|---|---|
| Student | Pradhik Nair |
| Batch | B084 |
| Organization | Networkwalks |
| Operating system | Kali Linux |
| Modules | W2-PM4, W2-PM5, W2-PM-FINAL |

## Objectives

- Understand footprinting and open-source intelligence (OSINT).
- Use theHarvester to query a public Certificate Transparency data source.
- Generate and validate JSON and XML output.
- Review virtual network configuration and perform host discovery within an authorized lab subnet.
- Use Zenmap to inspect scan output, discovered hosts, and topology.
- Document observations and limitations accurately and responsibly.

## Tools

| Tool or component | Version / purpose |
|---|---|
| Kali Linux | Operating system used for the practical work |
| theHarvester | 4.11.1; passive OSINT and domain footprinting |
| CertSpotter | Public Certificate Transparency data source queried by theHarvester |
| Zenmap | Graphical interface for Nmap |
| Nmap | 7.99; network host discovery |
| Oracle VirtualBox | Virtual machine environment; Kali was using NAT mode for the scan |
| Python 3 | Inspecting JSON and XML output |

## W2-PM4 — Passive Footprinting with theHarvester

### Objective and scope

Query CertSpotter for publicly available Certificate Transparency-related information associated with `networkwalks.com`. This was a passive query to the selected source; it was not an active scan of the domain's hosts.

### Command used

```bash
theHarvester -d networkwalks.com -b certspotter -l 50 -f W2PM4_Networkwalks
```

| Option | Meaning |
|---|---|
| `-d networkwalks.com` | Sets the domain queried. |
| `-b certspotter` | Selects CertSpotter as the data source. |
| `-l 50` | Sets the result limit. |
| `-f W2PM4_Networkwalks` | Sets the output filename prefix. |

### Observed results

| Data or output | Result in this execution |
|---|---|
| IP addresses | None reported |
| Email addresses | None reported |
| People | None reported |
| Hostnames | None reported |
| JSON report | Generated |
| XML report | Generated |

The JSON output was checked with:

```bash
python3 -m json.tool ~/W2PM4_Networkwalks.json
```

The XML output was also parsed successfully. Its root element was `theHarvester`, and its command metadata matched the command used.

### Interpretation and limitations

CertSpotter returned no results for the queried data types in this execution. This is limited to the selected source, query options, and time of execution; it does **not** establish that the domain has no publicly available information.

### Report files

The generated report filenames were:

- `W2PM4_Networkwalks.json`
- `W2PM4_Networkwalks.xml`

## W2-PM5 — Local Network Host Discovery with Zenmap

### Objective and scope

Discover responsive hosts on the Kali virtual machine's own VirtualBox NAT subnet and review the resulting scan output and topology. This was a host-discovery scan, not a port or vulnerability scan.

### Observed network configuration

| Property | Observed value |
|---|---|
| Network interface | `eth0` |
| Kali IPv4 address | `10.0.2.15/24` |
| Default gateway | `10.0.2.2` |
| Subnet | `10.0.2.0/24` |
| VirtualBox network mode | NAT |

Connectivity checks to `8.8.8.8` and `google.com` each received replies to all four packets, with 0% packet loss. These checks indicate connectivity at the time of testing; they do not establish anything about other hosts or services on the subnet.

### Ping scan

Zenmap was configured with:

- **Target:** `10.0.2.0/24`
- **Profile:** Ping scan

The generated Nmap command was:

```bash
nmap -sn 10.0.2.0/24
```

The `-sn` option performs host discovery without a port scan.

### Observed scan results

| Item | Observed result |
|---|---|
| Target subnet | `10.0.2.0/24` |
| Addresses in subnet | 256 |
| Live hosts discovered | 3 |
| Hosts reported down | 253 |
| Scan duration | Approximately 3.02 seconds |
| Nmap version | 7.99 |

The three reported addresses were:

| Address | Identification / context |
|---|---|
| `10.0.2.2` | Identified as the VirtualBox NAT gateway based on the observed guest network configuration |
| `10.0.2.3` | Identified as the VirtualBox NAT DNS service based on the observed NAT environment |
| `10.0.2.15` | Kali Linux virtual machine address |

Zenmap's **Topology** tab displayed these three discovered hosts. The **Host Details** tab reported `10.0.2.15` as up.

A Ping Scan does not test TCP or UDP ports. A host-discovery result therefore cannot be used to claim that ports are open, closed, or secure. Any zero-port count shown for this scan is expected because no port scan was requested.

### Separate localhost scan

An earlier scan targeted `127.0.0.1`. Its saved XML output recorded the following:

- Host status: up
- 1,000 TCP ports scanned
- 1,000 ports closed
- No open or filtered ports reported

This was a separate localhost scan and must not be confused with the `10.0.2.0/24` Ping Scan. These results describe only the scan output captured at that time; they are not a general security assessment of the machine.

### Report and image files

The following filenames were recorded for the evidence:

- `W2PM5_Report.xml` — localhost scan output
- `W2PM5_PingScan.xml` — subnet Ping Scan output
- `W2PM5_Topology.png` — saved topology diagram

## Key Findings

1. theHarvester queried CertSpotter for `networkwalks.com` and generated JSON and XML reports.
2. CertSpotter reported no IP addresses, email addresses, people, or hostnames in this execution. This is a source- and time-limited result, not proof that no public information exists.
3. The host-discovery scan reported three responsive addresses on the local VirtualBox NAT subnet out of 256 addresses.
4. The subnet scan used `-sn` and did not scan ports or identify vulnerabilities.
5. The separate localhost scan reported 1,000 closed TCP ports and no open or filtered ports in its saved output.

## Limitations

- The footprinting result came from one selected OSINT data source and one execution.
- The Zenmap scan covered only the local VirtualBox NAT subnet. It did not scan Networkwalks' production network or the physical LAN.
- Host discovery identifies hosts that responded to the selected probes; hosts that did not respond may still exist.
- The Ping Scan did not identify open services, vulnerabilities, or overall host security.
- The localhost scan and subnet Ping Scan were separate activities with different targets and scan behavior.
- Results reflect the commands and environment at the time of execution and may change.

## Authorization and Responsible Use

The passive footprinting activity for `networkwalks.com` was performed based on authorization communicated by the student's mentor. Network scanning was restricted to the student's own VirtualBox NAT environment (`10.0.2.0/24`).

No exploitation, password attacks, denial-of-service testing, unauthorized access, or data modification were performed. Any further testing must have explicit authorization and a clearly defined scope. Do not scan systems outside that scope.

## Repository Evidence

If publishing the reports and screenshots, place the actual files in the repository using a structure such as:

```text
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
```

This is a suggested layout, not confirmation that these files are currently present. Include only reports and screenshots that actually exist, and avoid publishing sensitive information.

## Conclusion

These exercises provided practical experience with passive OSINT, structured report validation, virtual networking, and local host discovery. theHarvester queried CertSpotter for `networkwalks.com`; Zenmap performed a host-discovery scan on the Kali VM's local VirtualBox NAT subnet. The scope and limitations are recorded so the observations are not mistaken for a production-network scan, port scan, or vulnerability assessment.

## Author

**Pradhik Nair**  
Batch: B084  
Organization: Networkwalks
