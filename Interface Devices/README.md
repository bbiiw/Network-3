## Interface Devices

Network Interfaces คืออุปกรณ์ Hardware หรืออุปกรณ์เสมือนที่อนุญาตให้ระบบเชื่อมต่อกับเครือข่าย Interfaces ทางกายภาพเหล่านี้สามารถแบ่งออกเป็น 3 ส่วน ดังนี้ 
1. [**LAN Card**](./LAN%20Card/README.md) (Network Interface Card - NIC) เป็นอุปกรณ์ที่เชื่อมต่อเครือข่ายคอมพิวเตอร์กับเครือข่าย LAN เพื่อให้สามารถสื่อสารกับเครื่องคอมพิวเตอร์อื่น ๆ ในเครือข่าย
2. [**Wireless Adapter**](./Wireless%20Adapter/README.md) เป็นอุปกรณ์ที่ทำให้เครื่องคอมพิวเตอร์สามารถเชื่อมต่อกับเครือข่ายไร้สายได้ เรียกอีกชื่อหนึ่งว่า Wi-Fi Adapter หรือ WLAN Card มีหน้าที่เป็นตัวกลางในการรับและส่งสัญญาณไร้สายระหว่างเครื่องคอมพิวเตอร์กับจุดเชื่อมต่อ (Access Point) หรือระบบไร้สายอื่น ๆ ที่ให้บริการ
3. [**Aircard Adapter**](./Aircard%20Adapter/README.md) เป็นอุปกรณ์ที่ช่วยให้คอมพิวเตอร์สามารถเชื่อมต่อกับเครือข่ายอินเทอร์เน็ตผ่านทางการสื่อสารไร้สาย 3G/4G USB Modem เป็นการให้บริการอินเทอร์เน็ตผ่านเครือข่ายโทรศัพท์มือถือ

แต่ละ Network Interface มีชื่อที่ไม่ซ้ำกัน เช่น eth0 หรือ wlan0 และจะมี MAC address ที่ไม่ซ้ำกันเป็นตัวแยกแต่ละตัว มีการกำหนดโดยผู้ผลิตสำหรับแต่ละอินเทอร์เฟซ

Linux รองรับหลายประเภทของ Network Interface เช่น Ethernet, Wireless (Wi-Fi), Bluetooth, 3G/4G cellular modems และ Virtual Interface เช่น loopback interface (lo)
