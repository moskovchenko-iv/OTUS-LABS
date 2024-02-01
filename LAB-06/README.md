# OSPF.

### Выполнение

Лаботаторная схема сети
![img.png](img.png)

1. Отобразим на схеме пункты задания относительно принадлежности зон, а также укажем типы Area
2. Отобразим на роутерах пункты задания относительно принадлежности зон, а также укажем типы Area
   ```
   Настройки на R14:
   router ospf 1
   router-id 14.14.14.14
   area 101 stub no-summary
   default-information originate
   !
   ip route 0.0.0.0 0.0.0.0 Null0
   !
   ipv6 route ::/0 Null0
   !
   ipv6 router ospf 1
   router-id 14.14.14.14
   area 101 stub no-summary
   default-information originate
   
   R14# show ip route ospf
      10.0.0.0/8 is variably subnetted, 24 subnets, 3 masks
   O IA     10.10.0.4/32 [110/21] via 10.10.2.6, 03:12:47, Ethernet0/1
                         [110/21] via 10.10.2.4, 03:12:47, Ethernet0/0
   O IA     10.10.0.5/32 [110/21] via 10.10.2.6, 03:12:47, Ethernet0/1
                         [110/21] via 10.10.2.4, 03:12:47, Ethernet0/0
   O        10.10.0.13/32 [110/11] via 10.10.2.6, 03:12:47, Ethernet0/1
   O        10.10.0.15/32 [110/21] via 10.10.2.6, 03:12:47, Ethernet0/1
                          [110/21] via 10.10.2.4, 03:12:47, Ethernet0/0
   O        10.10.0.19/32 [110/11] via 10.10.2.2, 03:12:57, Ethernet0/3
   O IA     10.10.0.20/32 [110/31] via 10.10.2.6, 03:12:47, Ethernet0/1
                          [110/31] via 10.10.2.4, 03:12:47, Ethernet0/0
   O        10.10.2.8/31 [110/20] via 10.10.2.4, 03:12:47, Ethernet0/0
   O        10.10.2.10/31 [110/20] via 10.10.2.6, 03:12:47, Ethernet0/1
   O IA     10.10.2.12/31 [110/30] via 10.10.2.6, 03:12:47, Ethernet0/1
                          [110/30] via 10.10.2.4, 03:12:47, Ethernet0/0
   O IA     10.10.2.14/31 [110/20] via 10.10.2.4, 03:12:47, Ethernet0/0
   O IA     10.10.2.16/31 [110/20] via 10.10.2.4, 03:12:47, Ethernet0/0
   O IA     10.10.2.18/31 [110/20] via 10.10.2.6, 03:12:47, Ethernet0/1
   O IA     10.10.2.20/31 [110/20] via 10.10.2.6, 03:12:47, Ethernet0/1
   O IA     10.10.101.0/24 [110/30] via 10.10.2.6, 03:12:47, Ethernet0/1
                           [110/30] via 10.10.2.4, 03:12:47, Ethernet0/0
   O IA     10.10.102.0/24 [110/30] via 10.10.2.6, 03:12:47, Ethernet0/1
                           [110/30] via 10.10.2.4, 03:12:47, Ethernet0/0
   
   R14# show ipv6 route ospf
   O   FD00::A0A:F/128 [110/20] via FE80::2, Ethernet0/0
                                via FE80::2, Ethernet0/1
   OI  FD00::10:10:0:4/128 [110/20] via FE80::2, Ethernet0/1
                                    via FE80::2, Ethernet0/0
   OI  FD00::10:10:0:5/128 [110/20] via FE80::2, Ethernet0/1
                                    via FE80::2, Ethernet0/0
   O   FD00::10:10:0:13/128 [110/10] via FE80::2, Ethernet0/1
   O   FD00::10:10:0:19/128 [110/10] via FE80::2, Ethernet0/3
   OI  FD00::10:10:0:20/128 [110/30] via FE80::2, Ethernet0/1
                                     via FE80::2, Ethernet0/0
   O   FD00::10:10:2:8/127 [110/20] via FE80::2, Ethernet0/0
   O   FD00::10:10:2:10/127 [110/20] via FE80::2, Ethernet0/1
   OI  FD00::10:10:2:12/127 [110/30] via FE80::2, Ethernet0/1
                                     via FE80::2, Ethernet0/0
   OI  FD00::10:10:2:14/127 [110/20] via FE80::2, Ethernet0/0
   OI  FD00::10:10:2:16/127 [110/20] via FE80::2, Ethernet0/0
   OI  FD00::10:10:2:18/127 [110/20] via FE80::2, Ethernet0/1
   OI  FD00::10:10:2:20/127 [110/20] via FE80::2, Ethernet0/1
   OI  FD00::10:10:101:0/112 [110/30] via FE80::2, Ethernet0/1
                                      via FE80::2, Ethernet0/0
   OI  FD00::10:10:102:0/112 [110/30] via FE80::2, Ethernet0/1
                                      via FE80::2, Ethernet0/0
   ```