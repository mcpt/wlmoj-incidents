# Incident 2020-06-05: Celery broker is down

## Issue Summary
The Celery broker isn't functioning properly, making batch rejudging submissions or using the MOSS API impossible.

## Timeline (Eastern Time)
- `2020-10-14 11:33` - Investigation began
- `2020-10-18 14:24` - Issue was solved 

## Root Cause
The Celery worker was never setup to run.

## Resolution and recovery
The Celery worker was added to the supervisord config on the general server. Documentation was also added.

## Corrective and Preventative Measures
Read over the installation docs and test the bare minimum functionality of a service or tool.
