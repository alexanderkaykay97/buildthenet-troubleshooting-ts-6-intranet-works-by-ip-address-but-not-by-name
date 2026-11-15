# Troubleshooting #6: Intranet Works by IP Address but Not by Name

_Level: Easy — BuildTheNet 3D Sandbox troubleshooting — Name resolution_

**Status: Verified.** Completed on 2026-11-15; every requirement was machine-checked against the network built in BuildTheNet and passed.

Snapshot taken 2026-11-15. Built in a simulation — no physical or production equipment was involved.

## The ticket

**Reported symptom:** http://10.5.0.80 opens on PC-1, http://intranet.corp.local does not.

Helpdesk Ticket #106: On PC-1 the intranet opens if the user types http://10.5.0.80, but http://intranet.corp.local gives "server not found". The colleague at the next desk (PC-2) can use the name. PC-1 was re-configured by hand after a NIC replacement.

_Area: Name resolution_

## Required resolution

Make PC-1 use the company DNS server again so names resolve, without changing any server.

## Checks

| # | Check | Status | Checked by |
|---|---|---|---|
| 1 | PC-1 to its DNS server works | Passed | Sandbox check |
| 2 | PC-1 to Intranet-Web still works | Passed | Sandbox check |
| 3 | PC-2 to DNS-Server still works | Passed | Sandbox check |
| 4 | PC-1 FastEthernet0 keeps its documented addressing | Passed | Sandbox check |
| 5 | PC-2 keeps its documented addressing | Passed | Sandbox check |
| 6 | DNS-Server keeps its documented addressing | Passed | Sandbox check |
| 7 | Intranet-Web keeps its documented addressing | Passed | Sandbox check |
| 8 | Office-Router keeps its documented addressing | Passed | Sandbox check |
| 9 | The ticket's service requirement is met | Passed | Sandbox check |

## Root cause

PC-1's DNS server is 10.5.0.35, an address nothing owns; the company DNS server is 10.5.0.53.

## The fix

What was changed to resolve the ticket, compared with the network as it was reported:

Compared with the starting topology, the learner changed the configuration of 1 device.

**PC-1**

```diff
- dns-server 10.5.0.35
+ dns-server 10.5.0.53
```

## Result

Every check passed on this snapshot:

> PC-1 resolves names through the company DNS server again, and the server answers it.

## Topology

![Topology diagram](topology.svg)

## Addressing

| Device | Interface | Address | Mask | Mode | Gateway |
|---|---|---|---|---|---|
| PC-1 | FastEthernet0 | 192.168.5.15/24 | 255.255.255.0 | VLAN 1 | 192.168.5.1 |
| PC-2 | FastEthernet0 | 192.168.5.16/24 | 255.255.255.0 | VLAN 1 | 192.168.5.1 |
| Office-Router | GigabitEthernet0/0 | 192.168.5.1/24 | 255.255.255.0 | routed |  |
| Office-Router | GigabitEthernet0/1 | 10.5.0.1/24 | 255.255.255.0 | routed |  |
| DNS-Server | FastEthernet0 | 10.5.0.53/24 | 255.255.255.0 | VLAN 1 | 10.5.0.1 |
| Intranet-Web | FastEthernet0 | 10.5.0.80/24 | 255.255.255.0 | VLAN 1 | 10.5.0.1 |

## Device configurations

Running-configs as `show running-config` prints them in the BuildTheNet sandbox. Passwords, secrets, keys and SNMP communities are replaced with `<removed>`.

- [SW-Office](configs/SW-Office.txt)
- [Office-Router](configs/Office-Router.txt)
- [SW-Servers](configs/SW-Servers.txt)

---

Built on [BuildTheNet](https://www.buildthenet.io/sandbox)
