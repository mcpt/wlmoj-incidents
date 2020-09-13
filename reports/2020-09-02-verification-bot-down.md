# Incident 2020-09-01: Verification Bot Down

## Issue Summary
Verification Bot failed to verify users. No error message would show and users that tried to verify themselves would get a `success` message.

## Timeline (Eastern Time)
- `2020-09-01 23:16` Issue discovered.
- `2020-09-12 20:04` Issue resolved.

## Root Cause
A code issue stemming from the fact that a variable was not properly initiated.

In addition there was another issue with the verification bot not having the correct permissions to add the `Verified` role.

## Resolution and recovery
The coding issue was fixed in https://github.com/mcpt/verify-discord/commit/e9f1b16e13d1e5391c54f122c535dae76a6e0124.

The Verification Bot role was moved above the `Verified` role.

## Corrective and Preventative Measures

Better testing of code before deployment must be practiced.
