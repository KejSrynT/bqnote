RIP



Router(config)#router rip

Router(config-router)#version 2

Router(config-router)#no auto-summary

Yonetim(config)#router rip
Yonetim(config-router)#redistribute rip metric 1


Nat 


Router# configure terminal

! İç ağa bakan arayüzü tanımla
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside
Router(config-if)# exit

! Dış ağa (İnternete) bakan arayüzü tanımla
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside
Router(config-if)# exit


Statik nat

Router(config)# ip nat inside source static 192.168.1.10 203.0.113.10

dinamik nat

Router(config)# ip nat pool YAZILIM_HAVUZ 203.0.113.50 203.0.113.60 netmask255.255.255.0
Router(config)# access-list 10 permit 192.168.2.0 0.0.0.255
Router(config)# ip nat inside source list 10 pool YAZILIM_HAVUZ

PAT
Router(config)# ip nat pool TEK_IP_HAVUZU 203.0.113.50 203.0.113.50 netmask 255.255.255.0 !
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255 
Router(config)# ip nat inside source list 1 pool TEK_IP_HAVUZU overload


OSPF

OSPF YAPILANDIRMASI

  
Ege(config)#router ospf 1
Ege(config-router)#network 85.0.0.0 0.255.255.255 area 0
Ege(config-router)#network 87.0.0.0 0.255.255.255 area 0
Ege(config-router)#network 192.168.10.0 0.0.0.127 area 0

Ege(config)#router ospf 1
Ege(config-router)#passive-interface GigabitEthernet 0/0
Ege(config-router)#auto-cost reference-bandwidth 1000

Ege(config)#interface Serial 0/3/1
Ege(config-if)#bandwidth 5000
veri hızını etkilemez sadece maliyet hesabını manüpüle eder
Ege>sh ip route
Ege>show ip ospf neighbor


##### **OSPF KİMLİK DOĞRULAMA**
Yönlendirici(config)#router ospf 1
Yönlendirici(config-router)#area 0 authentication message-digest
Yönlendirici(config)#interface serial0/0/0
Yönlendirici(config-if)# ip ospf message-digest-key 1 md5 “parola




EIGRP 
Afrika(config)#router eigrp 10
Afrika(config-router)#network 20.0.0.0 0.255.255.255
Afrika(config-router)#network 40.0.0.0 0.255.255.255
Afrika(config-router)#network 192.68.1.0 0.0.0.255
!! no auto summary

Afrika(config)#key chain Anahtar1
Afrika(config-keychain)#key 1
Afrika(config-keychain-key)#key-string M23041920K

Afrika(config)#interface GigabitEthernet 0/1
Afrika(config-if)#ip authentication mode eigrp 10 md5
Afrika(config-if)#ip authentication key-chain eigrp 10 Anahtar1


Asya(config)#ip route 0.0.0.0 0.0.0.0 Serial0/1/0
Asya(config)#router eigrp 10
Asya(config-router)#redistribute static

BGP
Yonlendirici(config)#router bgp ‘Otonom Sistem Numarası’
Yonlendirici(config-router)#neighbor ‘Komşu Yönlendirici IP adresi’ remote-as ‘Komşu Otonom Sis-
tem Numarası’
Yonlendirici(config-router)#network ‘Arayüz Ağ Adresi’ mask ‘Ağ Alt Ağ Maskesi’


PPP
Ankara(config)#username kullanıcıadı password parola
Ankara(config-if)#ppp authentication pap
RT1(config-if)#ppp pap sent-username mtt password mtt

Ankara(config)#interface serial 0/0/0
Ankara(config-if)#encapsulation ppp
2x


Standart ACL
Ankara(config)#access-list 1 permit 29.10.19.24
Ankara(config)#access-list 1 deny 29.10.19.0 0.0.0.255
Ankara(config)#access-list 1 permit 19.5.19.20
Ankara(config)#interface serial 0/0/0
Ankara(config-if)#ip access-group 1 in

Extented ACL
Samsun(config)#access-list 110 permit tcp host 29.10.19.24 host 192.168.1.254 eq 21
Samsun(config)#access-list 110 deny tcp 29.10.19.0 0.0.0.255 host 192.168.1.254 eq ftp
Samsun(config)#access-list 110 permit tcp 29.10.19.0 0.0.0.255 host 192.168.1.254 eq 80
Samsun (config)#interface gi0/1
Samsun(config-if)#ip access-group 110 in

