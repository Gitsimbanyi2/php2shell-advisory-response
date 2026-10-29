# Lessons learned

## Inventory

* Wazuh Syscollector is enough to **start** PHP discovery on enrolled agents. It is not enough for hosts without an agent.
* `php -v` plus `dpkg` plus the Apache module path is the evidence BNR will accept for “updated”.
* Empty Vulnerability Detection for these CVE IDs is a **feed gap**, not a clean bill of health.

## Patch

* Apache `mod_php` 8.3 can be moved to 8.3.36 when the sury/distro repo works.
* A host that cannot `apt-get update` cannot be claimed as patched.
* PHP 7.4 on a database server with `mod_php` is a migration problem, not a point-release problem.

## Hunt

* Sigma queries for Splunk/Sentinel do not run in Wazuh.
* YARA must be installed on each PHP server (including hosts you already patched).
* Without access.log and php-fpm error logs in the SIEM, you cannot hunt POST/5xx bursts or worker crashes for 30 days in Wazuh. Collect those logs on the host.

## Attestation

* Count patched and unpatched separately. Do not let three patched hosts hide three that are still 7.4 or 8.3.19.
* Record overdue letter dates. Supervisors prefer a late honest table to a claim that everything is closed.
