HMAC: Keyed-Hashing for Message Authentication
==============================================
HMAC 重點整理

Formula: **H(K XOR opad, H(K XOR ipad, text))**

``` python
# hmac.py (3.6)
key = key.ljust(blocksize, b'\0') # 補0
self.outer.update(key.translate(trans_5C))
self.inner.update(key.translate(trans_36))
if msg is not None:
    self.update(msg)

def _current(self):
    """ 只有在要求 digest 時, 才會做最後的 Hash function
    """
    h = self.outer.copy()
    h.update(self.inner.digest())
    return h
```
* self.inner 裡面的Hash function
* self.inner 外面的Hash function
* a.translate(b) == a XOR b

``` python
# hmac.py (3.7)
# 新增 function: hmac.digest --> 簡化計算HMAC的流程
import hmac
hmac.digest(key=xxx, msg=xxx, digest=xxx)
```


