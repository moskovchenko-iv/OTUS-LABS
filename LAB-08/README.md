# EIGRP

### Выполнение

Лаботаторная схема сети
![img.png](img.png)

1. Произведем настройку EIGRP Named Mode на R16,R17,R18,R32
```
R16
!
router eigrp named
 !
 address-family ipv4 unicast autonomous-system 2
  !
  af-interface Ethernet0/3
   summary-address 0.0.0.0 0.0.0.0              # R32 получает только маршрут по умолчанию
  exit-af-interface
  !
  af-interface Ethernet0/1
   summary-address 10.20.0.0 255.255.0.0        # R16-17 анонсируют только суммарные префиксы
  exit-af-interface
  !
  af-interface Ethernet0/0
   passive-interface                            # не шлем пакеты в сторону пользователей
  exit-af-interface
  !
  af-interface Ethernet0/2
   passive-interface                            # не шлем пакеты в сторону пользователей
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 10.20.0.0 0.0.255.255                 # указываем сети для анонса
  eigrp router-id 16.16.16.16
 exit-address-family
 
R16# show ip route eigrp 2
Gateway of last resort is 0.0.0.0 to network 0.0.0.0
D*    0.0.0.0/0 is a summary, 01:05:21, Null0
      10.0.0.0/8 is variably subnetted, 13 subnets, 4 masks
D        10.20.0.0/16 is a summary, 01:05:24, Null0
D        10.20.0.18/32 [90/1024640] via 10.20.2.5, 01:04:52, Ethernet0/1
D        10.20.0.32/32 [90/1024640] via 10.20.2.7, 01:05:21, Ethernet0/3
D        10.20.2.2/31 [90/1536000] via 10.20.2.5, 01:03:51, Ethernet0/1

```
```
R17
!
router eigrp named
 !
 address-family ipv4 unicast autonomous-system 2
  !
  af-interface Ethernet0/1
   summary-address 10.20.0.0 255.255.0.0        # R16-17 анонсируют только суммарные префиксы
  exit-af-interface
  !
  af-interface Ethernet0/0
   passive-interface                            # не шлем пакеты в сторону пользователей
  exit-af-interface
  !
  af-interface Ethernet0/2
   passive-interface                            # не шлем пакеты в сторону пользователей
  exit-af-interface
  !
  topology base
  exit-af-topology
  network 10.20.0.0 0.0.255.255                 # указываем сети для анонса
  eigrp router-id 17.17.17.17
 exit-address-family

R17# show ip route eigrp 2
Gateway of last resort is not set
      10.0.0.0/8 is variably subnetted, 10 subnets, 4 masks
D        10.20.0.0/16 is a summary, 01:04:21, Null0
D        10.20.0.18/32 [90/1024640] via 10.20.2.3, 01:03:48, Ethernet0/1
D        10.20.2.4/31 [90/1536000] via 10.20.2.3, 01:02:47, Ethernet0/1

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
  network 10.20.0.0 0.0.255.255                 # указываем сети для анонса
  eigrp router-id 18.18.18.18
 exit-address-family
 
R18# show ip route eigrp 2
Gateway of last resort is not set
      10.0.0.0/8 is variably subnetted, 10 subnets, 3 masks
D        10.20.0.0/16 [90/1024640] via 10.20.2.4, 01:02:52, Ethernet0/0
                      [90/1024640] via 10.20.2.2, 01:02:52, Ethernet0/1

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
 
R32# show ip route eigrp 2
Gateway of last resort is 10.20.2.6 to network 0.0.0.0
D*    0.0.0.0/0 [90/1024640] via 10.20.2.6, 01:01:58, Ethernet0/0
 
```