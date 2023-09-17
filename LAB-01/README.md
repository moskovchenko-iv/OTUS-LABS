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
   ```
3. 


Реализуемая схема:
![Реализуемая схема:](https://github.com/moskovchenko-iv/OTUS-LABS/blob/main/LAB-01/Screenshot_1.jpg)
