# network-scripts

#### rt_table
101 example
 
##### ifcfg-eth1
```bash
BOOTPROTO=none
DEFROUTE=no
DEVICE=eth4
IPV4_FAILURE_FATAL=yes
IPV6INIT=no
GATEWAY=10.10.10.1
IPADDR=10.10.10.10
NETMASK=255.255.255.0
ONBOOT=yes
PEERDNS=no
PEERROUTES=no
TYPE=Ethernet
 ```
#### route-eth1
```bash
10.10.10.0/24 dev eth1 src 10.10.10.10 table example
default via 10.10.10.1 dev eth1 table example
```

#### rule-eth1
```bash
from 10.10.10.10/32 table example
to 10.20.20.20/32 table example
```

# nmcli

##### /etc/iproute2/rt_tables
```bash
100     symantec
```

```bash
nmcli connection modify eno1 ipv4.routes "10.10.10.0/24 10.10.10.1 table=100 src=10.10.10.10"
nmcli connection modify eno1 +ipv4.routes "10.20.20.20 10.10.10.1 table=100"
```

```bash
nmcli connection modify eno1 ipv4.routing-rules "priority 1 from 10.10.10.10/32 table 100"
nmcli connection modify eno1 +ipv4.routing-rules "priority 1 to 10.20.20.20/32 table 100"
```
