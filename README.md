# Cybersecurity Lab

This project documents SOC-style alert investigations performed in a self-built home lab. The lab is designed to simulate a lightweight detection and analysis workflow using Suricata, Splunk, and Wireshark.

## Lab Architecture

### Core Components

- **Beelink mini PC**
  - Hosts **Splunk** in **Docker**
  - Acts as the central log aggregation and alerting system

- **Raspberry Pi**
  - Runs **Suricata** for network intrusion detection
  - Forwards Suricata logs to Splunk
  - Saves rotating **PCAP** files for packet-level investigation

- **Desktop workstation**
  - Runs a **Splunk Universal Forwarder** to send local logs to Splunk
  - Used for reviewing alerts in Splunk
  - Used for opening PCAP files in **Wireshark**

### Log and Investigation Workflow

1. Network traffic is inspected by **Suricata** on the Raspberry Pi
2. Suricata generates alerts and log data
3. Suricata logs are forwarded to **Splunk running in Docker on the Beelink**
4. The desktop also forwards its own logs to Splunk using a **Splunk forwarder**
5. When an alert is identified, relevant **PCAP** files are pulled from the Raspberry Pi to the desktop using **SCP**
6. The PCAP files are reviewed in **Wireshark** on the desktop to validate the alert and determine impact

## Data Flow

- **Desktop → Splunk (Beelink):** local logs via Splunk Universal Forwarder
- **Raspberry Pi → Splunk (Beelink):** Suricata logs via forwarder/log ingestion
- **Raspberry Pi → Desktop:** PCAP files transferred with SCP for Wireshark analysis

## Tools Used

- **Splunk** — SIEM, alerting, log analysis
- **Suricata** — IDS / network detection
- **Wireshark** — packet analysis
- **Docker** — hosts Splunk on the Beelink
- **SCP** — transfers PCAPs from the Pi to the desktop
- **Splunk Universal Forwarder** — sends logs from endpoints to Splunk

## Investigation Focus

This repository contains documented alert investigations, including:
- known malicious IP detections
- traffic spike alerts
- multi-target detections
- false positive validation
- packet-level confirmation of SIEM alerts

## Goal

The goal of this lab is to practice real-world alert triage and packet validation by correlating:
- SIEM alerts in Splunk
- IDS detections from Suricata
- packet evidence in Wireshark
