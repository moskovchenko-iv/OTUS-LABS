# iBGP

### Выполнение

Лаботаторная схема сети
![img.png](img.png)

1. Настроим iBGP в офисе Москва между маршрутизаторами R14 и R15.
   ```
   R14# sh run | sec bgp
   router bgp 1001
   bgp log-neighbor-changes
   neighbor 10.10.0.15 remote-as 1001
   neighbor 10.10.0.15 update-source Loopback0
   neighbor FD00::10:10:0:15 remote-as 1001
   neighbor FD00::10:10:0:15 update-source Loopback0
   !
   address-family ipv4
   network 123.14.14.0 mask 255.255.255.0
   neighbor 10.10.0.15 activate
   neighbor 10.10.0.15 next-hop-self
   no neighbor FD00::10:0:0:2 activate
   no neighbor FD00::10:10:0:15 activate
   exit-address-family
   !
   address-family ipv6
   network 2001::123:14:14:0/112
   neighbor FD00::10:10:0:15 activate
   neighbor FD00::10:10:0:15 next-hop-self
   exit-address-family
   
   R15# sh run | sec bgp
   router bgp 1001
   neighbor 10.10.0.14 remote-as 1001
   neighbor 10.10.0.14 update-source Loopback0
   neighbor FD00::10:10:0:14 remote-as 1001
   neighbor FD00::10:10:0:14 update-source Loopback0
   !
   address-family ipv4
   network 123.15.15.0 mask 255.255.255.0
   neighbor 10.10.0.14 activate
   neighbor 10.10.0.14 next-hop-self
   no neighbor FD00::10:10:0:14 activate
   exit-address-family
   !
   address-family ipv6
   network 2001::123:15:15:0/112
   neighbor FD00::10:10:0:14 activate
   neighbor FD00::10:10:0:14 next-hop-self
   exit-address-family

   R14# sh ip bgp ipv4 unicast summary
   Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
   10.10.0.15      4         1001      52      51        6    0    0 00:41:26        2

   R14# sh ip bgp ipv6 unicast summary
   Neighbor          V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
   FD00::10:10:0:15  4         1001      17      20        7    0    0 00:06:55        2
   ```