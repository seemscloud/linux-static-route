# `network-scripts`

#### `rt_table`
101 example
 
##### `ifcfg-eth1`
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
#### `route-eth1`
```bash
10.10.10.0/24 dev eth1 src 10.10.10.10 table example
default via 10.10.10.1 dev eth1 table example
```

#### `rule-eth1`
```bash
from 10.10.10.10/32 table example
to 10.20.20.20/32 table example
```

# `nmcli`

##### `/etc/iproute2/rt_tables`
```bash
100     symantec
```

#### `routes`
```bash
nmcli connection modify eno1 ipv4.routes "10.10.10.0/24 10.10.10.1 table=100 src=10.10.10.10"
nmcli connection modify eno1 +ipv4.routes "10.20.20.20 10.10.10.1 table=100"
```

#### `rules`
```bash
nmcli connection modify eno1 ipv4.routing-rules "priority 1 from 10.10.10.10/32 table 100"
nmcli connection modify eno1 +ipv4.routing-rules "priority 1 to 10.20.20.20/32 table 100"
```

# `networking`

```bash
up route add -net 192.168.0.0 netmask 255.255.0.0 gw 192.168.1.1
up route add -net 172.16.0.0 netmask 255.240.0.0 gw 192.168.1.1
```


# `netplan`
```yml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens3:
      addresses:
       - 192.168.3.30/24
      dhcp4: no
      routes:
       - to: 192.168.3.0/24
         via: 192.168.3.1
         table: 101
      routing-policy:
       - from: 192.168.3.0/24
         table: 101
    ens5:
      addresses:
       - 192.168.5.24/24
      dhcp4: no
      gateway4: 192.168.5.1
      routes:
       - to: 192.168.5.0/24
         via: 192.168.5.1
         table: 102
      routing-policy:
        - from: 192.168.5.0/24
          table: 102
```
