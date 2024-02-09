# Routing
Networking นั้นเป็นส่วนสำคัญต่อ computer infrastructure สมัยปัจจุบันอย่างมาก ไม่ว่าจะเป็นเพื่อ web browsing หรือการใช้งาน internet หรือใช้งานอย่างเฉพาะเจาะจงในบาง Application  การส่ง TCP/IP packet มักจะเป็นวิธีหลักในการส่งข้อมูล และเนื่องจากเครื่อข่ายมีการเชื่อมต่อกันตั้งแต่ ในระดับnode จนถึงระดับภูมิภาคหรือระดับโลก เราจึงต้องการวิธีในการหาเส้นทางในการส่งต่อข้อมูล
<br>
<br>
## **2 main-concepts**

___

1) **Routing หรือการหาเส้นทาง**
2) **Forwarding หรือการส่งต่อข้อมูล**

**Routing** เป็นกระบวนการในการส่ง packetข้อมูลจากnetworkหนึ่งไปสู่อีกnetworkหนึ่ง โดยหาเส้นทางที่มีประสิทธิภาพมากที่สุด โดยใน Linux Operating System Routing เกี่ยวข้องกับการใช้งาน Karnel Routing Table ที่มีข้อมูลถึงวิธีการส่งข้อมูลไปยังปลายทางที่ต้องการได้อย่างไร

Linux kernel รองรับ routing protocol หลายๆตัว รวมถึง  Routing Information Protocol (RIP), Open Shortest Path First (OSPF), and Border Gateway Protocol (BGP). ซึ่งเป็นโปรโตคอลที่มีใช้การแลกเปลี่ยนเส้นทางระหว่าง routers ทำให้สามารถหาเส้นทางที่ดีที่สุดในการส่งข้อมูลได้

## **ประโยชน์ของการ Routing**
---
1) **เพิ่มประสิทธิภาพ network performance** : routing ทำให้สามารถส่ง data packet ได้อย่างมีประสิทธิภาพ เป็นผลให้อัตราการส่งไวขึ้นและลด latency

2) **เพิ่มประสิทธิภาพด้าน network security** : การทำ routing ทำให้เกิดการสร้าง network แยกที่มี security ต่างกัน ทำให้ความเสี่ยงจากการเข้าถึงที่ไม่ได้รับอนุญาต และป้องกันการรั่วไหลของข้อมูล

3) **เพิ่มความยืดหยุ่น (scalability) ให้กับnetwork** : การทำ routing ทำให้ network สามารถขยายขึ้นโดยกการเพิ่ม subnet และ router โดยไม่กระทบต่อเน็ตเวิร์คที่มีอยู่ในปัจจุบัน

## หลักการทำงานและการconfig ของ routing
---
1) **ติดตั้งTools ในการใช้ route commands**
2) **ตรวจสอบเส้นทาง route ที่มี**
3) **เปิดใช้งาน ip forwarding**
4) **เพิ่ม routes ลงในตารางเส้นทาง**

<br>

### **1) ติดตั้ง Tools ในการใช้ route commands**

คำสั่งในการติดตั้ง Tools ในการใช้ route commands

**ในกรณีที่ใช้ ubuntu/debian distribution**

`$ apt-get install net-tools`

**ในกรณีที่ใช้ redhatdistribution**

`$ yum install net-tools`

![alt text](../Network-3/image/routing1.png)

การมีที่อยู่ IP จะทำให้ระบบของคุณสามารถสื่อสารกับระบบอื่นในเครือข่ายเดียวกันได้ และด้วยอุปกรณ์ routing คุณสามารถสื่อสารกับระบบในเครือข่ายอื่นได้


### **2) แสดงตารางข้อมูลเส้นทางที่มีอยู่ในปัจจุบัน**

ก่อนที่จะทำการแก้ไข ก่อนอื่นเราต้องตรวจสอบเงื่อนไขที่มีอยู่ในเส้นทางปัจจุบันก่อนที่จะทำการเปลี่ยนแปลงเส้นทาง

คำสั่งการ route ใน Linux ใช้สำหรับแก้ไขหรือทำงานกับตารางเส้นทาง IP/kernel ซึ่งการใช้งานหลักคือการตั้งค่าเส้นทางแบบStatic Route ในโฮสต์หรือเครือข่ายที่ต้องการ คำสั่งนี้มักใช้เพื่อแสดงเนื้อหาของตารางเส้นทาง ซึ่งเป็นข้อมูลที่แสดงเส้นทางการส่งข้อมูลในระบบเครือข่าย

### **คำสั่ง แสดง IP/kernel routing table:**
`$ route`

`$ route -n`

`$ The netstat -rn `

`$ ip route show`

![alt text](../Network-3/image/routing2.png)
![alt text](../Network-3/image/routing3.png)

คำสั่ง route หรือ netstat -r จะแสดงตารางเส้นทางการเชื่อมต่อ;
โดย -n จะแสดงผลลัพธ์เฉพาะเป็นที่อยู่ IP เท่านั้น และไม่พยายามทำการค้นหาชื่อ DNS ซึ่งจะแทนที่ที่อยู่ IP ด้วยชื่อโฮสต์หากมีอยู่

default gateway มักจะแสดงด้วยที่อยู่ 0.0.0.0 เมื่อใช้คำสั่ง -n หากไม่ได้ใช้ก็จะได้คำว่า "Default" แทน 
IP address ใน Gateway column คือ gateway ขาออก โดย netmask of 0.0.0.0 สำหรับ default gateway หมายถึงpacket ใดก็ตามที่ไม่มีการระบุใน local network หรือ routerขาออกอื่นๆ ด้วยentriesที่มีใน routing table ก็จะถูกส่งต่อไปยัง default gateway

เมื่อใช้ netstat หรือ route ในส่วน Iface (Interface) column คื่อชื่อของ NIC ขาออก(ตามภาพคือ enp0s3) 
Flag ใน Flag column แสดงว่า เส้นทางนั้น Up (U) หรือเป็น default Gateway (G).  หรือมีสถานะอื่นๆ

โดยเมื่อเราเชื่อมต่อกับคอมพิวเตอร์อื่น ๆ อาจใช้ที่อยู่ IP หรือชื่อโฮสต์ก็ได้ 
ชื่อโฮสต์สามารถใช้แทนได้หากมีการป้อนเข้าไปในไฟล์ /etc/hosts พร้อมกับที่อยู่ IP ที่เกี่ยวข้อง หรือหากมีเซิร์ฟเวอร์DNS ให้การแปลงที่อยู่ IP เป็นชื่อโฮสต์ 

โดยหลายๆชื่อที่มักจะอยู่ในไฟล์ /etc/hosts คือ localhost/ localhost.localdomain  ทั้งสองนี้ใช้เพื่ออ้างถึงเครื่องปัจจุบัน

![alt text](../Network-3/image/routing4.png)

ลองทำการ cat แสดงเนื้อหาในไฟล์  /etc/hosts เพื่อดู ip ที่ถูกแปลงเป็น hostname

### **3) การเปิดใช้งาน IP forwarding ใน Linux**

**ขั้นตอนที่ 1: เปิดใช้งาน IP forwarding**

IP forwarding เป็นคุณสมบัติที่ทำให้ Linux kernel สามารถส่งต่อแพ็กเก็ตระหว่างเครือข่ายได้ เพื่อเปิดใช้งาน IP forwarding ให้ทำการรันคำสั่งต่อไปนี้:

`$ sudo nano /etc/sysctl.conf`