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