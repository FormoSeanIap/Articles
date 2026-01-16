# Site Unaccessible but Accessible (Part 4): Tools and Reflections

# Commands

These are DNS-related commands and tools that were used during the incident and provided by a senior engineer.

## dig

Command format: `dig DNS_SERVER SITE_URL` (use `+short` for IP-only output)

Examples:
- Query the nearest DNS server for "service.com" (may reflect local cache): `dig service.com`
- Query Cloudflare's DNS server: `dig @1.1.1.1 service.com`
- Query Google's DNS server: `dig @8.8.8.8 service.com`
- Query our own name server: `dig <ns_name> service.com`

During the incident, different DNS servers returned different answers because propagation was still in progress.
That is why Pingdom alerted while some of us could still access the site.

## curl

- Check metadata for a specific IP (for example, to see if it is a CloudFront endpoint): `curl ipinfo.io/IP_ADDRESS`

# Tools

## Google DNS Lookup Tool

URL: https://toolbox.googleapps.com/apps/dig/

This web tool displays DNS records for a given domain and can be used to verify what Google DNS returns.

## WHOIS (Gandi)

URL: https://www.gandi.net/en/domain/p/whois

WHOIS records show the last update time and name server settings.
During the incident, we saw that the update time was only a few hours earlier and the NS settings did not match our expected configuration.
That confirmed the NS record had been changed recently.

# Reflections

I hope this incident shows a realistic path for root-cause investigation and response in a major event.
It also highlights the role SRE can play in a P0 incident: not only in technical diagnosis, but in coordinating urgent fixes and navigating communication risks.

At the same time, this kind of incident is rare.
Most SRE work does not involve full-scale emergencies, and even when major incidents occur, they are often caused by application bugs rather than operational changes.
An event like this, where SRE must be involved end-to-end, is the exception.

Author's aside: I hope I do not encounter incidents like this too often.
But it was one of the most intense and educational events of my first year as an SRE.
