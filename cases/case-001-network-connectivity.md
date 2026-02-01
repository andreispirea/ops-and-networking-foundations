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
TBD

### After inducing failure
- DNS set to: <wrong DNS IP>
- `ping 8.8.8.8`: <result>
- `nslookup google.com`: <result>
- `ping google.com`: <result>
- (optional) `tracert 8.8.8.8`: <result summary>

## Root Cause
- DNS server was set to an invalid/unreachable resolver, causing name resolution failures while IP routing remained functional.

## Fix
- Set DNS back to <working DNS>
- Flushed DNS cache: `ipconfig /flushdns`

## Validation (After Fix)
- `ipconfig /all` shows DNS: <DNS>
- `nslookup google.com` succeeds
- `ping google.com` succeeds

## Prevention / Notes
- Ensure DNS settings are managed via DHCP or documented static config.
- In multi-NIC systems, confirm DNS is applied to the NIC used for outbound traffic.
- Observed asymmetric ICMP behavior: Windows client allowed outbound ICMP but blocked inbound Echo Requests by default due to Windows Firewall rules. This is expected behavior in enterprise environments and does not indicate a network connectivity issue.
