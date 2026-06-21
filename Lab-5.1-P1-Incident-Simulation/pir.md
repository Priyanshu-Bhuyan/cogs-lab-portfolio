# Post Incident Report (PIR)

## Incident Summary

A critical outage occurred when the Nginx gateway service became unavailable, preventing users from accessing the application.

## Detection

The outage was detected through Uptime Kuma monitoring, which reported the HTTP service as DOWN.

## Timeline

* T+0: Alert detected in Uptime Kuma
* T+5: Investigation started
* T+10: Service-level failure identified
* T+20: Nginx service restarted
* T+22: Monitoring confirmed recovery
* T+25: Incident resolved

## Root Cause

The Nginx service was not running on the target server.

## Resolution

The Nginx service was restarted and functionality was verified through monitoring and HTTP checks.

## Impact

Users were unable to access the web service during the outage period.

## Preventive Actions

1. Configure service auto-restart policies.
2. Improve monitoring and alerting coverage.
3. Perform periodic service health validation.
