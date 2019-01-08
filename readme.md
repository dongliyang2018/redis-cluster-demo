# Redis sentinel group demo

## How to run

### 1. Run redis master and slave
* redis-server ./redis-16379.conf
* redis-server ./redis-26379.conf
* redis-server ./redis-36379.conf

### 2. Run redis sentinel
* redis-sentinel ./sentinel-16380.conf
* redis-sentinel ./sentinel-26380.conf
* redis-sentinel ./sentinel-36380.conf

### 3. Check status
1. redis-cli -h 192.168.83.136 -p 16379
2. auth "123456"
3. info replication

### 4. Sentinel cmd
1. redis-cli -h 192.168.83.136 -p 16380
2. sentinel masters
3. sentinel master <master_name>
4. sentinel slaves <master_name>