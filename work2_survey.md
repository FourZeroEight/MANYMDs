## Contents
01. 目錄架構
02. 修改程式碼
03. 
04. 
05. 
06. 
07. 
08. 
09. 
10. 
11. 

## 目錄架構
``` 
ciml/
    opts/
        __init__.py
        func.py # Generate a random multivariate function
        opts.py # Base optimiser class: Opts
        sgd.py # 繼承 Opts
        mom.py # 繼承 Opts
        samplesC.py # Produce classification data samples
        samplesR.py # # Produce regression data samples

    rand/
        __init__.py
        detrandom.py # Module for deterministic random processes in pyspark environment

	rs/
	    # Spark required
        __init__.py
        cf.py # on progress
        cv.py # split data
        data.py # RS Data representation
        param.py # a abstract parameter holder
        rs.py # inherited from rsbase2.rs.RS
        sampler.py # syntax error?

	rsbackend/
		__init__.py
        spark/ # ?
            __init__.py
            data.py
            databuilder.py
            datamapper.py
            dmat.py

	rsbase/
        __init__.py
        datagen.py # class: DataGen1 --> 0, >1 區間
                   # class: DataGen2 --> 0, 1 區間
        randseed.py # 沒被用到?
        rs.py # Virtual RS base
        rs1.py # each iteration:
                  # update(): 計算梯度 pgrad
                  # rs.opts(): 更新參數 param, 並把 pgrad 歸零
        rs2.py # 省略

	rsbase1/ 
        __init__.py
        datagen.py # 和`rsbase.datagen`相同
        rs.py # 和`rsbase.rs`相同
        rs1.py # 和`rsbase.rs1`相比, 似乎只差在 r 有無特地取出回傳(function `sample`)
        rs2.py # 和`rsbase.rs2`相同

	rsbase2/
        __init__.py
        datagen.py # 和`rsbase.datagen`相同
        rs.py # 和`rsbase.rs`相同
        rs1.py # 和`rsbase.rs1`相比, 修改回傳變數 (i, u, r --> arg)
        rs2.py # 和`rsbase.rs2`相比, 修改回傳變數 (i, u, r --> arg)

	utils/
        __init__.py
        crcbase.py
        randobj.py # rState
        randseed.py # singleton fashion
```
## 修改程式碼
1. 將所有 import 改成用 **絕對路徑** 導入模組 [PEP8](https://legacy.python.org/dev/peps/pep-0008/)

## Questions

## Spark Keywords
1. map v.s. mappartition
2. 
