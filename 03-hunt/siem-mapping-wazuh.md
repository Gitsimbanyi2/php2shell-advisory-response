# Mapping FinSOC hunts onto Wazuh

BNR FinSOC ships Sigma and ready-made queries for Splunk, Sentinel, Defender XDR, Elastic, and QRadar. This estate uses **Wazuh**. Do not import those `.spl` / `.kql` files as if they were Wazuh rules.

Run the six behaviours as **hunts** over at least 30 days before enabling standing alerts (advisory A.5). Tune false positives per host. Do not disable the rule.

## Behaviour → Wazuh telemetry

| FinSOC hunt | Log source they assume | Wazuh today (typical Linux agent) | What to add |
|-------------|------------------------|-----------------------------------|-------------|
| Web server spawns shell (Linux) | auditd / Sysmon-for-Linux process creation | Syscollector process **snapshot** only | auditd process-create on PHP hosts |
| PHP file written by web process | File-create events | FIM, if paths are watched | FIM on `/var/www`, `/srv`, upload dirs |
| Web process outbound | Network-connect events | Syscollector ports snapshot | Firewall / netflow, or auditd sockets |
| PHP-FPM crash / heap | PHP-FPM and PHP error logs | Often **not ingested** | Collect `php*-fpm.log`, PHP error log, Apache error log |
| POST `.php` 5xx burst (10 / 5 min / IP) | access.log | Often **not ingested** | Collect nginx/Apache access logs |
| Generic PHP webshell (YARA) | Files on disk | Not a SIEM query | Install `yara` **on each PHP host** |

Windows Sigma rules apply only if you have Windows PHP. Many hosted Linux estates can skip them.

## Honest 30-day statement

You may say:

* Wazuh alert archives exist for the lookback window.
* Current process tables did / did not show a shell child of php-fpm / apache2 / nginx.
* FIM did / did not watch web trees.

You may **not** say “no compromise” if access logs, PHP-FPM error logs, auditd, and YARA were not run.

## DQL notes (Wazuh dashboard)

* Software inventory: `package.name` starting with `php`.
* Vulnerability inventory: `vulnerability.id` for the CVE — expect **zero hits** until the feed contains those IDs.
* Threat Hunting: search the PHP agent names and perimeter syslog separately. Perimeter firewall syslog is not PHP evidence.
