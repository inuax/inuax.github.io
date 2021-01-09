# Python Secrets Module to Generate secure random data

## #Code4Sec Week #Day5 #NEIS0736 #NECS0736

ตั้งแต่ Python เวอร์ชั่น 3.6 ได้มีการเปิดตัว Secrets Module สำหรับสร้างข้อมูลแบบสุ่มที่มีความแข็งแรงและปลอดภัย เราจะมาเรียนรู้วิธีใช้  secret.SystemRandom() และ secrets.token() เพื่อทำการสร้าง secure random data ที่มีความมั่งคงปลอดภัย โดยก่อนหน้า Python เวอร์ชั่น 3.6 เรามีการใช้งาน os.urandom() และ random.SystemRandom() เพื่อสร้างข้อมูลแบบสุ่มที่มีความปลอดภัย

![](img/secrand_1.png){:height="75%" width="75%"}

Secrets module ทำงานแบบ Cryptographically Secure Pseudorandom Number Generator (CSPRNG) เป็นการสร้างตัวเลข Pseudo-Random ที่มีการเข้ารหัสที่แข็งแรงใช้เพื่อสร้างข้อมูลสุ่มซึ่งปลอดภัยและมีประโยชน์ในแอปพลิเคชันที่ต้องการความปลอดภัย โดยอ้างอิงจาก PEP - 0506 ที่ออกแบบมาเพื่อเพิ่ม Secrets Module เป็น library มาตรฐานใน Python และมีการสร้างข้อมูลแบบสุ่มโดยใช้ synchronization methods เพื่อให้แน่ใจว่าไม่มี process ใดที่สามารถรับข้อมูลเดียวกันในเวลาเดียวกันได้

ตัวอย่างการใช้งานทั่วไปของ secrets module ที่เกี่ยวข้อง
* Generating random numbers
* Passwords และ OTP
* Random token
* Password recovery safe URLs
* Session keys

Secrets module ทำงานอยู่บน os.urandom() และ random.SystemRandom() ซึ่งอาศัยการทำงานจาก Operating system เป็นหลัก เช่น

* Windows ใช้ os.urandom() ฟังก์ชั่น CryptGenRandom()
* Linux 3.17 หรือใหม่กว่า ใช้ฟังก์ชั่น getrandom() 
* OpenBSD 5.6  หรือใหม่กว่าใช้ฟังก์ชั่น C getentropy()

## Random numbers

### Class secrets.SystemRandom
เป็นคลาสสำหรับสร้างตัวเลขสุ่มโดยใช้ highest-quality sources ที่ระบบปฏิบัติการได้จัดเตรียมไว้ โดยเมื่อเรียกใช้งาน secrets.SystemRandom class เราก็จะสามารถใช้งานฟังก์ชั่นทั้งหมดของ random module ได้เลย

**ตัวอย่าง class secrets.SystemRandom**

### **secrets.choice(sequence)**

Return ค่า randomly ที่ถูกเลือกจาก non-empty sequence

```python
import secrets

code4secGenerator = secrets.SystemRandom()
# Secure Random choice using secrets
number_list = [6, 12, 18, 24, 30, 36, 42, 48, 54, 60]
secure_choice = code4secGenerator.choice(number_list)
print("code4secGenerator choice -> ", secure_choice)
```
Result:
```
code4secGenerator choice ->  54
```
### **secrets.randrange(n)**
Return ค่า random แบบ integer ระหว่าง 0 ถึง n
``` python
import secrets

code4secGenerator = secrets.SystemRandom()

secure_choice = code4secGenerator.randrange(100)
print("code4secGenerator randbelow -> ", secure_choice)
```
Result:
```
code4secGenerator randbelow ->  7
```

## Generating tokens

Secrets module มีฟังก์ชั่นสำหรับการสร้าง Token ที่ปลอดภัยสำหรับ application ที่ต้องการใช้งาน เช่น password resets, hard-to-guess URLs เป็นต้น

**ตัวอย่างการ generate tokens**

### **secrets.token_bytes([nbytes=None])**
Return ค่า random รูปแบบ byte string ที่มีข้อมูลขนาด n bytes

``` python
import secrets

token = secrets.token_bytes(32)
print(token)
```
Result:
```
b'\xe3\x87v\x9a\x08\xd1\xd5*\xe28\x9c\x8fwm4\xd8\xb3\xee\x01\t\x05T\xd4I\xda\x93\xf8\t\x10\x98\xc1{'
```

### **secrets.token_hex([nbytes=None])**
Return ค่า random ของ text string ในรูปแบบฐาน 16 (hexadecimal) ที่มีข้อมูลขนาด nbytes random bytes โดยในแต่ละ byte ถูกเปลี่ยนเป็น 2 Hex digits

Result:
```
1a36840ddb095370c37e477161f2fa1eb400e48be4ca97f0a314d1099840b165
```

### **secrets.token_urlsafe([nbytes=None])**
Return ค่า random ของ URL-safe ในรูปแบบ text string ที่มีข้อมูลขนาด n bytes โดยถูก encoded แบบ Base64

``` python
import secrets

url = 'https://code4sec.com/reset=' + secrets.token_urlsafe()
print(url)
```
Result:
```
https://code4sec.com/reset=VgZIgKN97sDsfhSQ14M5mG84hkIyLGHcfy_cXFp_7WQ
```

**Token ควรใช้ขนาดกี่ byte ?**

เพื่อความปลอดภัยจากการโจมตีด้วย brute-force attacks ต้องมีขนาดการสุ่มอย่างเพียงพอเนื่องจากปัจจุบัน computer มีประสิทธิภาพมากขึ้น ตั้งแต่ปี ค.ศ.2015 เชื่อกันว่าการสุ่มขนาด 32 bytes (256 bits) นั้นเพียงพอสำหรับกรณีการใช้งานทั่วไปที่ถูกคาดการณ์ไว้สำหรับ secrets module

**Reference:**
* [https://docs.python.org/3/library/secrets.html#module-secrets](https://docs.python.org/3/library/secrets.html#module-secrets)
* [https://pynative.com/python-secrets-module/](https://pynative.com/python-secrets-module/)

**Author:** 

Ekawut Chairat
