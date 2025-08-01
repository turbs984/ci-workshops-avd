# s2-spine1

## Table of Contents

- [Management](#management)
  - [Banner](#banner)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [Enable Password](#enable-password)
  - [AAA Authorization](#aaa-authorization)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [Logging](#logging)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [Router OSPF](#router-ospf)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Banner

#### MOTD Banner

```text
You shall not pass. Unless you are authorized. Then you shall pass.
EOF
```

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management0 | OOB_MANAGEMENT | oob | default | 192.168.0.20/24 | 192.168.0.1 |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management0 | OOB_MANAGEMENT | oob | default | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management0
   description OOB_MANAGEMENT
   no shutdown
   ip address 192.168.0.20/24
```

### DNS Domain

DNS domain: atd.lab

#### DNS Domain Device Configuration

```eos
dns domain atd.lab
!
```

### NTP

#### NTP Summary

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 192.168.0.1 | - | - | - | True | - | - | - | Management0 | - |

#### NTP Device Configuration

```eos
!
ntp server 192.168.0.1 iburst local-interface Management0
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | UNIX-Socket | Default Services |
| ---- | ----- | ----------- | ---------------- |
| False | True | - | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| default | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf default
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| arista | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username arista privilege 15 role network-admin secret sha512 <removed>
username arista ssh-key ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQClIASYU353xay5h6y83IBRyGmTdeq5AlkdSdLZ7/zcOlPNzCD8H3Q7iuBrAuu4GwcktHdduGzv/qn2MCw63sqgtFRauhJxeQOrI8/rVh/oObjvpe5AMwxWW16qiE2+lrk/IhCBl3EcsaU0ArqbtflMkG5EaR3vv8i3sbpSqWHdphicWhIx4G0PWqS9VjxAFbqKOmpcTEwPmFe9FGJUFjnJbexhUPkt+HACHgPs+lFpGL11MNTg+EUEToWTP2JVFryVij4Czu3M32QjDWxrQk/V0vhtaB3fvE7YmvkGz1FJGvBfEnxAJeNO4gCRhqT5rWxoB4s4ea8Tt8XGOQIy2i09 arista@radio-canada-cbc-23-c7918b36-eos
```

### Enable Password

Enable password has been disabled

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
!
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 192.168.0.5:9910 | - | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | False |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=192.168.0.5:9910 -cvauth=token,/tmp/token -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

### Logging

#### Logging Servers and Features Summary

| Type | Level |
| -----| ----- |

| VRF | Source Interface |
| --- | ---------------- |
| default | Management0 |

| VRF | Hosts | Ports | Protocol | SSL-profile |
| --- | ----- | ----- | -------- | ----------- |
| default | 10.200.0.108 | Default | UDP | - |
| default | 10.200.1.108 | Default | UDP | - |

#### Logging Servers and Features Device Configuration

```eos
!
logging host 10.200.0.108
logging host 10.200.1.108
logging source-interface Management0
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| SPINES | Vlan4094 | 10.2.253.1 | Port-Channel1 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id SPINES
   local-interface Vlan4094
   peer-address 10.2.253.1
   peer-link Port-Channel1
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 4096 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 4096
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 30 | Thirty | - |
| 40 | Forty | - |
| 4093 | MLAG_L3 | MLAG |
| 4094 | MLAG | MLAG |

### VLANs Device Configuration

```eos
!
vlan 30
   name Thirty
!
vlan 40
   name Forty
!
vlan 4093
   name MLAG_L3
   trunk group MLAG
!
vlan 4094
   name MLAG
   trunk group MLAG
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1 | MLAG_s2-spine2_Ethernet1 | *trunk | *- | *- | *MLAG | 1 |
| Ethernet2 | L2_s2-leaf1_Ethernet2 | *trunk | *30 | *- | *- | 2 |
| Ethernet3 | L2_s2-leaf2_Ethernet2 | *trunk | *30 | *- | *- | 2 |
| Ethernet4 | L2_s2-leaf3_Ethernet2 | *trunk | *40 | *- | *- | 4 |
| Ethernet5 | L2_s2-leaf4_Ethernet2 | *trunk | *40 | *- | *- | 4 |
| Ethernet6 | MLAG_s2-spine2_Ethernet6 | *trunk | *- | *- | *MLAG | 1 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet7 | P2P_WANCORE_Ethernet2 | - | 10.0.0.37/31 | default | 1500 | False | - | - |
| Ethernet8 | P2P_WANCORE_Ethernet2 | - | 10.0.0.41/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description MLAG_s2-spine2_Ethernet1
   no shutdown
   channel-group 1 mode active
!
interface Ethernet2
   description L2_s2-leaf1_Ethernet2
   no shutdown
   channel-group 2 mode active
!
interface Ethernet3
   description L2_s2-leaf2_Ethernet2
   no shutdown
   channel-group 2 mode active
!
interface Ethernet4
   description L2_s2-leaf3_Ethernet2
   no shutdown
   channel-group 4 mode active
!
interface Ethernet5
   description L2_s2-leaf4_Ethernet2
   no shutdown
   channel-group 4 mode active
!
interface Ethernet6
   description MLAG_s2-spine2_Ethernet6
   no shutdown
   channel-group 1 mode active
!
interface Ethernet7
   description P2P_WANCORE_Ethernet2
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.0.37/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Ethernet8
   description P2P_WANCORE_Ethernet2
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.0.41/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel1 | MLAG_s2-spine2_Port-Channel1 | trunk | - | - | MLAG | - | - | - | - |
| Port-Channel2 | L2_RACK1_Port-Channel2 | trunk | 30 | - | - | - | - | 2 | - |
| Port-Channel4 | L2_RACK2_Port-Channel2 | trunk | 40 | - | - | - | - | 4 | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description MLAG_s2-spine2_Port-Channel1
   no shutdown
   switchport mode trunk
   switchport trunk group MLAG
   switchport
!
interface Port-Channel2
   description L2_RACK1_Port-Channel2
   no shutdown
   switchport trunk allowed vlan 30
   switchport mode trunk
   switchport
   mlag 2
!
interface Port-Channel4
   description L2_RACK2_Port-Channel2
   no shutdown
   switchport trunk allowed vlan 40
   switchport mode trunk
   switchport
   mlag 4
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | ROUTER_ID | default | 10.2.252.1/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | ROUTER_ID | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description ROUTER_ID
   no shutdown
   ip address 10.2.252.1/32
   ip ospf area 0.0.0.0
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan30 | Thirty | default | - | False |
| Vlan40 | Forty | default | - | False |
| Vlan4093 | MLAG_L3 | default | 1500 | False |
| Vlan4094 | MLAG | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ------ | ------- |
| Vlan30 |  default  |  10.30.30.2/24  |  -  |  10.30.30.1  |  -  |  -  |
| Vlan40 |  default  |  10.40.40.2/24  |  -  |  10.40.40.1  |  -  |  -  |
| Vlan4093 |  default  |  10.2.253.2/31  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.2.253.0/31  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan30
   description Thirty
   no shutdown
   ip address 10.30.30.2/24
   ip virtual-router address 10.30.30.1
!
interface Vlan40
   description Forty
   no shutdown
   ip address 10.40.40.2/24
   ip virtual-router address 10.40.40.1
!
interface Vlan4093
   description MLAG_L3
   no shutdown
   mtu 1500
   ip address 10.2.253.2/31
   ip ospf network point-to-point
   ip ospf area 0.0.0.0
!
interface Vlan4094
   description MLAG
   no shutdown
   mtu 1500
   no autostate
   ip address 10.2.253.0/31
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: 00:1c:73:00:dc:01

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:dc:01
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |

#### IP Routing Device Configuration

```eos
!
ip routing
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| default | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP | Exit interface | Administrative Distance | Tag | Route Name | Metric |
| --- | ------------------ | ----------- | -------------- | ----------------------- | --- | ---------- | ------ |
| default | 0.0.0.0/0 | 192.168.0.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route 0.0.0.0/0 192.168.0.1
```

### Router OSPF

#### Router OSPF Summary

| Process ID | Router ID | Default Passive Interface | No Passive Interface | BFD | Max LSA | Default Information Originate | Log Adjacency Changes Detail | Auto Cost Reference Bandwidth | Maximum Paths | MPLS LDP Sync Default | Distribute List In |
| ---------- | --------- | ------------------------- | -------------------- | --- | ------- | ----------------------------- | ---------------------------- | ----------------------------- | ------------- | --------------------- | ------------------ |
| 100 | 10.2.252.1 | enabled | Vlan4093 <br> Ethernet7 <br> Ethernet8 <br> | disabled | 12000 | disabled | disabled | - | - | - | - |

#### Router OSPF Router Redistribution

| Process ID | Source Protocol | Include Leaked | Route Map |
| ---------- | --------------- | -------------- | --------- |
| 100 | connected | disabled | - |

#### OSPF Interfaces

| Interface | Area | Cost | Point To Point |
| -------- | -------- | -------- | -------- |
| Ethernet7 | 0.0.0.0 | - | True |
| Ethernet8 | 0.0.0.0 | - | True |
| Vlan4093 | 0.0.0.0 | - | True |
| Loopback0 | 0.0.0.0 | - | - |

#### Router OSPF Device Configuration

```eos
!
router ospf 100
   router-id 10.2.252.1
   passive-interface default
   no passive-interface Ethernet7
   no passive-interface Ethernet8
   no passive-interface Vlan4093
   redistribute connected
   max-lsa 12000
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |

### VRF Instances Device Configuration

```eos
```
