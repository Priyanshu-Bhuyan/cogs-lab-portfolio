# Change Request

## Objective

Enable HTTPS redirect on Nginx gateway.

## Risk Assessment

Low risk because changes are limited to web server configuration.

## Validation

* nginx -t
* service accessibility test
* monitoring verification

## Rollback

Restore previous configuration and restart Nginx.
