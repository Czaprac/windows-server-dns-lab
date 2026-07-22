# Lab Overview

This document describes the basic setup of my Windows Server 2019 DNS lab.

## Lab Goal

The goal of this lab was to configure a basic DNS server on Windows Server 2019 and test local name resolution in a private lab network.

The lab focused on:

- Windows Server preparation
- Basic VirtualBox networking
- Static IP configuration
- DNS Server role installation
- Forward lookup zone configuration
- DNS A record creation
- Name resolution testing

## Environment

- Virtualization platform: VirtualBox
- Operating system: Windows Server 2019 Standard Evaluation
- Server name: WIN-SRV-DNS01
- Lab network name: labnet
- DNS server IP address: 10.10.10.10
- DNS zone: lab.local

## VirtualBox Network Configuration

The virtual machine used two network adapters:

| Adapter | Mode | Purpose |
|---|---|---|
| Adapter 1 | NAT | Internet access |
| Adapter 2 | Internal Network / labnet | Private lab network |

## Lab IP Configuration

The lab network adapter was configured manually:

| Setting | Value |
|---|---|
| IP address | 10.10.10.10 |
| Subnet mask | 255.255.255.0 |
| Default gateway | Not configured |
| Preferred DNS server | 10.10.10.10 |

The default gateway was not configured on the lab adapter because the NAT adapter was used for internet access.

## DNS Configuration Summary

The following DNS configuration was completed:

| Item | Value |
|---|---|
| DNS role | Installed |
| Forward lookup zone | lab.local |
| Record type | A |
| Record name | web01 |
| FQDN | web01.lab.local |
| IP address | 10.10.10.20 |

## Test Summary

The DNS record was tested using `nslookup`.

The name `web01.lab.local` resolved successfully to `10.10.10.20`.

The name was also tested with `ping`. The hostname resolved correctly, but the destination host was unreachable because `10.10.10.20` was only a DNS record and not an active virtual machine.

## Notes

This was a basic learning lab. It was not connected to a production environment and did not use real company data.
