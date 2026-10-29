# Attestation outline (sign as Ag. Senior Manager, Cybersecurity and Compliance)

Fill host counts from **your** inventory. Do not paste production addresses into a public copy of this file.

## Header

* Institution
* Advisory reference and date
* CVEs: CVE-2026-109618, CVE-2026-109619
* Classification: supervisory / restricted
* Signatory name, title, telephone, email, signature, stamp, date

## Section 1 — Scope

Systems under operational control of the signatory. State what is **out of scope** (third-party websites, mobile banking vendors, appliances that embed PHP).

## Section 2 — Required table

| Item | Finding |
|------|---------|
| PHP-dependent systems identified | Count + class (internet-facing / internal / not assessed) |
| Updated to a listed fixed version | Count and versions (8.5.12 / 8.4.27 / 8.3.36 / 8.2.35) |
| Not yet on a fixed version | Count, current version, SAPI, compensating control, target date |
| Unsupported PHP (7.4 / 8.1 and earlier) | Isolate + migrate; no upstream fix |
| Suspected or confirmed compromise | None / incident reference. State telemetry gaps honestly |
| Outstanding risks | Hosts without agents, vendor products, NAT not confirmed |

## Section 3 — Overdue actions

If identification, internet-facing patch, or 30-day hunt dates in the letter have passed, **record the delay**. Do not hide it.

## Section 4 — Declaration

The statements are true to the best of the signatory’s knowledge. Unfixed systems remain under compensating controls. Any suspected exploitation is reported immediately and does not wait for a later attestation.

## Annex — hunt vs SIEM

One row per FinSOC behaviour: available telemetry, result, residual gap.

## Do not attach to a public Git repository

Signed Word/PDF with internal IPs, the TLP:AMBER zip, or host-level command output that names production systems.
