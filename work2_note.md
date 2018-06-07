### opts 作法
-------------
`__init__()`
verbose:
`update(g) !!!` 
`initParam() !!!`
`run()`
`printEval()`

### rsbase 作法
---------------
`__init__()`
k: latent factors 數量
data:
param dict: 
pgrad dict:
`getData()`
`zeroGrad()`
`initParams() !!!`
`update() !!!`
`opts() !!!`
`sample() !!!`
`eval() !!!`
`iter() !!!`
`run()`
`crc()`

### 合併
--------
## rs
__init__()
## rs_sgd

## rs_mom
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
    ``` python
    RANDSEED = 9999
    numpy.random.seed(RANDSEED)
    ```
2. 比較model performance 需求 ?

3. 迴歸問題和分類問題的 loss, grad 如何合併 ?
sklearn作法: 分別繼承自**BaseSGD** [sklearn](https://github.com/scikit-learn/scikit-learn/blob/master/sklearn/linear_model/stochastic_gradient.py#L46)

4. params需要放入dict裡嗎 ?
