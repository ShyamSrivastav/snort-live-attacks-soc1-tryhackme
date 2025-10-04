# Scenario 1 — Brute Force (Snort Challenge: Live Attacks)

## Overview

J&Y Enterprise, a technology‑focused coffee retailer, stores a super‑secret recipe in a digital safe. Attackers targeted the environment with a brute‑force campaign aimed at the service that controls access to the safe. Using Snort, the objective is to detect the brute‑force activity, author an IPS rule that blocks the attack, and validate that the attack has been stopped. When the attack is blocked correctly, the flag appears on the desktop.

This document records the approach, the evidence collected, and the final results of the scenario.

---

## Objectives

1. Start Snort in sniffer (console) mode and observe traffic to identify the attack source, target service, and port.
2. Craft a Snort IPS rule to stop the brute‑force attack.
3. Test the rule in console mode, then run Snort in IPS mode to block the attack.
4. Confirm the attack is blocked and collect the flag that appears on the desktop.
5. Document evidence (screenshots) and lessons learned.

---

## Questions & Answers

**Q1 — Stop the attack and get the flag (which will appear on your Desktop)**  
**Answer:** `THM{81b7fef657f8aaa6e4e200d616738254}`

**Q2 — What is the name of the service under attack?**  
**Answer:** `SSH`

**Q3 — What is the used protocol/port in the attack?**  
**Answer:** `TCP/22`

---

## Detection & Mitigation — Snort Rule

Below is a sample detection rule (alert) and the IPS version (drop). These are intended for the lab environment. Adjust HOME_NET, interface, and file locations according to your Snort installation.

**Detection (alert) rule — for testing in console `-A console`:**
```snort
drop tcp any 22 <> any any (msg:"ssh login detected!"; sid: 100001; rev:1.0)
