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
   
   Маршруты на R14:
   
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
   ```
   Настройки на R15:
   
   router ospf 1
   router-id 15.15.15.15
   area 102 filter-list prefix FILTER_IPV4-AREA_101 in
   !
   ipv6 router ospf 1
   router-id 15.15.15.15
   area 102 filter-list prefix FILTER_IPV6-AREA_101 in
   !
   ip prefix-list FILTER_IPV4-AREA_101 seq 5 deny 10.10.0.19/32
   ip prefix-list FILTER_IPV4-AREA_101 seq 10 deny 10.10.2.2/31
   ip prefix-list FILTER_IPV4-AREA_101 seq 15 permit 0.0.0.0/0 le 32
   !
   ipv6 prefix-list FILTER_IPV6-AREA_101 seq 5 deny FD00::10:10:0:19/128
   ipv6 prefix-list FILTER_IPV6-AREA_101 seq 10 deny FD00::10:10:2:2/127
   ipv6 prefix-list FILTER_IPV6-AREA_101 seq 15 permit ::/0 le 128
   
   Маршруты на R14:
   
   R15# show ip route ospf
   O*E2  0.0.0.0/0 [110/1] via 10.10.2.10, 04:12:54, Ethernet0/0
                   [110/1] via 10.10.2.8, 04:12:54, Ethernet0/1
   O IA     10.10.0.4/32 [110/21] via 10.10.2.10, 04:12:54, Ethernet0/0
                         [110/21] via 10.10.2.8, 04:13:04, Ethernet0/1
   O IA     10.10.0.5/32 [110/21] via 10.10.2.10, 04:12:54, Ethernet0/0
                         [110/21] via 10.10.2.8, 04:13:04, Ethernet0/1
   O        10.10.0.13/32 [110/11] via 10.10.2.10, 04:12:54, Ethernet0/0
   O        10.10.0.14/32 [110/21] via 10.10.2.10, 04:12:54, Ethernet0/0
                          [110/21] via 10.10.2.8, 04:12:54, Ethernet0/1
   O IA     10.10.0.19/32 [110/31] via 10.10.2.10, 04:12:54, Ethernet0/0
                          [110/31] via 10.10.2.8, 04:12:54, Ethernet0/1
   O        10.10.0.20/32 [110/11] via 10.10.2.12, 04:13:04, Ethernet0/3
   O IA     10.10.2.2/31 [110/30] via 10.10.2.10, 04:12:54, Ethernet0/0
                         [110/30] via 10.10.2.8, 04:12:54, Ethernet0/1
   O        10.10.2.4/31 [110/20] via 10.10.2.8, 04:12:54, Ethernet0/1
   O        10.10.2.6/31 [110/20] via 10.10.2.10, 04:12:54, Ethernet0/0
   O IA     10.10.2.14/31 [110/20] via 10.10.2.8, 04:13:04, Ethernet0/1
   O IA     10.10.2.16/31 [110/20] via 10.10.2.8, 04:13:04, Ethernet0/1
   O IA     10.10.2.18/31 [110/20] via 10.10.2.10, 04:12:54, Ethernet0/0
   O IA     10.10.2.20/31 [110/20] via 10.10.2.10, 04:12:54, Ethernet0/0
   O IA     10.10.101.0/24 [110/30] via 10.10.2.10, 04:12:54, Ethernet0/0
                           [110/30] via 10.10.2.8, 04:13:04, Ethernet0/1
   O IA     10.10.102.0/24 [110/30] via 10.10.2.10, 04:12:54, Ethernet0/0
                           [110/30] via 10.10.2.8, 04:13:04, Ethernet0/1
   
   R15# show ipv6 route ospf
   OE2 ::/0 [110/1], tag 1 via FE80::2, Ethernet0/1
                           via FE80::2, Ethernet0/0
   OI  FD00::10:10:0:4/128 [110/20] via FE80::2, Ethernet0/1
                                    via FE80::2, Ethernet0/0
   OI  FD00::10:10:0:5/128 [110/20] via FE80::2, Ethernet0/1
                                    via FE80::2, Ethernet0/0
   O   FD00::10:10:0:13/128 [110/10] via FE80::2, Ethernet0/0
   O   FD00::10:10:0:14/128 [110/20] via FE80::2, Ethernet0/0
                                     via FE80::2, Ethernet0/1
   OI  FD00::10:10:0:19/128 [110/30] via FE80::2, Ethernet0/1
                                     via FE80::2, Ethernet0/0
   O   FD00::10:10:0:20/128 [110/10] via FE80::2, Ethernet0/3
   OI  FD00::10:10:2:2/127 [110/30] via FE80::2, Ethernet0/1
                                    via FE80::2, Ethernet0/0
   O   FD00::10:10:2:4/127 [110/20] via FE80::2, Ethernet0/1
   O   FD00::10:10:2:6/127 [110/20] via FE80::2, Ethernet0/0
   OI  FD00::10:10:2:14/127 [110/20] via FE80::2, Ethernet0/1
   OI  FD00::10:10:2:16/127 [110/20] via FE80::2, Ethernet0/1
   OI  FD00::10:10:2:18/127 [110/20] via FE80::2, Ethernet0/0
   OI  FD00::10:10:2:20/127 [110/20] via FE80::2, Ethernet0/0
   OI  FD00::10:10:101:0/112 [110/30] via FE80::2, Ethernet0/1
                                      via FE80::2, Ethernet0/0
   OI  FD00::10:10:102:0/112 [110/30] via FE80::2, Ethernet0/1
                                      via FE80::2, Ethernet0/0
   ```
   ```
   Настройки на R19:

   router ospf 1
   router-id 19.19.19.19
   area 101 stub
   !
   ipv6 router ospf 1
   router-id 19.19.19.19
   area 101 stub

   Маршруты на R19:
   
   R19# show ip route ospf
   O*IA  0.0.0.0/0 [110/11] via 10.10.2.3, 04:28:32, Ethernet0/0

   R19# show ipv6 route ospf
   OI  ::/0 [110/11] via FE80::1, Ethernet0/0
   ```
   ```
   Настройки на R20
   
   router ospf 1
   router-id 20.20.20.20
   !
   ipv6 router ospf 1
   router-id 20.20.20.20

   Маршруты на R20:
   
   R20# show ip route ospf
   O*E2  0.0.0.0/0 [110/1] via 10.10.2.13, 04:55:45, Ethernet0/0
   O IA     10.10.0.4/32 [110/31] via 10.10.2.13, 04:55:59, Ethernet0/0
   O IA     10.10.0.5/32 [110/31] via 10.10.2.13, 04:55:59, Ethernet0/0
   O IA     10.10.0.13/32 [110/21] via 10.10.2.13, 04:55:50, Ethernet0/0
   O IA     10.10.0.14/32 [110/31] via 10.10.2.13, 04:55:50, Ethernet0/0
   O IA     10.10.0.15/32 [110/11] via 10.10.2.13, 04:55:59, Ethernet0/0
   O IA     10.10.2.4/31 [110/30] via 10.10.2.13, 04:55:59, Ethernet0/0
   O IA     10.10.2.6/31 [110/30] via 10.10.2.13, 04:55:50, Ethernet0/0
   O IA     10.10.2.8/31 [110/20] via 10.10.2.13, 04:55:59, Ethernet0/0
   O IA     10.10.2.10/31 [110/20] via 10.10.2.13, 04:55:59, Ethernet0/0
   O IA     10.10.2.14/31 [110/30] via 10.10.2.13, 04:55:59, Ethernet0/0
   O IA     10.10.2.16/31 [110/30] via 10.10.2.13, 04:55:59, Ethernet0/0
   O IA     10.10.2.18/31 [110/30] via 10.10.2.13, 04:55:50, Ethernet0/0
   O IA     10.10.2.20/31 [110/30] via 10.10.2.13, 04:55:50, Ethernet0/0
   O IA     10.10.101.0/24 [110/40] via 10.10.2.13, 04:55:59, Ethernet0/0
   O IA     10.10.102.0/24 [110/40] via 10.10.2.13, 04:55:59, Ethernet0/0

   R20# show ipv6 route ospf
   OE2 ::/0 [110/1], tag 1 via FE80::1, Ethernet0/0
   OI  FD00::A0A:F/128 [110/10] via FE80::1, Ethernet0/0
   OI  FD00::10:10:0:4/128 [110/30] via FE80::1, Ethernet0/0
   OI  FD00::10:10:0:5/128 [110/30] via FE80::1, Ethernet0/0
   OI  FD00::10:10:0:13/128 [110/20] via FE80::1, Ethernet0/0
   OI  FD00::10:10:0:14/128 [110/30] via FE80::1, Ethernet0/0
   OI  FD00::10:10:2:4/127 [110/30] via FE80::1, Ethernet0/0
   OI  FD00::10:10:2:6/127 [110/30] via FE80::1, Ethernet0/0
   OI  FD00::10:10:2:8/127 [110/20] via FE80::1, Ethernet0/0
   OI  FD00::10:10:2:10/127 [110/20] via FE80::1, Ethernet0/0
   OI  FD00::10:10:2:14/127 [110/30] via FE80::1, Ethernet0/0
   OI  FD00::10:10:2:16/127 [110/30] via FE80::1, Ethernet0/0
   OI  FD00::10:10:2:18/127 [110/30] via FE80::1, Ethernet0/0
   OI  FD00::10:10:2:20/127 [110/30] via FE80::1, Ethernet0/0
   OI  FD00::10:10:101:0/112 [110/40] via FE80::1, Ethernet0/0
   OI  FD00::10:10:102:0/112 [110/40] via FE80::1, Ethernet0/0
   ```