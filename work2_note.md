### opts 作法
-------------
`update(g)` 
`initParam()`

### rs 作法
-----------
`getData()`
`zeroGrad()`
`sample()`
`run()`
`crc()`

### 合併
--------
``` python
# sklearn
from sklearn import svm
clf = svm.SVC(gamma=0.001, C=100.)
clf.fit(digits.data[:-1], digits.target[:-1])  
clf.predict(digits.data[-1:])
```

### Questions
-------------
1. **RANDSEED** 需要一直重複宣告嗎
    ```
    RANDSEED = 9999
    numpy.random.seed(RANDSEED)
    ```
2. 迴歸和分類問題的opts 如何合併?
sklearn將這2類視為不同的的module