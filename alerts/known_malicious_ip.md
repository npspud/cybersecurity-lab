# Alert Investigation: Known Malicious IP Detected

## Timestamp
2026-03-26 ~20:23

---

## Summary

An alert was generated indicating communication with a known malicious IP address. Packet capture analysis was performed to validate the alert and determine the nature and severity of the activity.

---

## Network Details

- Source IP: 20.65.193.130 (External)
- Destination IP: 192.168.1.13 (Internal Host)
- Protocol: TCP
- Destination Port: 443 (HTTPS)

---

## Investigation Process

1. Alert identified in Splunk SIEM
2. Timestamp correlated with Suricata PCAP rotation window
3. Relevant packet capture retrieved from Suricata logging directory
4. Packet capture opened in Wireshark for analysis
5. Applied filter to isolate traffic:

wireshark:
ip.addr == 20.65.193.130

---

## Findings

- A single TCP SYN packet was observed from the external IP address
- No SYN-ACK or ACK response was recorded
- No payload or application-layer data present
- No repeated connection attempts detected
- No additional ports were targeted

---

## Analysis

- The observed traffic represents an initial TCP connection attempt
- Lack of reponse indicates the connection was not established
- Behavior is consistent with opportunistic internet scanning activity
- No evidence of exploitation or lateral movement
- No abnormal traffic patterns beyond the single connection attempt

---

## Security Context

- The source IP is flagged as malicious based on threat intelligence
- Behavior observed in the packet capture does not indicate active exploitation
- Firewall (UFW) likely blocked or dropped the connection attempt

---

## Risk Assessment

Severity: Low

- No successful connection established
- No payload delivery
- No persistence or follow-up activity
- Activity aligns with common background internet scanning

---

## Conclusion

The alert corresponds to a single external connection attempt from a known malicious IP. Packet-level analysis confirms that no connection was established and no malicious payload was delivered.

This activity is consistent with low-risk reconnaissance or opportunistic scanning and does not indicate system compromise.

---

## Tools Used

- Splunk (SIEM)
- Suricata (IDS)
- Wireshark (Packet Analysis)

---

## Key Takeaways

- Alerts must be validated with packet-level evidence
- Known malicious IP alerts don't always indicate active threats
- Packet inspection is critical for determining true impact
- Correlating SIEM alerts with PCAP data provides full visibility into network activity
