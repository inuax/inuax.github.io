# เริ่มใช้งาน Configuration File ในภาษา Python ด้วย Configparser
## Hard-coded credentials are security-sensitive

#Code4Sec Week #Day4 #NEIS0736 #NECS0736

ถ้าเราพูดถึงการพัฒนาโปรแกรมในปัจจุบันโดยมีการ hardcoding ข้อมูล security-sensitive เช่น username, password, IP address จะส่งผลต่อความปลอดภัยของ application ผู้ไม่หวังดีอาจ decompile code และด้วยเหตุนี้อาจทำให้ผู้ไม่หวังดีค้นพบข้อมูล sensitive ภายใน code ของเราได้ หลังจากนั้นผู้ไม่หวังดีสามารถโจมตีรูปแบบต่างๆ เช่น Denial of Service ไปยัง IP Address หรือทำการโจมตีด้วย IP address spoofing จาก IP Address ที่พบใน code ได้ อีกทั้งในปัจจุบันมีสถาปัตยกรรมที่เปลี่ยนแปลงอยู่ตลอดเวลาเนื่องจากความต้องการด้าน scaling และ redundancy อาจทำให้เกิดความผิดพลาด เมื่อมีการเปลี่ยนแปลงค่าต่างๆ และการทำ hardcoding จะต้องแก้ไขค่าเหล่านั้นด้วยซึ่งจะมีผลกระทบต่อการพัฒนาโปรแกรม การจัดส่ง และการปรับใช้งานอีกด้วย

![](img/configget_1.png)

**ตัวอย่างการ hardcoding**
``` python
import socket

#Code4Sec
ip = '192.168.12.42'
sock = socket.socket()
sock.bind((ip, 9090))
```
จากตัวอย่างมีการ hardcoding ค่า IP Address ไว้ใน code ของโปรแกรม หากผู้ไม่หวังดีสามารถเข้าถึง code ได้ เราจะมีความเสี่ยงด้านความมั่นคงปลอดภัยทันที

**ตัวอย่างการใช้งาน ConfigParser**

สำหรับ ConfigParser Module มีพื้นฐานคล้ายกับการทำ configuration file ที่ใช้กันใน Microsoft Windows แบบไฟล์ INI ซึ่งในภาษา Python เราก็พัฒนาโปรแกรมโดยใช้ configuration file แบบง่ายได้ด้วย module นี้ 

![](img/configget_2.png)

``` python
from configparser import ConfigParser

#Read config.ini file
config_object = ConfigParser()
config_object.read("config.ini")

#Get the password
serverinfo = config_object["CODE4SEC"]
ipaddress = serverinfo["ipaddr"]

print("IP Address is", ipaddress)
```
เราลองนำ ConfigParser Module มาใช้งานแทนการใช้ hardcoding โดยทำการ import ConfigParser เข้ามาในโปรแกรม โดยอันดับแรกเราจะสร้าง Configuration File (.ini) ที่ประกอบด้วยหนึ่งหรือหลาย Section โดยใช้เครื่องหมายวงเล็บเปิดและปิด [section_name] โดยในแต่ละ Section จะมี Key และ Value ที่คั่นด้วยเครื่องหมาย "=" หรือ ":" เช่น  host = test.com หรือ host : test.com

``` ini
# config.ini

[CODE4SEC]
hostname = test.com
port = 8080
ipaddr = 192.168.1.1
```

จากตัวอย่างเราจะทำการสร้าง configuration file ชื่อว่า config.ini ไว้ใน directory เดียวกันกับตัวโปรแกรม โดยใน file จะมีข้อมูล จะมี Section ชื่อว่า CODE4SEC ประกอบด้วย Key และ Value ของค่า host, IP address และ port อยู่ และใช้คำสั่ง config_object.read("config.ini") เพื่อทำการอ่านข้อมูล ใช้คำสั่ง serverinfo = config_object["CODE4SEC"] เพื่อเรียก section ชื่อ CODE4SEC และ ipaddress = serverinfo["ipaddr"] เพื่อนำ Key และ Value ของ ipaddr ไปใช้งานต่อได้เลย

![](img/configget_3.png){:height="50%" width="50%"}

ผลลัพธ์จากการ print() ตัวแปร ipaddress จะได้ค่าของ IP Address คือ 192.168.1.1 ซึ่งเป็นค่าที่ถูกเก็บไว้ใน config.ini ซึ่งเราสามารถนำตัวแปร ipaddress ไปใช้งานต่อในโปรแกรมได้เลย และหากต้องการแก้ไขค่า IP Address เราสามารถแก้ไขได้ในไฟล์ config.ini เพียงที่เดียว และยังสามารถบังคับให้ใช้ค่า configuration เดียวกันในทุก environment (dev, sys, qa, prod) ได้อีกด้วย

**Reference:**
* [https://docs.python.org/3/library/configparser.html](https://docs.python.org/3/library/configparser.html)

**Team Author:** 

Ekawut Chairat