> 同一進程的多個不同的線程可以共享相同的資源。相比而言，進程之間不會共享資源。

``` python
from mpi4py import MPI

comm = MPI.COMM_WORLD
rank = comm.rank
size = comm.size
```

API
===
**P2P**
`Comm.send(obj, dest, tag=0)`
`Comm.recv(buf=None, source=ANY_SOURCE, tag=ANY_TAG, status=None)`

**Collective**
`Comm.bcast(obj, root=0)`
`Comm.scatter(sendobj, root=0)`
`Comm.gather(sendobj, root=0)`

**Alltoall**
`Comm.Alltoall(sendbuf, recvbuf)`

> All-to-all 定制通讯也叫做全部交换。这种操作经常用于各种并行算法中，比如快速傅里叶变换，矩阵变换，样本排序以及一些数据库的 Join 操作。

**Reduction**
`Comm.reduce(sendobj, op=SUM, root=0)`

deadlock ?
=========
Buffer ?
=======
