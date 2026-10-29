# PHP inventory checklist (no exploit testing)

Use this on every Linux host you manage. Do not run vulnerability exploits, scanners that send PHP2Shell payloads, or unauthenticated RCE tests against production.

## Evidence that counts for an attestation

1. Package manager version (`dpkg` / `rpm`), not only `php -v` if several SAPIs exist.
2. Which SAPI is actually serving HTTP (`mod_php`, `php-fpm`, `php-cgi`).
3. Whether a web server is listening on 80/443.
4. Whether the address is private (RFC1918) or published through NAT / a WAF.

## Commands (defensive inventory)

```bash
php -v
command -v php php8.3 php8.2 php7.4 2>/dev/null
dpkg -l 'php*' 2>/dev/null | awk '/^ii/ {print $2, $3}'
apache2ctl -M 2>/dev/null | grep php
ss -lntp | grep -E ':80|:443|:9000'
systemctl is-active apache2 nginx php*-fpm 2>/dev/null
```

## SAPI risk (from the public PHP / BNR position)

| SAPI | Typical process | Relative RCE exposure |
|------|-----------------|------------------------|
| Apache `mod_php` | `apache2` / `httpd` with `libphp*.so` | Highest — request can hit the interpreter before application code |
| PHP-FPM behind nginx | `php-fpm` + `nginx` | High if the site is reachable |
| php-cgi | `php-cgi` | High if published |
| CLI only, no HTTP | `php` for cron/scripts | Lower — still inventory it |

## SIEM inventory (Wazuh)

Search **IT Hygiene / Software** for package name `php*`, not `vulnerability.package.name`.

CVE-2026-109618 and CVE-2026-109619 may be **absent** from the vulnerability feed. Absence of a CVE ID is **not** evidence that the host is safe. Use the package version against the fixed-version table.

## Classification for the supervisor table

| Class | Meaning |
|-------|---------|
| Internet-facing PHP | Public IP or confirmed DNAT / publish |
| Internal PHP | RFC1918 only; NAT still being confirmed |
| No host PHP | No `php*` packages on the OS. Containers and vendor appliances are out of scope until checked separately |
| Not assessed | No agent and no host login yet — **do not** declare PHP-free |
