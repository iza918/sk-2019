Zadanie 1
---------

![zadanie 1](zadanie-1.svg)

1. Zaprojektuj oraz przygotuj prototyp rozwiązania z wykorzystaniem oprogramowania ``VirtualBox`` lub podobnego. 
Zaproponuj rozwiązanie spełniające poniższe wymagania:
   * Usługodawca zapewnia domunikację z siecią internet poprzez interfejs ``eth0`` ``PC0``
   * Zapewnij komunikację z siecią internet na poziomie ``LAN1`` oraz ``LAN2``
   * Dokonaj takiego podziału sieci o adresie ``172.22.128.0/17`` aby w ``LAN1`` można było zaadresować ``500`` adresów natomiast w LAN2 ``5000`` adresów    
   * Przygotuj dokumentację powyższej architektury w formie graficznej w programie ``DIA``
 
    
Ustawienia adresów:
---------

Sieci
---------
``LAN1`` - 172.22.128.0/23  
``LAN2`` - 172.22.160.0/19  

PC0                                          
---------                                       
``eth0`` - domyślny  
``eth1`` - 172.22.128.1/23  (ip addr add 172.22.128.1/23 dev enp0s8)
``eth2`` - 172.22.160.1/19  (ip addr add 172.22.160.1/19 dev enp0s9)
ip link set ``nazwa urządzenia`` up - "podniesienie" urządzenia (po dodaniu PC1 i PC2)
 
PC1 
---------
``eth0`` - 172.22.128.2/23  (ip addr add 172.22.128.2/23 dev enp0s3)
 
PC2
---------
``eth0`` - 172.22.160.2/19  (ip addr add 172.22.160.2/19 dev enp0s3)


Ustawienia routingu:
---------

PC1
---------
ip route add default via 172.22.128.1/23 

PC2
---------
ip route add default via 172.22.160.1/19


Ustawienia forwardowania pakietów na PC0:
---------
echo 1 > /proc/sys/net/ipv4/ip_forward (komunikacja PC1 <-> PC2)


Ustawienia reguły masquerade w PC0:
---------
iptables -t nat -A POSTROUTING -s 172.22.128.0/23 -o enp0s3 -j MASQUERADE
iptables -t nat -A POSTROUTING -s 172.22.160.0/19 -o enp0s3 -j MASQUERADE
(udostępnienie internetu dla podsieci)
