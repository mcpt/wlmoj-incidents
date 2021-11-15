---
title: Judge Server Compromised
date: 2021-10-28 08:26:00
resolved: true
resolvedWhen: 2021-11-18 00:58:00
# Possible severity levels: down, disrupted, notice
severity: disrupted
affected:
  - Judge(s)
section: issue
---

*Investigating* - We're aware of the `judge-3` droplet having malicious processes running on it. (2021-11-18 23:34)

*Investigating* - Attacker actions and impact has been investigated with the root cause, a disabled firewall with a poorly configured SSH daemon, has being identified. (2021-11-18 00:52)

*Resolved* - A replcement judge, `judge-5` has been created with the compromised `judge-3` droplet deleted. (2021-11-18 00:58)
