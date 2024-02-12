# Основные протоколы сети интернет

### Выполнение

Лаботаторная схема сети
![img.png](img.png)

1. Настроим NAT(PAT) на R14 и R15. Трансляция осуществляться в адрес автономной системы AS1001.
   ```
   Настройки на R14:
   
   ip nat pool POOL-123.14.14.2 123.14.14.2 123.14.14.2 netmask 255.255.255.0
   ip nat inside source list 10 pool POOL-123.14.14.2 overload
   !
   access-list 10 permit 10.10.101.0 0.0.0.255
   access-list 10 permit 10.10.102.0 0.0.0.255
   !
   ip nat outside на uplink 
   ip nat inside на downlinks
   ``` 
   ```
   Настройки на R15:

   ip nat pool POOL-123.15.15.2 123.15.15.2 123.15.15.2 netmask 255.255.255.0
   ip nat inside source list 10 pool POOL-123.15.15.2 overload
   !
   access-list 10 permit 10.10.101.0 0.0.0.255
   access-list 10 permit 10.10.102.0 0.0.0.255

   ip nat outside на uplink 
   ip nat inside на downlinks
   ```
2. Настроим NAT(PAT) на R18. Трансляция осуществляться в пул из 5 адресов автономной системы AS2042.
   