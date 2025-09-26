# Mal medium

Based on the provided passive DNS (pdns.csv) and WHOIS data, I can identify the suspicious subdomain that briefly existed.

The suspicious subdomain is:  
**`support-hackorn.secpen.org`**

**Reasoning:**  
- In the `pdns.csv` file, there is only one entry for `support-hackorn.secpen.org` with the IP `192.168.43.68` at timestamp `2025-05-14T01:00:00`.  
- This subdomain appears only once in the entire dataset, suggesting it was created briefly and then removed — a common tactic in phishing campaigns to avoid detection.  
- The IP `192.168.43.68` is a private RFC 1918 address, which is suspicious because it should not appear in public DNS queries. This indicates possible internal testing or a misconfigured phishing setup.  
- The WHOIS info confirms `secpen.org` is a legitimate domain, making this anomalous subdomain stand out as malicious.

This matches the phishing email claim and confirms the subdomain used in the attack.

```
SPL{support-hackorn.secpen.org}
```