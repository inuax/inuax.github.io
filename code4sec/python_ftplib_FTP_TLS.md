# FTP - Clear-text protocols

## Secure FTP Server

FTP (File Transfer Protocol) คือโพรโทคอลที่ออกแบบมาเพื่อใช้ในการรับส่งไฟล์ระหว่าง Client และ Server โดยจะมีพอร์ตที่ใช้งานอยู่ 2 พอร์ต คือ พอร์ต 20 ใช้ในการรับส่งไฟล์ ส่วนอีกพอร์ตคือ พอร์ต 21 ใช้ในการควบคุมหรือส่งคำสั่ง FTP เช่น ตรวจสอบการเข้าถึงโปรแกรมจากผู้ใช้งาน เป็นต้น และในปัจจุบัน ผู้ให้บริการ Web hosting โดยส่วนใหญ่มักจะให้บริการแลกเปลี่ยนไฟล์ผ่าน FTP Server เพราะการการติดตั้งระบบและการบริหารจัดการไฟล์ทำได้ง่าย

แต่ FTP เป็นโพรโตคอลที่รับส่งข้อมูลโดยไม่มีการเข้ารหัสลับ จึงทำให้ข้อมูลที่รับ-ส่ง เช่น Username หรือ Password สามารถถูกดักรับ (Sniff) จากผู้ไม่ประสงค์ดีได้ 

![](img/ftptls_1.jpg)

ซึ่งโปรโตคอลเหล่านี้ไม่ปลอดภัยเนื่องจากทำให้แอปพลิเคชันมีความเสี่ยงต่างๆ เช่น
* Sensitive data exposure
* Traffic redirected to a malicious endpoint
* Malware infected software update or installer
* Execution of client side code
* Corruption of critical information

วิธีการแก้ไขนั้นควรเปลี่ยนมาใช้ Protocol สำหรับแลกเปลี่ยนข้อมูลที่มีการเข้ารหัสลับข้อมูลที่รับส่งเสมอ ซึ่งมีโพรโทคอลที่ถูกออกแบบมาเพื่อแก้ปัญหาดังกล่าว คือ FTPS



Reference: 
* [https://standard.etda.or.th/wp-content/uploads/2018/09/20150405-ER-WAS-V07-33-R1.pdf](https://standard.etda.or.th/wp-content/uploads/2018/09/20150405-ER-WAS-V07-33-R1.pdf)
* [https://docs.python.org/3/library/shlex.html](https://docs.python.org/3/library/shlex.html)
* [https://www.linkedin.com/pulse/os-command-injection-from-pythondjango-perspective-jerin-jose](https://www.linkedin.com/pulse/os-command-injection-from-pythondjango-perspective-jerin-jose)

Team Author: Ekawut Chairat
