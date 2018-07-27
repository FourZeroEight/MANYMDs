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
`recvobj = Comm.recv(buf=None, source=ANY_SOURCE, tag=ANY_TAG, status=None)`
`Comm.sendrecv(sendobj, dest, sendtag=0, recvbuf=None,
               source=ANY_SOURCE, recvtag=ANY_TAG, status=None)`
**雙方需要同時有 function, 否則某個 process 會一直等待**
               
**Collective**
`recv_obj = Comm.bcast(obj, root=0)`
`recv_obj = Comm.scatter(sendobj, root=0)`
`recv_obj = Comm.gather(sendobj, root=0)`

**Alltoall**
`recv_obj = Comm.alltoall(sendobj)`
`Comm.Alltoall(sendbuf, recvbuf)`

> All-to-all 定制通讯也叫做全部交换。这种操作经常用于各种并行算法中，比如快速傅里叶变换，矩阵变换，样本排序以及一些数据库的 Join 操作。

**Reduction**
`recvobj = Comm.reduce(sendobj, op=SUM, root=0)`

[0], [1], [2], [3] --SUM--> [0 1 2 3]
[0 1 2], [1 2 3], [2 3 4] --SUM--> [0 1 2 1 2 3 2 3 4 3 4 5]
1, 2, 3, 4 --SUM--> 10
1, 2, 3, 4 --PROD--> 24

`Comm.Reduce(sendbuf, recvbuf, op=SUM, root=0)`
`Comm.Allreduce(sendbuf, recvbuf, op=SUM)`

deadlock ?
=========
Buffer ?
=======
