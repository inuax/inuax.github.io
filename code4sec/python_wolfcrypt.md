# Symmetric Key Algorithms with Wolfcrypt

### **#Code4Sec Week #DayX #NEIS0736 #NECS0736**

## Symmetric-key Cryptography

ระบบรหัสแบบสมมาตร (Symmetric-key Cryptography) เป็นระบบรหัสที่ใช้ Key ชุดเดียวกันทั้งผู้ส่งและผู้รับในการเข้ารหัสและถอดรหัสลับ จากสาเหตุนี้จึงเป็นที่มาของชื่อระบบรหัสแบบสมมาตร เนื่องจากคำว่า “สมมาตร” เป็นการอธิบายถึงความเท่ากันหรือเหมือนกันของทั้งสองข้าง   Key ซึ่งอยู่ในรูปรหัสเป็นตัวแปรสำคัญสำหรับการเข้าและถอดรหัสลับข้อมูล ซึ่งขนาดของ Key มีหน่วยเป็น bit จะแสดงถึงระดับความปลอดภัยของข้อมูลที่ได้รับการเข้ารหัสลับ โดยการใช้ Key ที่มีความยาวหรือจำนวนบิตมาก จะทำให้การเข้ารหัสลับข้อมูลนั้นมีความปลอดภัยมากยิ่งขึ้นอีกด้วย

![](img/wolfcrypt_1.png)

ที่นี่เรามาลองทำ Symmetric-key Cryptography ด้วย Wolfcrypt จากภาษา Python กันเลย

ในตัวอย่างจะเป็นการเข้ารหัส Symmetric-key Cryptography ด้วย Advanced Encryption Standard หรือ AES โดย wolfcrypt.ciphers มี class ที่สามารถเรียกใช้งานกันได้เลย

``` python 
class Aes(_Cipher):
    """
    The **Advanced Encryption Standard** (AES), a.k.a. Rijndael, is
    a symmetric-key cipher standardized by **NIST**.
    """
    block_size = 16
    key_size = None  # 16, 24, 32
    _key_sizes = [16, 24, 32]
    _native_type = "Aes *"

    def _set_key(self, direction):
        if direction == _ENCRYPTION:
            return _lib.wc_AesSetKey(
                self._enc, self._key, len(self._key), self._IV, _ENCRYPTION)

        return _lib.wc_AesSetKey(
            self._dec, self._key, len(self._key), self._IV, _DECRYPTION)

    def _encrypt(self, destination, source):
        return _lib.wc_AesCbcEncrypt(self._enc, destination,
                                     source, len(source))

    def _decrypt(self, destination, source):
        return _lib.wc_AesCbcDecrypt(self._dec, destination,

                                     source, len(source))
```

**AES**

``` python
class wolfcrypt.ciphers.Aes(key, mode, IV=None)
```
The Advanced Encryption Standard (AES), a.k.a. Rijndael, is a symmetric-key cipher standardized by NIST.

โดยสิ่งที่เราต้องระบุให้กับ Aes คือ key = neis0736necs0736, mode = MODE_CBC, IV=0123456789abcdef

และข้อมูลที่เราจะทำการ encrypt คือคำว่า "Code 4 Sec Week "

**Example:**

``` python
from wolfcrypt.ciphers import Aes, MODE_CBC

cipher = Aes(b'neis0736necs0736', MODE_CBC, b'0123456789abcdef')

# Encrypt
ciphertext = cipher.encrypt('Code 4 Sec Week ')
print('Cipher Text = ', ciphertext)

# Decrypt
plaintext = cipher.decrypt(ciphertext)
print('Plain Text = ', plaintext)
```

**Result:**

![](img/wolfcrypt_2.png)

จากการลองทำ  ข้อมูลจะเห็นว่า wolfcrypt จะทำการ encrypt คำว่า "Code 4 Sec Week " ให้เป็น cipher text ที่ถูกเข้ารหัสด้วย key ที่เราระบุ หลังจากนั้นลองทำการ decrypt ข้อมูล cipher text ด้วย key เดิม เราจะได้ plain text เป็นข้อมูลเดิม คือ คำว่า "Code 4 Sec Week " ตามหลักการของ Symmetric-key Cryptography

Reference: 
* [https://wolfssl.github.io/wolfcrypt-py/symmetric.html](https://wolfssl.github.io/wolfcrypt-py/symmetric.html)

**Team Author:** Ekawut Chairat