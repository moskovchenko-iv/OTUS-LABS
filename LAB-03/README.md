# Part 1: Build the Network and Configure Basic Device Settings

### Выполнение Ч1
1. Создаем адресный план
    ```
     Subnet_A - network 192.168.1.0 255.255.255.192 (Clients R1)
     Subnet_B - network 192.168.1.128 255.255.255.224 (Management)
     Subnet_C - network 192.168.1.208 255.255.255.240 (Clients R2)
    ```
2. Собираем топологию сети
![img.png](img.png)
3. Производим базовую настройку оборудования
   ```
   Current configuration : 1515 bytes
   !
   service password-encryption
   !
   hostname S1
   no logging console
   enable secret 5 $1$r75v$qOesv2PxA6c09Ak7XBfhl1
   !
   aaa new-model
   !
   !
   aaa authentication login default local
   aaa authorization exec default local
   !
   clock timezone MSK 3 0
   !
   no ip domain lookup
   !
   username cisco privilege 15 secret 5 $1$IE4e$CRkxPOAQmhp5JbkBmIuQl/
   !
   banner login ^CC
        *************************************************************
        **         All activity is subject to monitoring.          **
        **    Any UNAUTHORIZED access or use is PROHIBITED,        **
        **             and may result in PROSECUTION.              **
        **                         <<S1>>                          **
        *************************************************************
   ^C
   !
   line con 0
   logging synchronous
   line aux 0
   line vty 0 4
   transport input all
   ```
4. Настраиваем маршрутизацию и интерфейсы на R1
    ```
    interface Ethernet0/0
     ip address 10.0.0.1 255.255.255.252
    !
    interface Ethernet0/1.100
     description CLIENTS
     encapsulation dot1Q 100
     ip address 192.168.1.1 255.255.255.192
    !
    interface Ethernet0/1.200
     description MANAGEMENT
     encapsulation dot1Q 200
     ip address 192.168.1.129 255.255.255.224
    !
    ip route 0.0.0.0 0.0.0.0 10.0.0.2
    ```
5. Настраиваем маршрутизацию и интерфейсы на R1
    ```
    interface Ethernet0/0
     ip address 10.0.0.2 255.255.255.252
    !
    interface Ethernet0/1
     ip address 192.168.1.209 255.255.255.240
     ip helper-address 10.0.0.1
    !
    ip route 0.0.0.0 0.0.0.0 10.0.0.1
    ```
6. Производим базовую настройку свичей
    ```
    Настройки аналогичные п3
   ```
7. Создаем VLANs на S1
    ```
       VLAN Name                             Status    Ports
    ---- -------------------------------- --------- -------------------------------
    1    default                          active
    100  CLIENTS                          active    Et0/0
    200  MANAGEMENT                       active
    999  PARKING_LOT                      active    Et0/1, Et0/2
    1000 NATIVE                           active
    ```
8. Назначаем VLAN на интерфейсы коммутатора, назначаем сети
    ```
    interface Ethernet0/0
     description PC-A
     switchport access vlan 100
     switchport mode access
     load-interval 60
    !
    interface Ethernet0/1
     description PARKING_LOT
     switchport access vlan 999
     switchport mode access
     shutdown
    !
    interface Ethernet0/2
     description PARKING_LOT
     switchport access vlan 999
     switchport mode access
     shutdown
    !
    interface Ethernet0/3
     description R1
     switchport trunk allowed vlan 100,200,1000
     switchport trunk encapsulation dot1q
     switchport trunk native vlan 1000
     switchport mode trunk
     load-interval 60
    !   
    interface Vlan200
     ip address 192.168.1.130 255.255.255.224
    !
    ```
### Выполнение Ч2
