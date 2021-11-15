# Incident 2021-10-28: Judge Server Compromised

## Issue Summary
Remote attackers were able to login as `root` and run malicious executables (ex. cryptocurrency miners) on the `judge-3` droplet. Rust and other languages may have had slower than expected compilation and execution times.

**No personal data exists on the judging servers, so none was accessed.**

## Timeline (Eastern Time)
- `2021-10-28 08:26` `judge-3` droplet created with improper firewall configuration
- `2021-10-28 08:41` First sign of vulnerability being exploited
- `2021-11-18 23:34` Intrusion detected
- `2021-11-18 23:47` Replacement judge droplet, `judge-5`, created
- `2021-11-18 00:58` `judge-3` droplet deleted

## Root Cause
In the process of modifying the Ansible Judge deployment configuration to prepare extra judges for the upcoming LCC with a newer networking stack (Wireguard instead of ZeroTier) in place, the firewall was disabled due to issues with logging in to the judge through SSH.

## Resolution and recovery
Upon logging in to the infected droplet, several malicious processes were found running. One was a cryptocurrency miner while two were attempting to prevent failed SSH connections from connecting again.

Observing SSH authentication logs (`/var/log/auth*`) revealed that the droplet was vulnerable immediately after Ansible deployment configuration.

Reading bash command history files (`/root/.bash_history`, `/usr/.work/.bash_history`, and the judge docker container's), revealed the attackers did little more than quickly browse the system and install malware. It does not appear that they accessed judge problem data or any other judge-related information, though given `root` access means knowing a complete history of actions is impossible.

A replacement judge droplet, `judge-5`, was spun up without the [judge deployment SSH daemon configuration](https://github.com/mcpt/support/blob/6b4ac8d5542adb802dca700eb92638abfdf852bf/deployment/config/ssh/sshd_config) which would have allowed anyone to log in as `root` without a password given the disabled firewall.

Before deleting the `judge-3` droplet, an archive of relevant directories on the infected server was created and downloaded.

## Corrective and Preventative Measures

Overall, simply disabling a firewall should not lead to potentially immediate and total compromise of the system, so the improved security steps below should be undertaken. This is especially true when Docker is present, as its networking rules override those of UFW.

Taking users reports of *highly unusual*, slower than normal compilation or execution times for languages should be more thoroughly investigated as they may have revealed this compromise several days earlier.

*Temporarily*, Debian's default SSH daemon configuration will be used with SSH keys for judge members being added manually.

The UFW firewall has also been re-enabled, though now for the primary purpose of preventing access to the judge's problem reloading API. The judge's problem reloading API may be further secured by only listening on the internal Wireguard network's IP.

*Planned* changes including adding member's SSH keys to the [`mcpt/support`](https://github.com/mcpt/support) repository and modifying the judge deployment SSH daemon configuration to disallow password authentication entirely.

Progress may be tracked on [the Judge project](https://github.com/orgs/mcpt/projects/2).

---

Good planning with regards to the overall judge architecture, treating judging systems as insecure and potentially compromised, yielded to no personal information being compromised (as none is stored on judge servers).
