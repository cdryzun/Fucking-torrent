# Trackers Blocklist

> Auto-updated daily blocklist of BitTorrent tracker addresses for network-level filtering.

[![GitHub Stars](https://img.shields.io/github/stars/cdryzun/trackers?style=flat-square)](https://github.com/cdryzun/trackers/stargazers)
[![GitHub Forks](https://img.shields.io/github/forks/cdryzun/trackers?style=flat-square)](https://github.com/cdryzun/trackers/network/members)
[![Entries](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/cdryzun/trackers/main/.shields/count.json&style=flat-square)](https://raw.githubusercontent.com/cdryzun/trackers/main/trackers.txt)
[![Last Commit](https://img.shields.io/github/last-commit/cdryzun/trackers?style=flat-square)](https://github.com/cdryzun/trackers/commits/main)
[![CI](https://img.shields.io/github/actions/workflow/status/cdryzun/trackers/ci.yaml?label=daily%20update&style=flat-square)](https://github.com/cdryzun/trackers/actions)
[![License](https://img.shields.io/github/license/cdryzun/trackers?style=flat-square)](LICENSE)

## Overview

A daily-updated, deduplicated blocklist aggregated from multiple upstream sources.
Contains **domains and IP addresses** of known BitTorrent trackers, ready to drop into your firewall or DNS resolver.

**Who is this for:**
- Network admins blocking P2P traffic on corporate/campus networks
- ISPs implementing bandwidth or usage policies
- Home lab setups with Pi-hole / dnsmasq
- Security appliances (pfSense, OPNsense, Unbound, etc.)

## Raw File

```
https://raw.githubusercontent.com/cdryzun/trackers/main/trackers.txt
```

One entry per line — either a **domain name** or a **bare IP address**.

## Usage

### dnsmasq

```bash
curl -fsSL "https://raw.githubusercontent.com/cdryzun/trackers/main/trackers.txt" \
  | awk '{print "address=/"$0"/#"}' > /etc/dnsmasq.d/trackers-block.conf
systemctl reload dnsmasq
```

### Pi-hole

Add the raw URL to **Group Management → Adlists**, then update gravity:

```
https://raw.githubusercontent.com/cdryzun/trackers/main/trackers.txt
```

```bash
pihole -g
```

### Unbound

```bash
curl -fsSL "https://raw.githubusercontent.com/cdryzun/trackers/main/trackers.txt" \
  | awk '{print "local-zone: \""$0"\" refuse"}' \
  > /etc/unbound/conf.d/trackers-block.conf
```

Then add to `unbound.conf`:

```
include: "/etc/unbound/conf.d/trackers-block.conf"
```

### iptables (IP addresses only)

```bash
curl -fsSL "https://raw.githubusercontent.com/cdryzun/trackers/main/trackers.txt" \
  | grep -E '^([0-9]{1,3}\.){3}[0-9]{1,3}$' \
  | xargs -I{} iptables -A OUTPUT -d {} -j DROP
```

### /etc/hosts

```bash
curl -fsSL "https://raw.githubusercontent.com/cdryzun/trackers/main/trackers.txt" \
  | grep -v '^[0-9]' \
  | awk '{print "0.0.0.0 "$0}' \
  >> /etc/hosts
```

## Data Sources

| Source | Files used |
|--------|-----------|
| [ngosang/trackerslist](https://github.com/ngosang/trackerslist) | `trackers_all.txt`, `trackers_best.txt`, `trackers_all_https.txt` |

The pipeline extracts hostnames from full tracker URLs (strips protocol, port, and path),
merges all sources, deduplicates, and sorts the result.

## Update Schedule

Automatically updated every day at **04:15 UTC** via GitHub Actions.
The [entries badge](#) above reflects the live count from the last successful run.

[Trigger a manual update](https://github.com/cdryzun/trackers/actions/workflows/ci.yaml) from the Actions tab.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[MIT](LICENSE)
