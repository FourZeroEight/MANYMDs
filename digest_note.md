HMAC: Keyed-Hashing for Message Authentication
==============================================
公式: **H(K XOR opad, H(K XOR ipad, text))**

HMAC 套件比較
-------------
* hashlib + `hmac` v.s `pycrypto`
* `hmac`可以修改 opad 和 ipad (有意義嗎??); `pycrypto` 沒這功能
* `hmac` 支援 sha3; `pycrypto` 只支援 sha2, 無法調用 hashlib 的 hash function
* `hmac` 的 XOR 用 bytes.translate; `pycrypto` 的 XOR 調用 c function
* 

``` python
# hmac.py (3.6)

key = key.ljust(blocksize, b'\0') # 補0
self.outer.update(key.translate(trans_5C)) # 裡面的Hash function
self.inner.update(key.translate(trans_36)) # 外面的Hash function
if msg is not None:
    self.update(msg)

def _current(self):
    """ 只有在要求 digest 時, 才會做最後的 Hash function
    """
    h = self.outer.copy()
    h.update(self.inner.digest())
    return h
```

``` python
# hmac.py (3.7)
# 新增 function: hmac.digest --> 簡化 HMAC 流程

import hmac, hashlib
hmac.digest(key=b'123', msg=b'123', digest=hashlib.sha3_512)
```


