# Incident 2022-12-06: Judge Database Compromised

## Report summary

On Dec 6, 2022 MCPT leadership was alerted that a vulnerability exists in the software which can
potentially be exploited remotely. An SSRF (server-side request forgery) vulnerability of *wstunnel*, an
internal service unrelated to WLMOJ, could enable read and write access to the WLMOJ SQL database.

Exploiting this vulnerability, a potential attacker could have accessed user data stored in the database.
At this time there is no evidence that personal data was accessed or edited at any time between Nov 10
and Dec 5.

Note that no passwords are stored on the WLMOJ servers which became accessible due to this
vulnerability. We recommend that all users change their password anyways as a precaution.
Databases of WLMOJ judges servers and other websites such as mCTF were not affected.

## Timeline

- 2022-11-10 22:56 wstunnel deployed with vulnerable configuration
- 2022-12-05 18:28 First evidence (logs) of wstunnel being accessed by unauthorized users
- 2022-12-06 16:15 wstunnel stopped

## Root cause

In the process of deploying *wstunnel* to help with maintenance of the servers, a configuration issue
allowed any client to send arbitrary requests as the general (mcpt.ca) server. The vulnerable service
could be used to do SSRF on the WLMOJ database server and the WLMOJ Redis server.

## Resolution and recovery

The vulnerable internal service *wstunnel* was shut down immediately after we received information of
the vulnerability.

Reviewing *wstunnel* logs revealed that several connections were made to the SQL server instance for
WLMOJ in addition to external sites such as www.google.com. There is no evidence that connections
were made to the Redis server instance (which did not store personal data) for WLMOJ.

## Corrective and Preventative Measures
To remedy this vulnerability the following security steps will be undertaken:
- The SQL server and Redis server have been secured against access from local users.
- Planned changes include increasing access logging on services such as SQL and Redis servers.
