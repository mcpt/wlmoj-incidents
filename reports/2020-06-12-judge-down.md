# Incident 2020-06-12: Judge Down

## Issue Summary
An update to the WLMOJ-judge removed functionality that required it to start, causing downtime.

## Timeline (Eastern Time)
- `2020-06-12 19:00` The [judge](https://github.com/mcpt/WLMOJ-judge) (`cptjudge`) was update to commit `5f53f71`.
- `2020-06-12 19:58` The `jury` judge was started to replace `cptjudge` during its downtime.
- `2020-06-15 19:00` The judge configuration was updated and functionality was restored.

## Root Cause
Commit [`2561691`](https://github.com/DMOJ/judge-server/commit/25616914a75121067e9e6e908c969b4400b7274b) removed multi-judge support which meant that the current configuration file was no longer suitable. [`9eaa572`](https://github.com/DMOJ/judge-server/commit/9eaa57246904f91c17b9a350d63dca7082b45a0c) also added jemalloc support which did not function correctly.

## Resolution and recovery
The judge configuration file was changed from:
```docker
judges:
 - {id: cptjudge.1, key: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"}
problem_storage_root:
 - /problems/
```
to
```docker
id: cptjudge.1
key: "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
problem_storage_root:
 - /problems/
```

Additionally, the docker entry file had to have this line commented out:
```docker
#export LD_PRELOAD="$(jemalloc-config --libdir)/libjemalloc.so.$(jemalloc-config --revision)"
```

From a previous attempt at resolution, the docker-compose entry for `cptjudge` had a `network_host` of `host` which had to be removed because it would conflict withe the bridge.

---

Further changes should be done to allow jemalloc and newer languages to function.

## Corrective and Preventative Measures
Plans to have an automatic backup judge startup on primary judge failure is planned. Judge updates will also occur more often to make it easier to narrow down problems.
