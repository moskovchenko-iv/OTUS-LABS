# Избыточность локальных сетей. STP 

### Выполнение
1. Воссоздаем схему и коммутацию, схема реализации п.7
2. Производим базовую настройку оборудования
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
3. Настраиваем интерфейсы S1, S2, S3
    ```
    interface Vlan1
    description MANAGEMENT
    ip address 192.168.1.1 255.255.255.0
    end
   
    interface Vlan1
    description MANAGEMENT
    ip address 192.168.1.2 255.255.255.0
    end

    interface Vlan1
    description MANAGEMENT
    ip address 192.168.1.3 255.255.255.0
    end
   ```
4. Проверяем связь 
    ```
    S1#ping 192.168.1.2
    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 192.168.1.2, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
    
    S1#ping 192.168.1.3
    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms
   
    S2#ping 192.168.1.3
    Type escape sequence to abort.
    Sending 5, 100-byte ICMP Echos to 192.168.1.3, timeout is 2 seconds:
    !!!!!
    Success rate is 100 percent (5/5), round-trip min/avg/max = 1/1/1 ms

```