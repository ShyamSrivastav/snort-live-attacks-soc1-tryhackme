# Scenario 2 — Reverse Shell (Snort Challenge: Live Attacks)

## Overview

This scenario continues the Snort SOC exercise. After blocking inbound brute-force attempts, persistent outbound traffic is observed. The objective is to identify and stop a suspected reverse shell used by an attacker who gained an internal foothold. Using Snort, the task is to detect the outbound connection, author an IPS rule to block the reverse shell, run Snort in IPS mode to enforce the block, and validate that the lab flag appears on the desktop.

This document records the analysis, the detection and mitigation steps, the answers to lab questions, and the evidence collected.

---

## Objectives

1. Start Snort in sniffer (console) mode and observe outbound traffic to identify the source host, destination, protocol and port used by the reverse shell.
2. Create a Snort detection rule and its IPS (drop) variant to block the reverse shell traffic.
3. Test the rule in console (`-A console`) mode and then run Snort in IPS inline mode (`-A full`) to enforce the block.
4. Verify the attack is stopped and collect the flag that appears on the desktop.
5. Document commands, rule(s), and evidence (screenshots) to support findings.

---

## Detection & Mitigation — Snort Rules

**Detection (alert) rule:**
```snort
drop tcp any any <> any 4444 (msg: "Suspicious activity detected!"; sid:100002; rev:1.0)
```
Rule structure:
-  drop — blocks matching traffic when Snort runs in inline/IPS mode.
-  tcp — matches TCP traffic observed in the capture.
-  any any (source IP/port) and any 4444 (destination IP and port 4444) ensure the rule blocks any host targeting port 4444.
-  <> — bidirectional direction operator.
-  sid and rev — unique identifier and revision for the rule.
---

## Evidence — Reverse Shell

The following section documents the six screenshots collected for Scenario 2 (Reverse Shell). All screenshots are stored in the repository at `screenshots/s2_xx.png/`. Each entry includes the command used, a concise explanation of what the screenshot shows, and why it is relevant evidence.

### 1. Start Snort in sniffer (verbose) mode - ![Start Snort in sniffer (verbose) mode](/screenshots/s2_01.png)

**Command:** `sudo snort -v`

This screenshot shows Snort running in verbose sniffer mode. The -v parameter causes Snort to print packet contents to the console. This initial step is used to observe live traffic and identify anomalous outbound connections that may indicate a reverse shell.

### 2. Sniffer output showing suspicious outbound traffic - ![Sniffer output showing suspicious outbound traffic](/screenshots/s2_02.png)

Observed details
- Console output highlights repeated packets originating from `10.10.144.156:4444`.
- The output shows protocol details (TCP) and other details like flags or warnings associated with the packets.

This image captures the Snort console while analysing the traffic. The console output indicates repeated outbound TCP traffic to port 4444 from the internal host (10.10.144.156). The repeated/warning entries are the initial indication of persistent outbound connections that are consistent with a reverse shell.


### 3. Editing the local rules file - ![Editing the local rules file](/screenshots/s2_03.png)

**Command:** `sudo nano /etc/snort/rules/local.rules`

This screenshot shows the text editor (nano) being used to open /etc/snort/rules/local.rules. This is where custom Snort rules are added. The image documents the process of preparing to add a custom rule to mitigate the observed suspicious outbound traffic.

### 4. Custom rule written to mitigate the reverse shell - ![Custom rule written to mitigate the reverse shell](/screenshots/s2_04.png)

**Custom Rule:** `drop tcp any any <> any 4444 (msg:"reverse shell"; sid:100001; rev:1;)`

This screenshot displays the custom drop rule added to local.rules. The rule components are:

- action: `drop` — configured to block matching traffic when Snort runs in IPS (inline) mode.
- protocol: `tcp` — matches the observed TCP traffic.
- source_ip, source_port: `any any` — the rule applies regardless of source IP or port.
- direction: `<>` — bidirectional; matches traffic in either direction.
- destination_ip, destination_port: `any 4444` — any destination IP on port 4444 will match.
- msg: `"reverse shell"` — human-readable message to identify the rule when it fires.
- sid: `100001` — unique Snort rule identifier.
- rev: `1` — revision number for the rule.

This rule is intended to block outbound connections targeting port 4444, which in the lab environment is associated with the reverse shell.

### 5. Testing the custom rule in console mode - ![Testing the custom rule in console mode](/screenshots/s2_05.png)

**Command:** `sudo snort -C /etc/snort/rules/local.rules -A console` 

This screenshot shows the test run of Snort using `-A console` to verify that the custom rule loads without syntax errors and triggers appropriately in alert mode. Testing in console mode confirms rule syntax and expected alert output before deploying Snort in IPS mode.

### 6. Run Snort in IPS mode and obtain flag - ![Run Snort in IPS mode and obtain flag](/screenshots/s2_06.png)

**Command:** `sudo snort -C /etc/snort/rules/local.rules -q -Q --daq afpacket -i eth0:eth1 -A full` 

This screenshot documents Snort running with the custom drop rule in inline/IPS mode. Parameters used:

- `-C /etc/snort/rules/local.rules` — load the specified rules file.
- `-q` — quiet startup output.
- `-Q` — enable inline/queue mode for IPS operation (requires compatible DAQ).
- `--daq afpacket` — use the AFPacket DAQ module for packet capture/inline capability.
- `-i eth0:eth1` — define the interface pair for inline operation (example).
- `-A full` — enable full alert/logging mode.

The screenshot shows Snort dropping traffic to destination port 4444 and, after the traffic is blocked for the required period, the hidden lab flag appears on the desktop. This final image is the confirmation that the IPS rule successfully mitigated the reverse shell and that the lab task is complete.

_End of Evidence._

---
## Questions & Answers

**Q1 — Stop the attack and get the flag (which will appear on your Desktop)**  
**Answer:** `THM{0ead8c494861079b1b74ec2380d2cd24}`\
From point no. 6, we successfully obtain the hidden flag file after blocking the attack.

**Q2 — What is the used protocol/port in the attack?**  
**Answer:** `TCP/4444`\
From point no. 2, after observing the captured packets by Snort sniffer mode, we identified the source IP, port number and protocol being used in the packets.

**Q3 — Which tool is highly associated with this specific port number?**\
**Answer:** `Metasploit`\
Google search for 'associated tool' with port number '4444', to find the tool name.

---

## Conclusion & Remarks

In this scenario, persistent outbound traffic was detected from an internal host, indicative of a reverse shell. Using Snort in verbose sniffer mode, the traffic was identified as TCP connections to port 4444. A custom IPS rule was authored and deployed to block this outbound traffic. Testing confirmed the rule fired correctly, and running Snort in inline IPS mode successfully mitigated the attack. The appearance of the lab flag on the desktop validated that the reverse shell was effectively stopped.

Key Takeaways:
- Monitoring outbound traffic is crucial to detect insider threats and active reverse shells.
- Snort rules can be tailored to detect and block specific malicious traffic patterns.
- Testing rules in console mode before deploying in IPS mode ensures proper syntax and expected behavior.
- Timely detection and mitigation can prevent exfiltration of sensitive data and protect internal assets.
