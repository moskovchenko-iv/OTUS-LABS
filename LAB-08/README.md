# EIGRP

### Выполнение

Лаботаторная схема сети
![img.png](img.png)

1. Произведем настройку EIGRP Named Mode на R16, R17, R18, R32
```
R16
!
router eigrp named
 !
 address-family ipv4 unicast autonomous-system 2
  !
  af-interface Ethernet0/3
   summary-address 0.0.0.0 0.0.0.0                      # to R32 default only
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 10.20.0.0 255.255.0.0                # only summary prefix
  exit-af-interface
  !
  af-interface Ethernet0/0
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/2
   passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 10.20.0.0 0.0.255.255
  eigrp router-id 16.16.16.16
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2
  !
  af-interface Ethernet0/3                              # to R32 default only
   summary-address ::/0
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address FD00::10:20:0:0/96                   # only summary prefix
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp router-id 16.16.16.16
 exit-address-family
```
```
R17
!
router eigrp named
 !
 address-family ipv4 unicast autonomous-system 2
  !
  af-interface Ethernet0/1
   summary-address 10.20.0.0 255.255.0.0                # only summary prefix
  exit-af-interface
  !
  af-interface Ethernet0/0
   passive-interface
  exit-af-interface
  !
  af-interface Ethernet0/2
   passive-interface
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 10.20.0.0 0.0.255.255
  eigrp router-id 17.17.17.17
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2
  !
  af-interface Ethernet0/1
   summary-address FD00::10:20:0:0/96                   # only summary prefix
  exit-af-interface
  !
  topology base
  exit-af-topology
  eigrp router-id 17.17.17.17
 exit-address-family
```
```
R18
!
router eigrp named
 !
 address-family ipv4 unicast autonomous-system 2
  !
  topology base
  exit-af-topology
  network 10.20.0.0 0.0.255.255
  eigrp router-id 18.18.18.18
 exit-address-family
 !
 address-family ipv6 unicast autonomous-system 2
  !
  topology base
  exit-af-topology
  eigrp router-id 18.18.18.18
 exit-address-family
```
```
R32
!
router eigrp named
 !
 address-family ipv4 unicast autonomous-system 2
  !
  topology base
  exit-af-topology
  network 10.20.0.0 0.0.255.255
  eigrp router-id 32.32.32.32
 exit-address-family
```
2. Проверяем таблицы маршрутизации
```
R16# show ip route eigrp
Gateway of last resort is 0.0.0.0 to network 0.0.0.0
D*    0.0.0.0/0 is a summary, 01:05:21, Null0
      10.0.0.0/8 is variably subnetted, 13 subnets, 4 masks
D        10.20.0.0/16 is a summary, 01:05:24, Null0
D        10.20.0.18/32 [90/1024640] via 10.20.2.5, 01:04:52, Ethernet0/1
D        10.20.0.32/32 [90/1024640] via 10.20.2.7, 01:05:21, Ethernet0/3
D        10.20.2.2/31 [90/1536000] via 10.20.2.5, 01:03:51, Ethernet0/1

R16# show ipv6 route eigrp
D   ::/0 [5/1280]
     via Null0, directly connected
D   FD00::10:20:0:0/96 [5/1280]
     via Null0, directly connected
D   FD00::10:20:0:18/128 [90/1024640]
     via FE80::1, Ethernet0/1
D   FD00::10:20:0:32/128 [90/1024640]
     via FE80::2, Ethernet0/3
D   FD00::10:20:2:2/127 [90/1536000]
     via FE80::1, Ethernet0/1
```
```
R17# show ip route eigrp
Gateway of last resort is not set
      10.0.0.0/8 is variably subnetted, 10 subnets, 4 masks
D        10.20.0.0/16 is a summary, 01:04:21, Null0
D        10.20.0.18/32 [90/1024640] via 10.20.2.3, 01:03:48, Ethernet0/1
D        10.20.2.4/31 [90/1536000] via 10.20.2.3, 01:02:47, Ethernet0/1

R17# show ipv6 route eigrp
D   FD00::10:20:0:0/96 [5/1280]
     via Null0, directly connected
D   FD00::10:20:0:18/128 [90/1024640]
     via FE80::1, Ethernet0/1
D   FD00::10:20:2:4/127 [90/1536000]
     via FE80::1, Ethernet0/1
```
```
R18# show ip route eigrp
      10.0.0.0/8 is variably subnetted, 10 subnets, 3 masks
D        10.20.0.0/16 [90/1024640] via 10.20.2.4, 01:09:29, Ethernet0/0
                      [90/1024640] via 10.20.2.2, 01:09:29, Ethernet0/1

R18# show ipv6 route eigrp
D   FD00::10:20:0:0/96 [90/1024640]
     via FE80::2, Ethernet0/0
     via FE80::2, Ethernet0/1
```
```
R32# show ip route eigrp 2
Gateway of last resort is 10.20.2.6 to network 0.0.0.0
D*    0.0.0.0/0 [90/1024640] via 10.20.2.6, 01:01:58, Ethernet0/0

R32# show ipv6 route eigrp
D   ::/0 [90/1024640]
     via FE80::1, Ethernet0/0
```