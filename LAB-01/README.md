# VLAN и маршрутизация между VLAN 

### Выполнение
1. Воссоздаем схему и коммутацию, реализуемая схема ниже.
2. Производим базовую настройку оборудования.
   ```
   Current configuration : 1515 bytes
   !
   service password-encryption
   !
   hostname R1
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
        **                         <<R1>>                          **
        *************************************************************
   ^C
   !
   line con 0
   logging synchronous
   line aux 0
   line vty 0 4
   transport input all
   ```
3. 


Реализуемая схема:
![Реализуемая схема:](https://github.com/moskovchenko-iv/OTUS-LABS/blob/main/LAB-01/Screenshot_1.jpg)
