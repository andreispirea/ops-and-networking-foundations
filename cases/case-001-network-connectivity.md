# Case 001 — DNS Resolution Failure (Windows Client)

## Date
01-02-2026

## Environment 
- Windows 11: LAB-W1-WIN11, NICs: NAT + Host-only
- Ubuntu Server: LAB-W1-UBU-SRV, Host-only
- Host-only subnet: 192.168.56.0/24

## Impact
TBD

## Symptoms
TBD

## Initial Hypotheses
TBD

## Tests & Evidence (Baseline)
- Initial DNS change was applied to the Host-only interface, which does not carry outbound traffic.
- Windows continued resolving domains via the NAT interface DNS due to default gateway and interface priority.
- DNS failure was correctly induced only after misconfiguring DNS on the NAT interface.

### After inducing failure

## Root Cause
- DNS server on the NAT (outbound) network interface was manually set to an invalid resolver (10.10.10.10), causing name resolution failures while IP routing remained functional.

## Fix
- Restored DNS configuration on the NAT NIC network interface to a valid resolver via DHCP.
- Flushed local DNS cache to remove stale entries.

## Validation (After Fix)
- `ipconfig /all` confirms valid DNS server is configured on the NAT interface.
- `nslookup google.com` succeeds and returns a valid IP address.
- `ping google.com` succeeds, confirming name resolution and connectivity are restored.


## Prevention / Notes
- Ensure DNS settings are managed via DHCP or documented static config.
- On multi-homed systems, outbound traffic and DNS resolution follow the interface with a default gateway; setting DNS on non-routing interfaces will not affect name resolution.
- When diagnosing connectivity issues, verify both IP-level connectivity and DNS resolution, and be aware of local DNS caching behavior.
- Observed asymmetric ICMP behavior: Windows client allowed outbound ICMP but blocked inbound Echo Requests by default due to Windows Firewall rules. This is expected behavior in enterprise environments and does not indicate a network connectivity issue.
