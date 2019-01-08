# Redis cluster demo

## How to run

### 1. Run redis node
* redis-server ./redis-16379.conf
* redis-server ./redis-16380.conf
* redis-server ./redis-16381.conf
* redis-server ./redis-16382.conf
* redis-server ./redis-16383.conf
* redis-server ./redis-16384.conf

### 2. Use cluster meet cmd connect other nodes
1. redis-cli -h 192.168.83.136 -p 16379
2. cluster meet 192.168.83.136 16380
3. cluster meet 192.168.83.136 16381
4. cluster meet 192.168.83.136 16382
5. cluster meet 192.168.83.136 16383
6. cluster meet 192.168.83.136 16384

### 3. Show cluster nodes
1. redis-cli -h 192.168.83.136 -p 16379
2. cluster nodes

```
71df986bb290ddf4fcc201f33163b9f8d3372889 192.168.83.136:16379 myself,master - 0 0 2 connected
2d482caca68fc3705547c096edff8e4f23cffca9 192.168.83.136:16380 master - 0 1546927209842 1 connected
ead885c1d3fad47f25cdfbabe4a879f92dd71510 192.168.83.136:16381 master - 0 1546927211855 0 connected
410e4365a14519779063da041b9f6065cd4f37ed 192.168.83.136:16382 master - 0 1546927211855 3 connected
00b83e4c1395336c45f6091d1b71f40e2abfbd5e 192.168.83.136:16383 master - 0 1546927212865 4 connected
e382ad73ed9a52e177d5a5883dd803b3c4a98a58 192.168.83.136:16384 master - 0 1546927210843 5 connected
```

### 4. Assign slots
1. redis-cli -h 192.168.83.136 -p 16379 cluster addslots {0..5461}
2. redis-cli -h 192.168.83.136 -p 16380 cluster addslots {5462..10922}
3. redis-cli -h 192.168.83.136 -p 16381 cluster addslots {10923..16383}

### 5. Assgin replication
1. redis-cli -h 192.168.83.136 -p 16382 cluster replicate 71df986bb290ddf4fcc201f33163b9f8d3372889
2. redis-cli -h 192.168.83.136 -p 16383 cluster replicate 2d482caca68fc3705547c096edff8e4f23cffca9
3. redis-cli -h 192.168.83.136 -p 16384 cluster replicate ead885c1d3fad47f25cdfbabe4a879f92dd71510

**16382 is replication of 16379,16383 is replication of 16380,16384 is replication of 16381**

### 6. Check cluster
redis-cli -h 192.168.83.136 -p 16379 cluster nodes

```
71df986bb290ddf4fcc201f33163b9f8d3372889 192.168.83.136:16379 myself,master - 0 0 2 connected 0-5461
00b83e4c1395336c45f6091d1b71f40e2abfbd5e 192.168.83.136:16383 slave 2d482caca68fc3705547c096edff8e4f23cffca9 0 1546929804708 4 connected
2d482caca68fc3705547c096edff8e4f23cffca9 192.168.83.136:16380 master - 0 1546929802687 1 connected 5462-10922
e382ad73ed9a52e177d5a5883dd803b3c4a98a58 192.168.83.136:16384 slave ead885c1d3fad47f25cdfbabe4a879f92dd71510 0 1546929805718 5 connected
410e4365a14519779063da041b9f6065cd4f37ed 192.168.83.136:16382 slave 71df986bb290ddf4fcc201f33163b9f8d3372889 0 1546929803698 3 connected
ead885c1d3fad47f25cdfbabe4a879f92dd71510 192.168.83.136:16381 master - 0 1546929803698 0 connected 10923-16383

```