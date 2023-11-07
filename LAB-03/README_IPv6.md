# Part 1: Build the Network and Configure Basic Device Settings

### Выполнение Ч1
1. Собираем топологию сети
![img_1.png](img_1.png)
2. Производим базовую настройку свичей
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
3. Производим базовую настройку роутеров
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
4. Настраиваем интерфейсы на R1 и R2
    ```
    R1#sh run int e0/0
    !
    interface Ethernet0/0
     no ip address
     ipv6 address FE80::1 link-local
     ipv6 address 2001:DB8:ACAD:2::1/64
     ipv6 enable
    end
    
    R1#sh run int e0/1
    !
    interface Ethernet0/1
     no ip address
     ipv6 address FE80::1 link-local
     ipv6 address 2001:DB8:ACAD:1::1/64
     ipv6 enable
    end
    
    R2#sh run int e0/0
    !
    interface Ethernet0/0
     no ip address
     ipv6 address FE80::2 link-local
     ipv6 address 2001:DB8:ACAD:2::2/64
     ipv6 enable
    end
    
    R2#sh run int e0/1
    !
    interface Ethernet0/1
     no ip address
     ipv6 address FE80::1 link-local
     ipv6 address 2001:DB8:ACAD:3::1/64
     ipv6 enable
    end
   
    default route R1 - ipv6 route ::/0 2001:DB8:ACAD:2::2
    default route R2 - ipv6 route ::/0 2001:DB8:ACAD:2::1
   
    R2#ping  2001:DB8:ACAD:1::1
    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 2001:DB8:ACAD:1::1, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
    ```
