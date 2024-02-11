# Wireless Adapter

`Wireless Adapter`
หรือบางครั้งก็เรียกว่า Wi-Fi Adapter หมายถึงอุปกรณ์ที่ใช้สำหรับเชื่อมต่อกับเครือข่ายคอมพิวเตอร์แบบไร้สาย ซึ่งเป็นอุปกรณ์ภายนอกที่สามารถเสียบทาง Port USB ของเครื่องคอมพิวเตอร์ได้ 

อุปกรณ์ชนิดนี้จำเป็นจะต้องติดตั้ง Driver ด้วยจึงจะสามารถใช้งานได้ ส่วนใหญ่จะนำมาติดตั้งกับเครื่องคอมพิวเตอร์ส่วนบุคคล ที่ต้องการใช้เครือข่ายไร้สาย เนื่องจากบน Mainboard ของคอมพิวเตอร์นั้นจะไม่ได้มีอุปกรณ์สำหรับรับสัญญาณ Wi-Fi ติดตั้งมาด้วย 

บทบาทหลักของ Wireless adapter คือให้สามารถเชื่อมต่อกับเครือข่ายไร้สาย, การสื่อสารแบบไร้สาย, และการรับ-ส่งข้อมูลผ่านสัญญาณไร้สาย

![](https://m.media-amazon.com/images/I/81CsUmlJ+iL.jpg)
Source : https://www.amazon.com/Panda-Wireless-PAU06-300Mbps-Adapter/dp/B00JDVRCI0

## หลักการทำงานของ Wireless Adapter

### การกำหนดค่าเครือข่ายไร้สาย มีขั้นตอนสองส่วน

- `ส่วนแรก` คือการระบุและตรวจสอบ Driver ที่ถูกติดตั้งถูกต้อง สำหรับอุปกรณ์ไร้สาย และการกำหนดค่าอินเตอร์เฟซ

- `ส่วนที่สอง` คือการเลือกวิธีการจัดการการเชื่อมต่อไร้สาย


## 1) ตรวจสอบสถานะ Driver
เพื่อตรวจสอบว่า Driver สำหรับการ์ดถูกโหลดหรือไม่ ให้ตรวจสอบผลลัพธ์จากคำสั่ง `lspci -k` หรือ `lsusb -v` ขึ้นอยู่กับว่าการ์ดเชื่อมต่อด้วย PCI(e) หรือ USB

```
lspci -k
```
```
06:00.0 Network controller: Intel Corporation WiFi Link 5100
	Subsystem: Intel Corporation WiFi Link 5100 AGN
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
```

> [!NOTE]
> - หากการ์ดเป็นอุปกรณ์ USB การรัน `dmesg | grep usbcore` ในฐานะ root ควรให้ผลลัพธ์เช่น `usbcore: registered new interface driver rtl8187` แสดงออกมา

___
<br>

นอกจากนี้ ตรวจสอบผลลัพธ์จากคำสั่ง `ip link` เพื่อดูว่ามี wireless interface ถูกสร้างขึ้นหรือไม่

โดยทั่วไปชื่อของ wireless network interfaces  จะขึ้นต้นด้วยตัวอักษร `wl`, เช่น `wlan0` หรือ `wlp2s0` จากนั้นให้เปิดใช้งานอินเตอร์เฟซด้วยคำสั่ง

```
ip link set interface up
```
ตัวอย่างเช่น ในกรณีที่ interface คือ wlan0, ให้ใช้คำสั่ง `ip link set wlan0 up`

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp0s31f6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP
    link/ether 00:50:56:c0:00:08 brd ff:ff:ff:ff:ff:ff
3: wlan0: <BROADCAST,MULTICAST> mtu 1500 qdisc noqueue state DOWN
    link/ether 00:1a:7d:da:71:11 brd ff:ff:ff:ff:ff:ff
```

___


ตรวจสอบข้อความของ kernel เพื่อดูว่า firmware ถูกโหลดหรือไม่ :

```
dmesg | grep firmware
```
```
[   7.148259] iwlwifi 0000:02:00.0: loaded firmware version 39.30.4.1 build 35138 op_mode iwldvm
```


หากไม่มีผลลัพธ์ที่เกี่ยวข้อง, ตรวจสอบข้อความสำหรับผลลัพธ์ทั้งหมดสำหรับโมดูลที่คุณระบุไว้ก่อนหน้านี้ (ในตัวอย่างนี้คือ `iwlwifi`) เพื่อระบุข้อความที่เกี่ยวข้องหรือปัญหาเพิ่มเติม:


```
# dmesg | grep iwlwifi
```
```
[   12.342694] iwlwifi 0000:02:00.0: irq 44 for MSI/MSI-X
[   12.353466] iwlwifi 0000:02:00.0: loaded firmware version 39.31.5.1 build 35138 op_mode iwldvm
[   12.430317] iwlwifi 0000:02:00.0: CONFIG_IWLWIFI_DEBUG disabled
...
[   12.430341] iwlwifi 0000:02:00.0: Detected Intel(R) Corporation WiFi Link 5100 AGN, REV=0x6B
```

## 2) รับข้อมูลสถานะของ interface
เพื่อตรวจสอบสถานะการเชื่อมต่อ ใช้คำสั่งต่อไปนี้ :

```
iw dev interface link
```

### เปิดใช้งาน interface

บางการ์ดต้องการให้อินเตอร์เฟซของ kernel ถูกเปิดใช้งานก่อนที่คุณจะสามารถใช้ iw หรือ wireless_tools ได้

```
ip link show interface
```
```
3: wlan0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state DOWN mode DORMANT group default qlen 1000
    link/ether 12:34:56:78:9a:bc brd ff:ff:ff:ff:ff:ff
```

| `iw` Command                      | `wireless_tools` Command        | คำอธิบาย                                     |
|----------------------------------|-----------------------------|---------------------------------------------|
| `iw dev wlan0 link`               | `iwconfig wlan0`            | ดึงข้อมูลสถานะการเชื่อมต่อ                                 |
| `iw dev wlan0 scan`               | `iwlist wlan0 scan`         | สแกนหาจุดเข้าถึงที่มีพร้อมใช้                          |
| `iw dev wlan0 set type ibss`      | `iwconfig wlan0 mode ad-hoc` | ตั้งค่าโหมดการทำงานเป็น ad-hoc                             |
| `iw dev wlan0 connect your_essid` | `iwconfig wlan0 essid your_essid` | Connecting to open network.                     |
| `iw dev wlan0 connect your_essid 2432` | `iwconfig wlan0 essid your_essid freq 2432M` | 	Connecting to open network specifying channel.     |

## 3) ค้นหา Access Points
ใช้คำสั่ง `iw dev interface scan | less` เพื่อดู Access Points ที่มีทั้งหมด

```
iw dev wlan0 scan | less
```
```
BSS 1/1 (on wlan0)
    SSID: MySSID
    BSSID: 00:1a:7d:da:71:11
    signal: -60 dBm
    frequency: 2.412 GHz
    capabilities: [WPA2-PSK-CCMP][ESS]

BSS 2/1 (on wlan0)
    SSID: AnotherSSID
    BSSID: 00:22:44:55:66:77
    signal: -70 dBm
    frequency: 5.180 GHz
    capabilities: [WPA2-PSK-CCMP][ESS]
```

> [!NOTE]
> - หากมีข้อความ `Interface does not support scanning` น่าจะเป็นเพราะคุณลืมติดตั้ง firmware ในบางกรณี ข้อความนี้ก็อาจปรากฏเมื่อไม่ได้รับอนุญาตให้รัน `iw` ในฐานะ root ด้วย

### ข้อความที่สำคัญที่ต้องตรวจสอบ Access Points
- `SSID` : ชื่อของเครือข่าย

- `Signal` : อัตรากำลังไร้สายในหน่วย dBm (ตั้งแต่ -100 ถึง 0)
  - ยิ่งค่าลบเข้าหาที่ 0 มากขึ้น สัญญาณก็ยิ่งดีขึ้นการสังเกตค่ากำลังที่รายงานในลิงก์ที่มีคุณภาพดีและลิงก์ที่มีคุณภาพไม่ดีควรจะให้ความเข้าใจเกี่ยวกับระยะทางแต่ละอัน.


- `Security`: แม้ไม่ได้เห็นโดยตรง สามารถตรวจสอบบรรทัดที่เริ่มต้นด้วย capability ถ้ามี Privacy เช่น `capability: ESS Privacy ShortSlotTime (0x0411)` แสดงว่าเครือข่ายนั้นๆ ได้รับการป้องกันรูปใดรูปแบบหนึ่ง
  - หากมีบล็อก `RSN` แสดงว่าเครือข่ายได้รับการป้องกันโดยโปรโตคอล Robust Security Network (WPA2).
  - หากมีบล็อก `WPA` แสดงว่าเครือข่ายได้รับการป้องกันโดยโปรโตคอล Wi-Fi Protected Access (WPA).

## 4) ตั้งค่าโหมดการทำงาน
ตั้งค่าโหมดการทำงานที่เหมาะสมของการ์ดไร้สาย โดยเฉพาะอย่างยิ่ง,หากกำลังจะเชื่อมต่อกับเครือข่าย ad-hoc จะต้องตั้งค่าโหมดการทำงานเป็น `ibss`

โหมด Ad-hoc ช่วยให้คอมพิวเตอร์สามารถเชื่อมต่อกันโดยตรงโดยไม่ต้องใช้ Access Point
```
iw dev interface set type ibss
```
```
# iw dev wlan0 set type ibss

# iw dev wlan0 info

Interface wlan0
    type: ibss
    channel: 6
    SSID: MyAdhocNetwork
    BSSID: 00:1a:7d:da:71:11
    signal: -60 dBm
    frequency: 2.412 GHz
    RX bitrate: 65 Mbps
    TX bitrate: 65 Mbps
    capabilities: [IBSS][ESS]
    ...
```

## คำสั่งที่เกี่ยวกับ Wireless Adapter ใน Linux
### ตรวจสอบสถานะ :

- `iwconfig`: แสดงข้อมูลเกี่ยวกับ Wireless Interface ทั้งหมด
- `ip link`: แสดงข้อมูลเกี่ยวกับ Network Interface ทั้งหมด
- `nmcli device status`: แสดงสถานะของ Wireless Adapter
- `iw dev <interface> scan`: ค้นหา Wireless Network ที่ใช้งานได้

### การเชื่อมต่อ :

- `nmcli device connect <SSID>`: เชื่อมต่อกับ Wireless Network - โดยระบุ SSID
- `iw dev <interface> connect <SSID>`: เชื่อมต่อกับ Wireless Network โดยระบุ SSID

> [!WARNING]
>
> **ข้อควรระวังการใช้งาน Wireless Adapter**
> - การเลือก Wireless adapter ที่รองรับ
>   - ตรวจสอบให้แน่ใจว่า Wireless adapter ที่คุณเลือกนั้นรองรับ Linux 
> - การกำหนดค่า Wireless adapter
>   - หลังจากติดตั้งไดรเวอร์แล้ว ต้องกำหนดค่า Wireless adapter สามารถทำได้โดยใช้ Network Manager หรือ CLI (Command Line Interface)

## 📚 Reference

- https://wiki.archlinux.org/title/Network_configuration/Wireless
- https://wireless.wiki.kernel.org/en/users/Documentation/iw/replace-iwconfig
- https://www.ninetechno.com/a/computer-word/1679-wireless-adapter-%E0%B8%AB%E0%B8%A1%E0%B8%B2%E0%B8%A2%E0%B8%96%E0%B8%B6%E0%B8%87%E0%B8%AD%E0%B8%B0%E0%B9%84%E0%B8%A3.html
