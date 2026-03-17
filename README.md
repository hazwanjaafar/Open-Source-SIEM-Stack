# Open-Source-SIEM-Stack
SOC Home Lab using open-source tools like Wazuh (SIEM), Graylog (log management), and OpenCTI (threat intelligence). Gained hands-on experience in implementing detection rules, monitoring security events, and responding to incidents in a controlled environment, simulating real-world scenarios for cybersecurity operations

# SOC Home Lab

A Security Operations Center (SOC) Home Lab built with open-source tools for hands-on cybersecurity experience. This lab simulates real-world scenarios for threat detection, log management, threat intelligence, and incident response.

---

## Overview

This home lab integrates the following open-source tools to replicate a production-grade SOC environment:

| Tool | Role |
|---|---|
| [Wazuh](https://wazuh.com/) | SIEM & XDR |
| [Graylog](https://www.graylog.org/) | Log Management |
| [Grafana](https://grafana.com/) | Dashboards & Visualization |
| [OpenCTI](https://github.com/OpenCTI-Platform/opencti) | Threat Intelligence Platform |
| [MISP](https://github.com/MISP/MISP) | Malware Information Sharing Platform |
| [IRIS](https://github.com/dfir-iris/iris-web) | Incident Response Platform |
| [Velociraptor](https://github.com/Velocidex/velociraptor) | Digital Forensics & Endpoint Visibility |
| [Shuffle](https://shuffler.io/) | Security Orchestration, Automation & Response (SOAR) |
| [InfluxDB](https://github.com/influxdata/influxdb) | Time-Series Metrics Database |
| [pfSense](https://www.pfsense.org/) | Firewall & Network Security |
| [Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon) | Windows System Activity Monitoring |

---

## Architecture

```
                          +------------------+
                          |    pfSense       |
                          |  (Firewall/IDS)  |
                          +--------+---------+
                                   |
               +-------------------+-------------------+
               |                                       |
    +----------+----------+               +-----------+-----------+
    |   Windows Endpoints |               |    Linux Endpoints    |
    |   (Sysmon + Wazuh)  |               |    (Wazuh Agent)      |
    +----------+----------+               +-----------+-----------+
               |                                       |
               +-------------------+-------------------+
                                   |
                          +--------+---------+
                          |   Wazuh Manager  |
                          |  (SIEM / XDR)    |
                          +--------+---------+
                                   |
               +-------------------+-------------------+
               |                   |                   |
    +----------+-----+   +---------+------+   +--------+-------+
    |   Graylog      |   |   InfluxDB     |   |   OpenCTI      |
    | (Log Manager)  |   | (Metrics DB)   |   | (Threat Intel) |
    +----------+-----+   +---------+------+   +--------+-------+
               |                   |                   |
               |          +--------+---------+         |
               |          |    Grafana       |         |
               |          | (Visualization)  |         |
               |          +------------------+         |
               |                                       |
    +----------+-----+                       +---------+------+
    |    Shuffle     |                       |    MISP        |
    |   (SOAR)       |                       | (Intel Sharing)|
    +----------+-----+                       +---------+------+
               |
    +----------+-----+
    |    IRIS        |
    | (IR Platform)  |
    +----------------+
```

---

## Tools

### Wazuh — SIEM & XDR
[https://wazuh.com/](https://wazuh.com/)

Wazuh serves as the central SIEM (Security Information and Event Management) and XDR (Extended Detection and Response) platform. It collects, aggregates, and analyzes security events from all endpoints.

**Key capabilities used:**
- Agent-based log collection from Windows and Linux hosts
- File integrity monitoring (FIM)
- Vulnerability detection
- Custom detection rules for threat identification
- Active response to automatically block malicious activity
- Integration with OpenCTI and MISP for threat intelligence enrichment

---

### Graylog — Log Management
[https://www.graylog.org/](https://www.graylog.org/)

Graylog is used for centralized log management, parsing, and search across all log sources in the lab.

**Key capabilities used:**
- Ingestion of syslog, Windows Event Logs, and application logs
- Stream-based log routing and filtering
- Alerting on log patterns and anomalies
- Dashboard creation for log visibility
- Integration with Wazuh for forwarding alerts

---

### Grafana — Dashboards & Visualization
[https://grafana.com/](https://grafana.com/)

Grafana provides real-time dashboards for visualizing metrics and security events collected by Wazuh and InfluxDB.

**Key capabilities used:**
- Security event dashboards sourced from Wazuh and InfluxDB
- Alert panels for high-severity incidents
- Network traffic and system performance visualization
- Integration with InfluxDB as a data source

---

### OpenCTI — Threat Intelligence Platform
[https://github.com/OpenCTI-Platform/opencti](https://github.com/OpenCTI-Platform/opencti)

OpenCTI is used to store, organize, and operationalize threat intelligence from multiple sources.

**Key capabilities used:**
- Ingestion of STIX/TAXII threat intelligence feeds
- Mapping indicators of compromise (IoCs) to MITRE ATT&CK techniques
- Integration with MISP for intelligence sharing
- Enrichment of Wazuh alerts with threat context

---

### MISP — Malware Information Sharing Platform
[https://github.com/MISP/MISP](https://github.com/MISP/MISP)

MISP is used for storing and sharing structured threat intelligence, particularly IoCs such as IP addresses, file hashes, and domains.

**Key capabilities used:**
- Creating and managing threat events with IoCs
- Sharing intelligence with OpenCTI
- Exporting IoC feeds for Wazuh detection rules
- Correlation of IoCs across multiple events

---

### IRIS — Incident Response Platform
[https://github.com/dfir-iris/iris-web](https://github.com/dfir-iris/iris-web)

IRIS is used to manage and track security incidents throughout their lifecycle.

**Key capabilities used:**
- Case management for security incidents
- Evidence tracking and timeline construction
- Task assignment and collaboration during incident response
- Integration with Shuffle for automated case creation from Wazuh alerts

---

### Velociraptor — Digital Forensics & Endpoint Visibility
[https://github.com/Velocidex/velociraptor](https://github.com/Velocidex/velociraptor)

Velociraptor provides deep endpoint visibility for digital forensics and threat hunting across monitored hosts.

**Key capabilities used:**
- Artifact collection from Windows and Linux endpoints
- Live forensic triage during incident response
- Hunt queries for IoC sweeping across all endpoints
- Process, network connection, and file system analysis

---

### Shuffle — SOAR
[https://shuffler.io/](https://shuffler.io/)

Shuffle is the SOAR (Security Orchestration, Automation and Response) platform that automates repetitive SOC workflows.

**Key capabilities used:**
- Automated alert triage triggered by Wazuh detections
- IoC enrichment workflows using MISP and OpenCTI
- Automatic case creation in IRIS for high-severity alerts
- Email and messaging notifications for security events
- Playbooks for common incident response procedures

---

### InfluxDB — Time-Series Metrics Database
[https://github.com/influxdata/influxdb](https://github.com/influxdata/influxdb)

InfluxDB stores time-series metrics from network devices, endpoints, and security tools for visualization in Grafana.

**Key capabilities used:**
- Storage of system performance metrics (CPU, memory, network)
- Retention policies for short- and long-term metric storage
- Grafana data source for real-time dashboards

---

### pfSense — Firewall & Network Security
[https://www.pfsense.org/](https://www.pfsense.org/)

pfSense acts as the network gateway and firewall, controlling traffic between lab network segments and providing network-level visibility.

**Key capabilities used:**
- Network segmentation between lab zones
- Firewall rules to simulate real-world perimeter controls
- Suricata IDS/IPS integration for network threat detection
- Log forwarding to Graylog for centralized analysis

---

### Sysmon — Windows System Activity Monitoring
[https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

Sysmon (System Monitor) is deployed on all Windows endpoints to provide detailed telemetry about process creation, network connections, file changes, and more.

**Sysmon configuration used:** [SwiftOnSecurity/sysmon-config](https://github.com/SwiftOnSecurity/sysmon-config)

**Key capabilities used:**
- Process creation and command-line logging
- Network connection tracking with process attribution
- File creation time change detection
- Driver and image load monitoring
- Forwarding events to Wazuh agent for SIEM ingestion

---

## Skills & Experience Gained

- **Detection Engineering:** Writing and tuning Wazuh detection rules mapped to MITRE ATT&CK techniques
- **Log Management:** Centralizing and normalizing logs from diverse sources using Graylog
- **Threat Intelligence:** Operationalizing threat feeds via OpenCTI and MISP to enrich detections
- **Incident Response:** Managing the full incident lifecycle in IRIS, from detection to closure
- **Digital Forensics:** Conducting endpoint forensic triage and threat hunting with Velociraptor
- **Security Automation:** Building SOAR playbooks in Shuffle to reduce manual analyst workload
- **Network Security:** Configuring pfSense for segmentation, firewalling, and IDS/IPS
- **Monitoring & Visualization:** Building security dashboards in Grafana backed by InfluxDB
- **Windows Internals:** Capturing deep endpoint telemetry on Windows hosts with Sysmon

---

## References

- Wazuh Documentation: https://documentation.wazuh.com/
- Graylog Documentation: https://go2docs.graylog.org/
- Grafana Documentation: https://grafana.com/docs/
- OpenCTI Documentation: https://docs.opencti.io/
- MISP Documentation: https://www.misp-project.org/documentation/
- IRIS Documentation: https://docs.dfir-iris.org/
- Velociraptor Documentation: https://docs.velociraptor.app/
- Shuffle Documentation: https://shuffler.io/docs
- InfluxDB Documentation: https://docs.influxdata.com/
- pfSense Documentation: https://docs.netgate.com/pfsense/en/latest/
- Sysmon Documentation: https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon
- SwiftOnSecurity Sysmon Config: https://github.com/SwiftOnSecurity/sysmon-config
