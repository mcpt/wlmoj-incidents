# Incident 2020-12-04: Disrupted Maintenance Capability

## Issue Summary
Update to Zerotier caused the `auth` server not to come back online automatically.

## Timeline (Eastern Time)
- `2020-12-04 11:10` Updates began on `auth` server.
- `2020-12-04 11:18` Disruption first noticed.
- `2020-12-04 11:29` `auth` server force restarted.
- `2020-12-04 12:20` `auth` server reconnects to Zerotier network.

## Root Cause
Between Zerotier updates 1.4 and 1.6 the Systemd service file was changed. The update however did not reload Systemd which meant old versions of the service file was used to start a newer version.

## Resolution and Recovery
A DigitalOcean (DO) droplet was created and setup to be on a private DO network with the `auth` server.

Using SSH agent forwarding, the `auth` server was accessed through the newly setup server with the internal DO network IP.

From there the output of a `systemctl status` on the Zerotier service revealed the service files were out of date. From here, a `systemctl daemon-reload` reloads the files and the Zerotier service can be started.

Port 22 was then explicitly allowed from everywhere on UFW to prevent this in the future.

## Corrective and Preventative Measures
Port 22 was explicitly allowed from anywhere in the firewall allowing SSH connection to the `auth` server from outside the Zerotier network. While this decreases the overall security of this system it vastly decreases future risk of getting locked out from the server.
