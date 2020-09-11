# RedisCluster-Sentinel
Setup Redis Cluster Master/Slave with Sentinel Fail-over and Redis EndPoint Route By HAProxy
Hello, welcome, the configs of how to implement " Redis Master/Slave replication" and addition to this, also showed how to implement cluster fail over managed by sentinel service and how the application will point to master based on HAPROXY routing.  

The steps are in details and in sequences to deploy a successful case.  

Starting with 4 vm ubuntu 
 1. redis-a 192.168.1.121
 2. redis-b 192.168.1.122
 3. redis-c 192.168.1.123
 4. haproxy 192.168.1.125
 ---------------------------------------------------
 Step sequences:
Things to do:
     1. implement redis cluster master/slave replication 
          a. master node : redis-a: 192.168.1.121 [ad joined, read/write]
               1. AD joined
               2. install redis-server
                    a. update and upgrade
                    b. install redis-server
                    c. test redis     
               3. install redis-sentinel
                    a. install redis-sentinel
                    b. stop sentinel service
               4. set permission for user: 'redis'
                    a. sentinel conf
                    b. sentinel log
               5. allow ports main[6379, 26379], [for app test 22 not required]
               6. configure redis-server
                    a. bind
                    b. port
                    c. protected-mode no
                    d. restart redis-server
          b. slave node 1 : redis-b: 192.168.1.122 [ad joined, read only]
               1. AD joined
               2. install redis-server
                    a. update and upgrade
                    b. install redis-server
                    c. test redis
               3. install redis-sentinel
                    a. install redis-sentinel
                    b. stop sentinel service
               4. set permission for user: 'redis'
                    a. sentinel conf
                    b. sentinel log
               5. allow ports main[6379, 26379], [for app test 22 not required]
               6. configure redis-server
                    a. bind
                    b. port
                    c. protected-mode no
                    d. slaveof master [redis-a: 192.168.1.121]
                    e. restart redis-server
          c. slave node 2 : redis-c: 192.168.1.123 [ad joined, read only]
               repeat node 1 steps.
     2. implement redis failover using sentinel
          a. master node : redis-a: 192.168.1.121
               1. configure sentinel.conf
                    a. bind
                    b. protected-mode no
                    c. port
                    d. monitor mymaster 192.168.1.121
                    e. down time
                    f. failover time
               2. configure sentinel.service
                    a. start with redis-server
               3. reload daemon   
               4. restart sentinel
               5. the change by sentinel in sentinel.conf
          b. slave node 1 : redis-b: 192.168.1.122
               1. configure sentinel.conf
                    a. bind
                    b. protected-mode no
                    c. port
                    d. monitor mymaster 192.168.1.121
                    e. down time
                    f. failover time
               2. configure sentinel.service
                    a. start with redis-server
               3. reload daemon   
               4. restart sentinel
               5. the change by sentinel in sentinel.conf 
          c. slave node 2 : redis-c: 192.168.1.123
              repeate node1 steps.
     3. implement haproxy for redis [to identify master]
          a. already deployed haproxy
          b. add block for tcp mode for redis with send and expect rules
     4. test from client
          when redis-b is down -- redis-a promote to master
          when redis-a is down -- redis-c promote to master

 
You can download the config files @

Software used:
1. VM workstation 14 [https://www.vmware.com/products/workstation-pro.html]
2. Bandicam: [https://www.bandicam.com/]
3. Ubuntu 18.04 [https://releases.ubuntu.com/18.04/]
4. Microsoft Server 2016 [https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016]




#redissentinel
#rediscluster
#redis
