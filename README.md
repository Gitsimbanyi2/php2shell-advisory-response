# php2shell-advisory-response

PHP2Shell Advisory Response — Inventory, Patch, Hunt & Attest Portfolio.

======================================================================================================================================

# PHP2Shell Advisory Response Portfolio

## Overview

This repository documents how a financial-sector SOC responded to the PHP2Shell advisories of 21–22 October 2026: **CVE-2026-109618** and **CVE-2026-109619** (CVSS v4.0 9.2, unauthenticated remote code execution in the PHP interpreter).

It is a **detective and remediation control pack**. It does not contain exploit code, vendor hunt-pack binaries, or production host lists.

Work completed in this response:

* Identified PHP runtimes on hosted Linux servers (package inventory + host confirmation).
* Patched confirmed 8.3 hosts to the BNR-listed fixed version **8.3.36** (Apache `mod_php`).
* Recorded remaining unfixed runtimes (PHP 8.3 below 8.3.36, and PHP 7.4 EOL).
* Mapped the FinSOC hunt behaviours onto **Wazuh** (not Splunk/Sentinel).
* Prepared a supervisory attestation outline for BNR / NCSA.

## Areas Covered

* PHP runtime inventory (packages, SAPI, HTTP listener)
* Fixed-version matrix and EOL branches
* Compensating controls when a host cannot be patched the same day
* Threat hunting mapped to Wazuh telemetry gaps
* Where to run YARA vs where to run Sigma conversion
* Attestation and overdue-action recording

## Public references

* PHP project emergency releases: 8.5.12, 8.4.27, 8.3.36, 8.2.35
* NCSA Rwanda public alert on PHP2Shell
* UK NCSC — critical security issue affecting PHP
* BNR FinSOC advisory (institutional letter; TLP:AMBER hunt pack is **not** republished here)

## Repository Structure

### 01-identify

How to find PHP on Linux hosts without exploit testing.

### 02-patch

Fixed versions, EOL branches, and compensating controls.

### 03-hunt

Wazuh mapping of the six FinSOC hunt behaviours. YARA on each PHP host; Sigma conversion on one SOC box.

### 04-attest

Attestation outline that a CISO / Ag. Senior Manager can sign. No internal addresses.

### 05-lessons

What the SIEM could and could not prove in 30 days.

## What is deliberately not in this repository

* The BNR file `php2shell-hunt-pack.zip` (TLP:AMBER — obtain from FinSOC).
* YARA / Sigma rule bodies from that pack.
* Production IP addresses, hostnames, credentials, or signed Word attestations.

## Author

Gitsimbanyi Prosper

Ag. Senior Manager | Cybersecurity and Compliance | SOC Operations
