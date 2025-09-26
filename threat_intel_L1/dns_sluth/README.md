# DNS Sluth

## Infection Path Analysis

### 1. Malicious File Detection
From the `vt_reports.json` file, I found a highly suspicious file with these characteristics:
- **SHA256**: `0f05e0d0313a1afa5702b53f91aa1812b55815d9ae67d0ac7681215084a3518e`
- **Detection**: 47/70 antivirus engines flagged it as malicious
- **Tags**: `macro.dropper`, `powershell`, `emotet-like`
- **Related Hash**: `deadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeefdeadbeef`

This file appears to be the initial infection vector - a malicious document with macros that drops a PowerShell-based payload.

### 2. DNS Query Evidence
From the `network.log` file, I can see DNS queries to various domains. Among the legitimate-looking queries (telemetry.microsoft.com, windowsupdate.com, cdn.cloudflare.com, ocsp.verisign.net), there are **two suspicious domains**:

```
[DNS] Query malicious-ops.secpen.net -> 185.223.142.130
[DNS] Query malicious-ops.secpen.net -> 185.180.208.228
```

### 3. C2 Domain Identification
The domain `malicious-ops.secpen.net` appears twice in the DNS logs with different IP addresses, indicating C2 communication.

### 4. Additional Evidence
In the `pastebin.txt` file, Paste 9 contains a Base64 encoded string:
```
ZmxhZ3ttYWxpY2lvdXMtb3BzLnNlY3Blbi5uZXR9
```

When decoded, this reveals:
```bash
echo "ZmxhZ3ttYWxpY2lvdXMtb3BzLnNlY3Blbi5uZXR9" | base64 -d
```
**Output**: `flag{malicious-ops.secpen.net}`

This confirms the C2 domain and even provides it in flag format.

## Conclusion

**Infection Path:**
1. Victim opens malicious document (`0f05e0d0313a1afa5702b53f91aa1812b55815d9ae67d0ac7681215084a3518e`)
2. Macros execute, dropping PowerShell payload
3. Malware establishes C2 communication with `malicious-ops.secpen.net`
4. DNS queries are made to the C2 servers at `185.223.142.130` and `185.180.208.228`

**C2 Domain:** `malicious-ops.secpen.net`

The flag format in the pastebin data confirms this is the correct C2 domain used in the attack.

```
SPL{malicious-ops.secpen.net}
```