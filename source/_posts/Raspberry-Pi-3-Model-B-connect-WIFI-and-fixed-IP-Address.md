---
title: Raspberry Pi 3 Model B connect WIFI and fixed IP Address
date: 2018-06-30 12:04:05
tags: Raspberry Pi
---

#### 连接 WIFI：

`iwlist scan` 或 `iwlist wlan0 scan` 扫描可用的网络，前者回扫描全部的网络，后者只扫描无线网络：

```
wlan0     Scan completed :
          Cell 01 - Address: 50:BD:5F:FA:DB:E4
                    Channel:11
                    Frequency:2.462 GHz (Channel 11)
                    Quality=51/70  Signal level=-59 dBm  
                    Encryption key:on
                    ESSID:"pno"
                    Bit Rates:1 Mb/s; 2 Mb/s; 5.5 Mb/s; 11 Mb/s; 6 Mb/s
                              9 Mb/s; 12 Mb/s; 18 Mb/s
                    Bit Rates:24 Mb/s; 36 Mb/s; 48 Mb/s; 54 Mb/s
                    Mode:Master
                    Extra:tsf=0000000000000000
                    Extra: Last beacon: 36650ms ago
                    IE: Unknown: 0003706E6F
                    IE: Unknown: 010882848B960C121824
                    IE: Unknown: 03010B
                    IE: Unknown: 050700010000000000
                    IE: Unknown: 2A0100
                    IE: Unknown: 32043048606C
                    IE: Unknown: 2D1A6E1003FF00000000000000000000000000000000000000000000
                    IE: Unknown: 3D160B070200000000000000000000000000000000000000
                    IE: IEEE 802.11i/WPA2 Version 1
                        Group Cipher : CCMP
                        Pairwise Ciphers (1) : CCMP
                        Authentication Suites (1) : PSK
                    IE: WPA Version 1
                        Group Cipher : CCMP
                        Pairwise Ciphers (1) : CCMP
                        Authentication Suites (1) : PSK
                    IE: Unknown: DD180050F2020101000003A4000027A4000042435E0062322F00
                    IE: Unknown: DD05000AEB0100
                    IE: Unknown: DD310050F204104A0001101044000102104700100000000000001000000050BD5FFADBE4103C0001011049000600372A000120


```

查看要连接的 WIFI 信息，其中 `ESSID` 就是网络名称；

`sudo vim /etc/wpa_supplicant/wpa_supplicant.conf` 编辑该文件，在最后追加：

```
network={
    ssid="your wifi name"
    psk="your wifi password"
}
```

#### 固定 IP :

`sudo vim /etc/dhcpcd.conf` 编辑该文件，在最后追加：

```
interface wlan0
static ip_address=192.168.1.200/24
static routers=192.168.1.1
static domain_name_servers=114.114.114.114
```

重启 

finish