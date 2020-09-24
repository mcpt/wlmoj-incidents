# Incident 2020-08-28: Live updates down for submission updates

## Issue Summary
Live grading and submission updates didn't work.

## Timeline (Eastern Time)
- `2020-08-28 11:05` The issue was identified
- `2020-09-21 20:54` An attempt to solve the issue was made by switching to RabbitMQ
- `2020-09-23 23:07` The issue was resolved by fixing the old event server

## Root Cause
The `EVENT_DAEMON_USE` setting was not set to `True` for the bridge.

## Resolution and recovery
Initially, switching to RabbitMQ alongside the AMQP event server was attempted. However, once the `local_settings.py` file was checkd on the general server, the issue was found and fixed by setting it to `True`. Installing the `websocket-client` on the general server's python environment was also necessary for the bridge to run.

## Corrective and Preventative Measures
Better analysis of the issues (realizing that the issue *didn't* happen with tickets) and more investigation before attempting a resolution would have saved lots of time.
