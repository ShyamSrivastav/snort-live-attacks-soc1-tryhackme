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

## Questions & Answers

> The lab will provide the definitive answers when you detect and stop the reverse shell. Below are the expected answers used in many reverse‐shell lab scenarios. Replace them with the values you observe if they differ.

**Q1 — Stop the attack and get the flag (which will appear on your Desktop)**  
**Answer:** `THM{0ead8c494861079b1b74ec2380d2cd24}` (the flag will appear on the desktop once the attack is successfully blocked.)

**Q2 — What is the used protocol/port in the attack?**  
**Answer:** `TCP/4444`  

**Q3 — Which tool is highly associated with this specific port number?**  
**Answer:** `Metasploit`  

> If your lab shows a different port or a different tool (for example, a Netcat reverse shell on an arbitrary port or an HTTPS reverse shell on TCP/443), use the observed values as the answers.

---

## Detection & Mitigation — Snort Rules

Below are sample rules for detection and IPS enforcement.

**Detection (alert) rule — test in console mode (`-A console`):**
```snort
drop tcp any any <> any 4444 (msg:"Suspicious activity detected!"; sid:100002; rev:1.0)
