# DNS Configuration Notes

This document describes the DNS configuration completed in the Windows Server 2019 DNS lab.

## DNS Server Role

The DNS Server role was installed on Windows Server 2019 using Server Manager.

Path used:

Manage → Add Roles and Features → Role-based or feature-based installation → DNS Server

After installation, DNS Manager became available through:

Tools → DNS

## Forward Lookup Zone

A primary forward lookup zone was created.

| Setting | Value |
|---|---|
| Zone type | Primary zone |
| Zone name | lab.local |
| Dynamic updates | Not allowed |
| Active Directory integration | Not used |

Active Directory integration was not used because this was a standalone DNS lab, not an Active Directory domain lab.

## A Record

An A record was created inside the `lab.local` zone.

| Setting | Value |
|---|---|
| Record type | A |
| Hostname | web01 |
| Fully qualified domain name | web01.lab.local |
| IP address | 10.10.10.20 |

The IP address `10.10.10.20` was used as a fictional lab host address.

At this stage, there was no active virtual machine using this IP address. The goal was to test DNS name resolution, not host connectivity.

## DNS Client Settings

The server was configured to use itself as the preferred DNS server for the lab network.

| Setting | Value |
|---|---|
| Preferred DNS server | 10.10.10.10 |
| Alternate DNS server | Not configured |

After this change, DNS queries for `lab.local` records were handled by the local DNS server.

## Name Resolution Test

The DNS record was tested with `nslookup`.

Query:

```cmd
nslookup web01.lab.local
```

Result:

```text
Server:  UnKnown
Address: 10.10.10.10

Name:    web01.lab.local
Address: 10.10.10.20
```

The result confirmed that the DNS server resolved `web01.lab.local` to `10.10.10.20`.

## Server: UnKnown Observation

The `Server: UnKnown` message appeared because reverse lookup was not configured.

This did not prevent forward name resolution from working correctly.

A reverse lookup zone and PTR record could be added in a future lab extension.

## Ping Test Observation

The name was also tested with `ping`.

The hostname resolved correctly:

```text
Pinging web01.lab.local [10.10.10.20]
```

The host was unreachable because `10.10.10.20` was only a DNS record and not an active machine.

This confirmed an important difference:

- DNS resolution means that a hostname can be translated into an IP address.
- Host availability means that a real machine is reachable at that IP address.

## Summary

The DNS configuration was successful.

The lab confirmed that a Windows Server 2019 DNS server can resolve a custom hostname from a primary forward lookup zone.
