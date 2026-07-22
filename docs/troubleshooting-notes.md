# Troubleshooting Notes

This document contains troubleshooting observations from the Windows Server 2019 DNS lab.

The notes are based on issues and observations that appeared during the lab configuration and testing process.

## Issue 1: `Server: UnKnown` in nslookup

During DNS testing, `nslookup` displayed the following message:

```text
Server:  UnKnown
Address: 10.10.10.10
```

However, the DNS record still resolved correctly:

```text
Name:    web01.lab.local
Address: 10.10.10.20
```

### Explanation

The `Server: UnKnown` message appeared because reverse lookup was not configured.

The DNS server could resolve the forward record `web01.lab.local`, but there was no PTR record available to resolve the DNS server IP address back to a hostname.

### Conclusion

This was not a critical issue for this lab.

Forward DNS resolution worked correctly.

A reverse lookup zone and PTR record could be added in a future lab extension.

## Issue 2: nslookup Used the Router DNS Instead of the Lab DNS Server

At one point, running:

```cmd
nslookup web01.lab.local
```

queried a different DNS server:

```text
Server:  lan.home
Address: 192.168.1.1
```

This caused the query for `web01.lab.local` to fail.

### Explanation

The system was using the DNS server from another network adapter instead of the local lab DNS server.

The router DNS did not know the custom `lab.local` zone.

### Fix

The DNS client settings were adjusted so that the system used the Windows Server DNS service:

```text
Preferred DNS server: 10.10.10.10
```

After this change, the query worked:

```text
Server:  UnKnown
Address: 10.10.10.10

Name:    web01.lab.local
Address: 10.10.10.20
```

### Conclusion

For local lab zones, the client must query the DNS server that hosts the zone.

## Issue 3: Ping Resolved the Name but the Host Was Unreachable

The following command was tested:

```cmd
ping web01.lab.local
```

The hostname resolved correctly:

```text
Pinging web01.lab.local [10.10.10.20]
```

However, the host was unreachable.

### Explanation

The DNS record pointed to `10.10.10.20`, but there was no active virtual machine using that IP address.

DNS resolution and host availability are separate things.

### Conclusion

The ping test confirmed that name resolution worked, even though the destination host was not reachable.

This is expected behavior when a DNS record exists but no device is active at the target IP address.

## Key Lessons

- DNS resolution can work even when the target host is offline.
- `nslookup` may show `Server: UnKnown` if reverse lookup is not configured.
- A client must query the correct DNS server for custom local zones.
- Forward lookup zones resolve hostnames to IP addresses.
- Reverse lookup zones are used to resolve IP addresses back to hostnames.
