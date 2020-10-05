# Incident 2020-05-05: Problem status symbol on site isn't being updated

## Issue Summary
The status symbol (red, yellow, green circle) based on problem completion isn't being updated after submitting to the problem.

## Timeline (Eastern Time)
- `2020-10-05 00:20` - Investigation began
- `2020-10-05 00:56` - Issue was solved 

## Root Cause
The `local_settings.py` configuration on the general server didn't have it's `CACHES` directive set to the Redis cache.

## Resolution and recovery
The `local_settings.py` of the general server had its `CACHES` variable copied from a site server. The `django_redis` pip package was also installed in the appropriate virtualenv.

## Corrective and Preventative Measures
Verify that the settings are identical on general server and sites. This issue was quickly handled within ~30 minutes, so no significant changes to procedure are required.
