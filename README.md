# snort-live-attacks-soc1-tryhackme
Snort SOC L1 hands‑on lab: Investigate live and captured traffic from the "Snort Challenge — Live Attacks" room; document detections, rules, and mitigations.
---

## Project Introduction

This repository documents step‑by‑step the approach and outcomes of two TryHackMe lab challenges. The content focuses on methodology, commands, evidence (screenshots), and lessons learned to demonstrate practical skills in offensive security techniques used in controlled lab environments.

**Scope**: The repository covers:
- [Scenario 1 — Brute Force](/scenario1-bruteforce.md)
- [Scenario 2 — Reverse Shell](/scenario2-reverseshell.md)

> All activities documented were performed in TryHackMe lab environments for learning and demonstration purposes only. **[Click Here](https://tryhackme.com/room/snortchallenges2)** to visit _**Snort Challenge - Live Attacks Room**_

---

## Skills Demonstrated

- bash scripting
- snort
- nano/vim -- text editor

---

## Tools Used

- `snort` — an open-source, rule-based Network Intrusion Detection and Prevention System (NIDS/NIPS)
- `bash` - Linux command line (Terminal)
- `nano` - Text editor
- TryHackMe platform for lab environment

---

## Repository Structure

```
snort-live-attacks-soc1-tryhackme/
├─ README.md
├─ screenshots/
│ ├─ s1_xx.png/
│ └─ s2_xx.png/
├─ scenario1-analysis.md
└─ scenario2-analysis.md
```

_**[screenshots](/screenshots)**_ — stores all visual evidence captured during the labs.

---

## Lesson Learned

- Systematic enumeration is critical: never skip version discovery and banner grabbing.
- Use targeted wordlists and throttle brute force attempts to avoid unnecessary noise.
- Always document commands used, environment variables, and timestamps for reproducibility.
- When obtaining interactive shells, upgrade to a stable TTY before performing heavy enumeration.
- Respect lab scope and legal/ethical boundaries; these exercises are for education only.

---

## Conclusion

This repository is a concise, evidence‑based record of two practical TryHackMe lab scenarios: Brute Force and Reverse Shell. It demonstrates the methodology, tools, and thought process used during the exercises. The artefacts captured here (screenshots, notes) are intended for personal portfolio use and to showcase hands‑on capability in controlled lab environments.

---
