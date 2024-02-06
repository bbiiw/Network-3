# LAN Card

(Network Interface Card - NIC) เป็นอุปกรณ์ที่เชื่อมต่อเครือข่ายคอมพิวเตอร์กับเครือข่าย LAN เพื่อให้สามารถสื่อสารกับเครื่องคอมพิวเตอร์อื่น ๆ ในเครือข่าย

## หลักการทำงาน 

### ตรวจสอบสถานะ

ใช้คำสั่ง `lspci` ใน Linux เพื่อแสดงรายการของ network card พิมพ์คำสั่ง `lspci` ตามด้วยคำสั่ง `egrep` เพื่อกรองอุปกรณ์ที่เกี่ยวข้อง :

```
lspci | egrep -i --color 'network|ethernet'
lspci | egrep -i --color 'network|ethernet|wireless|wi-fi'
```

ผลลัพธ์ที่ได้

```
09:00.0 Ethernet controller: Broadcom Corporation NetXtreme BCM5761e Gigabit Ethernet PCIe (rev 10)
0c:00.0 Network controller: Intel Corporation Ultimate N WiFi Link 5300
```

---

คำสั่ง `lshw` สามารถดึงข้อมูลรายละเอียดเกี่ยวกับการกำหนดค่า hardware ของเครื่องได้ รวมถึงข้อมูลเกี่ยวกับ network card :

```
lshw -class network
```
ผลลัพธ์แสดงรายละเอียดข้อมูล hardware ที่เกี่ยวกับ network card บน Linux :
```
  *-network DISABLED      
       description: Wireless interface
       product: Ultimate N WiFi Link 5300
       vendor: Intel Corporation
       physical id: 0
       bus info: pci@0000:0c:00.0
       logical name: wlan0
       version: 00
       serial: 00:21:6a:ca:9b:10
       width: 64 bits
       clock: 33MHz
       capabilities: pm msi pciexpress bus_master cap_list ethernet physical wireless
       configuration: broadcast=yes driver=iwlwifi driverversion=3.2.0-0.bpo.1-amd64 firmware=8.83.5.1 build 33692 latency=0 link=no multicast=yes wireless=IEEE 802.11abgn
       resources: irq:46 memory:f1ffe000-f1ffffff
  *-network
       description: Ethernet interface
       product: NetXtreme BCM5761e Gigabit Ethernet PCIe
       vendor: Broadcom Corporation
       physical id: 0
       bus info: pci@0000:09:00.0
       logical name: eth0
       version: 10
       serial: b8:ac:6f:65:31:e5
       size: 1GB/s
       capacity: 1GB/s
       width: 64 bits
       clock: 33MHz
       capabilities: pm vpd msi pciexpress bus_master cap_list ethernet physical tp 10bt 10bt-fd 100bt 100bt-fd 1000bt 1000bt-fd autonegotiation
       configuration: autonegotiation=on broadcast=yes driver=tg3 driverversion=3.121 duplex=full firmware=5761e-v3.71 ip=192.168.1.5 latency=0 link=yes multicast=yes port=twisted pair speed=1GB/s
       resources: irq:48 memory:f1be0000-f1beffff memory:f1bf0000-f1bfffff
```

แสดงผลลัพธ์แบบสั้น ใช้คำสั่ง
`sudo lshw -class network -short`
```
[sudo] password for vivek:
H/W path           Device        Class          Description
===========================================================
/0/100/1d.6/0      wlp82s0       network        Wi-Fi 6 AX200
/0/100/1f.6        eth0          network        Ethernet Connection (7) I219-LM
```

> [!NOTE] ข้อกำหนด
> - โดยปกติแล้วคำสั่ง `lshw` อาจจะยังไม่ได้ถูกติดตั้งในระบบ ใช้คำสั่ง
>   - `sudo apk add lshw` บน Alpine Linux
>   - `sudo dnf install lshw` หรือ `sudo yum install lshw` บน RHEL
>   - `sudo apt install lshw` หรือ `sudo apt-get install lshw` บน Debian Ubuntu
>   - `sudo zypper install lshw` บน SUSE
>   - `sudo pacman -S lshw` บน Arch Linux 
>   - เพื่อติดตั้ง `lshw`





## 📚 Reference

- https://www.cyberciti.biz/faq/linux-list-network-cards-command/
- https://www.baeldung.com/linux/list-network-cards
- https://www.baeldung.com/linux/network-interface-configure
- https://amod-kadam.medium.com/linux-network-interface-s-minimal-know-how-helped-me-a-lot-168547b471d9






<!-- การทำงานของ LAN Card ต้องตรวจสอบสถานะและข้อมูลของ LAN Card ที่เชื่อมต่อกับระบบ ซึ่งสามารถทำได้โดยใช้คำสั่ง `ifconfig` หรือ `ip` ซึ่งเป็นเครื่องมือสำหรับการดูแลและติดตั้ง network interface ในระบบ Linux

```
ip addr
```

![alt text](https://media.geeksforgeeks.org/wp-content/uploads/20240126214338/1-min.png)

จะแสดงรายการ network interface พร้อมรายละเอียดที่เกี่ยวข้อง เช่น ชื่ออินเตอร์เฟซ สถานะและที่อยู่ฮาร์ดแวร์ คำสั่ง `ip` ให้ความสามารถในการกำหนดค่าและดูข้อมูลการกำหนดค่าเครือข่ายอย่างละเอียดในระบบ Linux -->

