# Windows Server DNS Lab

This repository documents a basic Windows Server 2019 DNS lab created in VirtualBox.

The goal of this lab was to configure a simple DNS server, create a forward lookup zone, add an A record, and test name resolution using `nslookup` and `ping`.

## Lab Environment

- Virtualization platform: VirtualBox
- Server OS: Windows Server 2019 Standard Evaluation
- Server name: WIN-SRV-DNS01
- DNS server IP address: 10.10.10.10
- DNS zone: lab.local
- Example DNS record: web01.lab.local → 10.10.10.20

## Network Setup

The virtual machine was configured with two network adapters:

- Adapter 1: NAT  
  Used for internet access.

- Adapter 2: Internal Network / labnet  
  Used as the private lab network.

The lab adapter was configured with a static IP address:

- IP address: 10.10.10.10
- Subnet mask: 255.255.255.0
- Default gateway: not configured
- Preferred DNS server: 10.10.10.10

## Tasks Completed

- Installed Windows Server 2019 in VirtualBox
- Created a clean installation snapshot
- Renamed the server to WIN-SRV-DNS01
- Configured a static IP address for the lab network adapter
- Installed the DNS Server role
- Created a primary forward lookup zone: lab.local
- Added an A record: web01.lab.local → 10.10.10.20
- Tested DNS name resolution with `nslookup`
- Tested name resolution behavior with `ping`

## Documentation

- [Lab Overview](docs/lab-overview.md)
- [DNS Configuration Notes](docs/dns-configuration-notes.md)
- [Troubleshooting Notes](docs/troubleshooting-notes.md)

## DNS Test Results

The following command was used to query the DNS server directly:

```cmd
nslookup web01.lab.local 10.10.10.10
```

Result:

```text
Name:    web01.lab.local
Address: 10.10.10.20
```

The record was successfully resolved.

After adjusting DNS settings, the following command also resolved correctly:

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

The `Server: UnKnown` message appeared because reverse lookup / PTR records were not configured in this basic lab.

## Ping Test Observation

The following command was used:

```cmd
ping web01.lab.local
```

The hostname was resolved correctly:

```text
Pinging web01.lab.local [10.10.10.20]
```

The host itself was unreachable because `10.10.10.20` was only a DNS record and not an active virtual machine in this lab.

## What I Learned

This lab helped me practice:

- Installing and preparing a Windows Server lab environment
- Configuring a static IP address
- Installing the DNS Server role
- Creating a forward lookup zone
- Adding a basic DNS A record
- Testing DNS resolution with command-line tools
- Understanding the difference between DNS resolution and host availability

## Scope and Data Handling

This is a learning lab.

This repository does not contain:

- Real company data
- Real user data
- Passwords or credentials
- Private screenshots
- Production configuration files
- Confidential information

All examples are generic and created for learning purposes only.
