# Incident 2020-06-05: Mathoid, Texoid, Event Server down

## Issue Summary
Mathoid, texoid, and the event server ceased to function. This left certain users seeing raw latex instead of rendered text, and left all of the texoid diagrams as raw text.

## Timeline (Eastern Time)
- `2020-06-05 02:11` Issue first identified.
- `2020-06-05 12:46` First attempt at resolution made.
- `2020-06-11 19:57` Texoid functionality restored.
- `2020-06-11 20:11` Mathoid functionality restored.
- `2020-06-15 22:21` Live updates (event server functionality) restored.

## Root Cause
Unknown.

## Resolution and recovery
During the first attempt made mathoid and texoid were found in a stopped state. Both services were restarted with `docker start <id>`. This did not resolve the issue.

After further inspection of the setup, mathoid, texoid and web-event were not explicitly set up in `local_settings.py`. The following configuration was added (note the port for mathoid is not 8888 as it is in the dmoj documentation):
```python
#############
## Mathoid ##
#############
MATHOID_URL = 'http://localhost:10044'
MATHOID_CACHE_ROOT = '/code/cache/mathoid'
MATHOID_CACHE_URL = '//mcpt.ca/mathoid/'

############
## Texoid ##
############
TEXOID_URL = 'http://localhost:8888'
TEXOID_CACHE_ROOT = '/code/cache/texoid'
TEXOID_CACHE_URL = '//mcpt.ca/texoid/'

##################
## Event Server ##
##################
EVENT_DAEMON_USE = True
EVENT_DAEMON_POST = 'ws://127.0.0.1:15101/'
EVENT_DAEMON_GET = 'ws://mcpt.ca/event/'
EVENT_DAEMON_GET_SSL = 'wss://mcpt.ca/event/'
EVENT_DAEMON_POLL = '/channels/'
```

While `websocket/config.js` did exist and was properly configured (along with the nginx config), there was no supervisord daemon. `nginx_and_supervisor/` was created with the following contents:
```
[program:wsevent]
command=/usr/bin/node /code/site/websocket/daemon.js
environment=NODE_PATH="/code/site/node_modules"
user=dmoj
group=dmoj
stdout_logfile=/code/cache/logs/wsevent.stdout.log
stderr_logfile=/code/cache/logs/wsevent.stderr.log
```
This was linked to supervisord.

Mathoid, texoid, the event server, the bridged, and the site were restarted and all functionality was restored.

## Corrective and Preventative Measures
None.
