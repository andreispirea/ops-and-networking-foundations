# Case 001 — DNS Resolution Failure (Windows Client)

## Date
01-02-2026

## Environment 
- Windows 11: LAB-W1-WIN11, NICs: NAT + Host-only
- Ubuntu Server: LAB-W1-UBU-SRV, Host-only
- Host-only subnet: 192.168.56.0/24

## Impact
- Windows client was unable to access external services using domain names.
- Applications relying on DNS resolution were impacted.
- Direct IP connectivity to external hosts remained functional.

## Symptoms
- `nslookup google.com` failed with DNS request timeouts.
- `ping google.com` failed after DNS cache was flushed.
- `ping 8.8.8.8` continued to succeed, indicating IP connectivity was available.

## Initial Hypotheses
1. DNS resolver misconfigured or unreachable on the outbound network interface.
2. General network connectivity issue (routing or default gateway problem).
3. Local DNS cache serving stale entries and masking resolver failure.

## Tests & Evidence

### Baseline (Before Failure)
- `ipconfig /all`: NAT interface configured via DHCP with valid DNS server.
- `ping 8.8.8.8`: Success, confirming outbound IP connectivity.
- `nslookup google.com`: Success, confirming functional DNS resolution.
- `ping google.com`: Success.

### After Inducing Failure
- DNS server on NAT interface manually set to `10.10.10.10` (invalid).
- `ping 8.8.8.8`: Success, confirming routing was unaffected.
- `nslookup google.com`: Failed with DNS request timeouts.
- `ping google.com`: Failed after DNS cache was flushed.
  
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
