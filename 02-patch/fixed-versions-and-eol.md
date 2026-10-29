# Fixed versions and remaining risk

## Upstream fixed releases (PHP project, 21 October 2026)

| Branch | Minimum safe version |
|--------|----------------------|
| 8.5 | 8.5.12 |
| 8.4 | 8.4.27 |
| 8.3 | 8.3.36 |
| 8.2 | 8.2.35 |
| 8.1 and earlier | No upstream fix. Vendor backport, migrate, or isolate |

PHP 7.4 is end-of-life. There is no official 7.4 security release for these CVEs. Isolation plus migration is the control, not “wait for 7.4.x”.

## What “we patched” means

A host is **updated** only when:

* `php -v` (and the Apache module / FPM package) shows a listed fixed version, and
* the binary on disk matches that version (for example `/usr/bin/php8.3` and `libphp8.3.so`).

A reboot or `apt update` without an actual version change is **not** patched.

## Compensating controls (same day, if patch is blocked)

Use these only as interim controls. The vendor update is the complete remediation.

* Restrict TCP 80/443 on the PHP host to jumpbox / admin networks at host firewall and the perimeter.
* Confirm there is no DNAT of the PHP listener to the internet.
* Do not expose phpMyAdmin or other PHP admin UIs on database servers.
* Restore package-repository access if `apt-get update` fails, then patch.
* Do not stop production services from SOC without a change window.

## Typical leftover cases

| Situation | Action |
|-----------|--------|
| PHP 8.3.x below 8.3.36 (sury or distro) | Upgrade to 8.3.36+ in a change window |
| `apt-get update` fails (no repo / no internet) | Restore repo path; keep HTTP restricted until patched |
| PHP 7.4 `mod_php` on a database host | Isolate Apache immediately; migrate off 7.4 or remove the web stack from the DB server |
| Host has no Wazuh agent | Still patch and hunt; enrol the agent afterwards |

## Change hygiene

Back up configuration and data before upgrading PHP. Restart only the PHP SAPI / web server in the agreed window. Confirm `php -v` and the Apache module version after the change.
