# Lab Findings

## Objective

Simulate a controlled production change and rollback process.

## Change Performed

Added a custom HTTP response header to the Nginx configuration.

## Validation

* Configuration tested using `nginx -t`
* Service reloaded successfully
* Header verified using curl

## Failure Simulation

An intentional syntax error was introduced into the Nginx configuration.

## Detection

Configuration validation failed and Nginx reported a syntax error.

## Rollback

The original configuration backup was restored.

## Verification

Configuration validation passed and the service remained operational.

## Learning Outcome

Demonstrated change management, validation procedures, rollback planning, and service recovery.
