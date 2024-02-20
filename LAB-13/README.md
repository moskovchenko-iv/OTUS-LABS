# VPN. GRE. DmVPN

### Выполнение

Лаботаторная схема сети
![img.png](img.png)

1. Настроим GRE между офисами Москва и С.-Петербург.
   ```
   Конфигурация на R15:
   R15# show running-config interface tunnel 0
   !
   interface Tunnel0
   description TUN-to-R18-PETERBURG
   ip address 10.0.3.15 255.255.255.0
   ip mtu 1400
   ip tcp adjust-mss 1360
   tunnel source Loopback1
   tunnel destination 123.18.18.1
   end
   !
   ip route 10.20.0.0 255.255.0.0 Tunnel0

   Конфигурация на R18:
   R18# show running-config interface tunnel 0
   !
   interface Tunnel0
   description TUN-to-R15-MOSKOW
   ip address 10.0.3.18 255.255.255.0
   ip mtu 1400
   ip tcp adjust-mss 1360
   tunnel source Loopback1
   tunnel destination 123.15.15.1
   end
   !
   ip route 10.10.0.0 255.255.0.0 Tunnel0
   
   Проверяем связность к VPC:
   !
   R15# ping 10.20.101.100
   Type escape sequence to abort.
   Sending 5, 100-byte ICMP Echos to 10.20.101.100, timeout is 2 seconds:
   !!!!!
   Success rate is 100 percent (5/5), round-trip min/avg/max = 2/3/6 ms
   !
   R18# ping 10.10.101.4
   Type escape sequence to abort.
   Sending 5, 100-byte ICMP Echos to 10.10.101.4, timeout is 2 seconds:
   !!!!!
   Success rate is 100 percent (5/5), round-trip min/avg/max = 2/3/7 ms
   ```
2. Настроим DMVPN между Москва и Чокурдах, Лабытнанги.
   ```
   Настройки на R15 (Москва):
   !
   interface Loopback1
   description REAL-IP-FOR-TEST
   ip address 123.15.15.1 255.255.255.0
   end
   !
   interface Tunnel1
   ip address 10.0.4.15 255.255.255.0
   no ip redirects
   ip mtu 1400
   ip nhrp map multicast dynamic
   ip nhrp network-id 1
   ip tcp adjust-mss 1360
   ip ospf network broadcast
   ip ospf priority 255
   ip ospf 1 area 3
   tunnel source Loopback1
   tunnel mode gre multipoint
   end

   Настройки на R27 (Лабытнанги):
   !
   interface Loopback1
   ip address 123.25.25.17 255.255.255.248
   end
   !
   interface Tunnel1
   ip address 10.0.4.27 255.255.255.0
   no ip redirects
   ip mtu 1400
   ip nhrp map 10.0.4.15 123.15.15.1
   ip nhrp map multicast 123.15.15.1
   ip nhrp network-id 1
   ip nhrp nhs 10.0.4.15
   ip nhrp registration no-unique
   ip tcp adjust-mss 1360
   ip ospf network broadcast
   ip ospf priority 0
   ip ospf 1 area 3
   tunnel source Loopback1
   tunnel mode gre multipoint
   end
   !
   R27# traceroute 10.0.4.28 source tunnel 1
   Type escape sequence to abort.
   Tracing the route to 10.0.4.28
   VRF info: (vrf in name/id, vrf out name/id)
   1 10.0.4.28 2 msec 2 msec *

   Настройки на R28 (Чокурдах):
   !
   interface Loopback1
   ip address 123.25.25.9 255.255.255.248
   end
   !
   interface Tunnel1
   ip address 10.0.4.28 255.255.255.0
   no ip redirects
   ip mtu 1400
   ip nhrp map 10.0.4.15 123.15.15.1
   ip nhrp map multicast 123.15.15.1
   ip nhrp network-id 1
   ip nhrp nhs 10.0.4.15
   ip tcp adjust-mss 1360
   ip ospf network broadcast
   ip ospf priority 0
   ip ospf 1 area 3
   tunnel source Loopback1
   tunnel mode gre multipoint
   end
   !
   R28# traceroute 10.0.4.27 source tunnel 1
   Type escape sequence to abort.
   Tracing the route to 10.0.4.27
   VRF info: (vrf in name/id, vrf out name/id)
   1 10.0.4.27 3 msec 2 msec *
   ```
3. Все узлы в офисах в лабораторной работе имеют IP связность.
   ```
   Связность между сетями обеспечена по ospf area 3

   VPCS_1> ping 10.70.101.100
   
   84 bytes from 10.70.101.100 icmp_seq=1 ttl=60 time=4.663 ms
   84 bytes from 10.70.101.100 icmp_seq=2 ttl=60 time=4.067 ms
   84 bytes from 10.70.101.100 icmp_seq=3 ttl=60 time=4.875 ms
   84 bytes from 10.70.101.100 icmp_seq=4 ttl=60 time=4.561 ms
   84 bytes from 10.70.101.100 icmp_seq=5 ttl=60 time=3.872 ms
   
   VPCS_1> ping 10.70.102.100
   
   84 bytes from 10.70.102.100 icmp_seq=1 ttl=60 time=5.580 ms
   84 bytes from 10.70.102.100 icmp_seq=2 ttl=60 time=3.612 ms
   84 bytes from 10.70.102.100 icmp_seq=3 ttl=60 time=5.907 ms
   84 bytes from 10.70.102.100 icmp_seq=4 ttl=60 time=6.399 ms
   84 bytes from 10.70.102.100 icmp_seq=5 ttl=60 time=5.535 ms
   
   VPCS_7> ping 10.70.101.100
   
   84 bytes from 10.70.101.100 icmp_seq=1 ttl=60 time=4.974 ms
   84 bytes from 10.70.101.100 icmp_seq=2 ttl=60 time=5.816 ms
   84 bytes from 10.70.101.100 icmp_seq=3 ttl=60 time=7.893 ms
   84 bytes from 10.70.101.100 icmp_seq=4 ttl=60 time=4.828 ms
   84 bytes from 10.70.101.100 icmp_seq=5 ttl=60 time=5.780 ms
   
   VPCS_7> ping 10.70.102.100
   
   84 bytes from 10.70.102.100 icmp_seq=1 ttl=60 time=4.838 ms
   84 bytes from 10.70.102.100 icmp_seq=2 ttl=60 time=6.115 ms
   84 bytes from 10.70.102.100 icmp_seq=3 ttl=60 time=6.298 ms
   84 bytes from 10.70.102.100 icmp_seq=4 ttl=60 time=7.792 ms
   84 bytes from 10.70.102.100 icmp_seq=5 ttl=60 time=4.458 ms

   VPCS_30> ping 10.10.101.4
   
   84 bytes from 10.10.101.4 icmp_seq=1 ttl=60 time=5.713 ms
   84 bytes from 10.10.101.4 icmp_seq=2 ttl=60 time=4.035 ms
   84 bytes from 10.10.101.4 icmp_seq=3 ttl=60 time=3.980 ms
   84 bytes from 10.10.101.4 icmp_seq=4 ttl=60 time=5.724 ms
   84 bytes from 10.10.101.4 icmp_seq=5 ttl=60 time=4.401 ms
   
   VPCS_30> ping 10.10.102.4
   
   84 bytes from 10.10.102.4 icmp_seq=1 ttl=60 time=5.410 ms
   84 bytes from 10.10.102.4 icmp_seq=2 ttl=60 time=4.275 ms
   84 bytes from 10.10.102.4 icmp_seq=3 ttl=60 time=4.618 ms
   84 bytes from 10.10.102.4 icmp_seq=4 ttl=60 time=4.080 ms
   84 bytes from 10.10.102.4 icmp_seq=5 ttl=60 time=4.232 ms

   VPCS_31> ping 10.10.101.4

   84 bytes from 10.10.101.4 icmp_seq=1 ttl=60 time=5.017 ms
   84 bytes from 10.10.101.4 icmp_seq=2 ttl=60 time=5.835 ms
   84 bytes from 10.10.101.4 icmp_seq=3 ttl=60 time=5.206 ms
   84 bytes from 10.10.101.4 icmp_seq=4 ttl=60 time=6.902 ms
   84 bytes from 10.10.101.4 icmp_seq=5 ttl=60 time=9.638 ms
   
   VPCS_31> ping 10.10.102.4
   
   84 bytes from 10.10.102.4 icmp_seq=1 ttl=60 time=5.379 ms
   84 bytes from 10.10.102.4 icmp_seq=2 ttl=60 time=4.248 ms
   84 bytes from 10.10.102.4 icmp_seq=3 ttl=60 time=4.946 ms
   84 bytes from 10.10.102.4 icmp_seq=4 ttl=60 time=6.275 ms
   84 bytes from 10.10.102.4 icmp_seq=5 ttl=60 time=4.753 ms
   ```