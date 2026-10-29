# Where to install YARA and Sigma

## Short answer

| Tool | Install where | Why |
|------|----------------|-----|
| YARA | **Each Linux PHP host** | It reads files on that disk (`/var/www`, `/srv`, `/tmp`, upload dirs) |
| YARA | Optional copy on the Wazuh manager or a SOC VM | Store the `.yar` file and SHA256; do not scan production from here |
| sigma-cli | **One** SOC workstation, Wazuh manager, or analysis VM | Converts Sigma rules. Not a scanner. Not needed on PHP servers |
| New dedicated server | Useful as a jump / analysis box | Does **not** replace host YARA unless you SSH to the PHP hosts anyway |

The Wazuh manager cannot see `/var/www` on other machines. Copying web roots to a central scanner is the wrong control (extra copy of possibly sensitive files).

## YARA — first hunt (BNR Section 5)

On each PHP host, off-peak, without restarting Apache/nginx:

```bash
sudo apt-get update
sudo apt-get install -y yara
```

Copy **only** the FinSOC `.yar` file onto the host (not the whole TLP:AMBER zip). Then:

```bash
sudo yara -r php2shell_generic_php_webshell.yar /var/www /srv /tmp
```

Add real vhost and upload paths. Review matches; obfuscated commercial plugins can hit the loader rule.

Hosts without a Wazuh agent are still scanned over SSH the same way. Enrol the agent later if you want scheduled scans.

After the first hunt, Wazuh can **orchestrate** YARA via a command wodle or FIM integration. The `yara` binary still runs **on the agent**.

## Sigma — conversion only

```bash
pip install sigma-cli
sigma plugin install <backend>
```

BNR tested Splunk, Defender XDR, Sentinel ASIM, Elastic, QRadar. There is **no** Wazuh backend in that pack. For Wazuh, use the mapping in `siem-mapping-wazuh.md` instead of converting and hoping the field names match.

## If a hunt matches on an internet-facing host

Treat it as a suspected incident: isolate, preserve disk and memory before rebuild, rotate credentials stored on or reachable from the host, and report to the supervisor / BNR immediately. Do not wait for the attestation date.
