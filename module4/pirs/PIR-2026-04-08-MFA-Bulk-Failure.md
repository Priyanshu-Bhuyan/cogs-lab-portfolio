# Post Incident Report (PIR)

## Incident Summary

**Incident ID:** PIR-2026-04-08-001

**Service:** MFA SMS OTP

**Priority:** P2 – High

**Status:** Resolved

---

## Timeline

09:15 IST – Customer reported SMS OTP failure.

09:30 IST – Initial investigation started.

09:45 IST – SMS gateway logs reviewed.

10:00 IST – Issue escalated to L2 Support.

10:30 IST – Backup sender ID activated.

11:00 IST – Customer confirmed successful OTP delivery.

11:15 IST – Incident closed.

---

## Root Cause

The primary SMS sender ID was blocked due to carrier DND policy, preventing SMS OTP delivery.

---

## Impact Assessment

- 25 users affected.
- Finance department unable to authenticate.
- Email OTP service remained operational.
- No data loss.

---

## Resolution

- Activated backup sender ID.
- Verified SMS gateway configuration.
- Successfully tested SMS OTP delivery.
- Customer confirmed service restoration.

---

## Preventive Measures

- Implement continuous monitoring of sender ID health.
- Configure automatic failover to backup sender ID.
- Perform regular carrier compliance checks.
- Review SMS gateway health during maintenance windows.

---

## Open Items

- Monitor SMS delivery for the next 24 hours.
- Review carrier whitelist policies.
- Update operational documentation.

---

## Lessons Learned

Maintaining a backup sender ID and proactive monitoring significantly reduces authentication service disruptions.
