# RedisCluster-Sentinel

<img src="haPRoxyAndRedisCluste2 copy.png" />

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

    <p>Things to do:</p>
<ol>
<li>implement redis cluster master/slave replication &lt;br&gt;
<ol style="list-style-type: lower-alpha;">
<li>master node : redis-a: 192.168.1.121 [ad joined, read/write]<br />
<ol style="list-style-type: lower-alpha;">
<li>AD joined</li>
<li>install redis-server
<ol>
<li>update and upgrade</li>
<li>install redis-server</li>
<li>test redis</li>
</ol>
</li>
<li>install redis-sentinel
<ol style="list-style-type: lower-alpha;">
<li>install redis-sentinel</li>
<li>stop sentinel service</li>
</ol>
</li>
<li>set permission for user: 'redis'
<ol style="list-style-type: lower-alpha;">
<li>sentinel conf</li>
<li>sentinel log</li>
<li>allow ports main[6379, 26379], [for app test 22 not required]</li>
</ol>
</li>
<li>configure redis-server
<ol style="list-style-type: lower-alpha;">
<li>bind</li>
<li>port</li>
<li>protected-mode no</li>
<li>restart redis-server</li>
</ol>
</li>
</ol>
</li>
<li>slave node 1 : redis-b: 192.168.1.122 [ad joined, read only]
<ol style="list-style-type: lower-alpha;">
<li>AD joined</li>
<li>install redis-server
<ol style="list-style-type: lower-alpha;">
<li>update and upgrade</li>
<li>install redis-server</li>
<li>test redis</li>
</ol>
</li>
<li>install redis-sentinel
<ol style="list-style-type: lower-alpha;">
<li>install redis-sentinel</li>
<li>stop sentinel service</li>
</ol>
</li>
<li>set permission for user: 'redis'
<ol style="list-style-type: lower-alpha;">
<li>sentinel conf</li>
<li>sentinel log</li>
</ol>
</li>
<li>allow ports main[6379, 26379], [for app test 22 not required]</li>
<li>configure redis-server
<ol style="list-style-type: lower-alpha;">
<li>bind</li>
<li>port</li>
<li>protected-mode no</li>
<li>slaveof master [redis-a: 192.168.1.121]</li>
<li>restart redis-server</li>
</ol>
</li>
</ol>
</li>
<li>slave node 2 : redis-c: 192.168.1.123 [ad joined, read only]<br /> repeat node 1 steps.</li>
<li>implement redis failover using sentinel
<ol style="list-style-type: lower-alpha;">
<li>master node : redis-a: 192.168.1.121
<ol style="list-style-type: lower-alpha;">
<li>configure sentinel.conf
<ol style="list-style-type: lower-alpha;">
<li>bind</li>
<li>protected-mode no</li>
<li>port</li>
<li>monitor mymaster 192.168.1.121</li>
<li>down time</li>
<li>failover time</li>
</ol>
</li>
<li>start with redis-server</li>
<li>reload daemon</li>
<li>estart sentinel</li>
<li>the change by sentinel in sentinel.conf</li>
</ol>
</li>
<li>slave node 1 : redis-b: 192.168.1.122
<ol style="list-style-type: lower-alpha;">
<li>configure sentinel.conf
<ol style="list-style-type: lower-alpha;">
<li>bind</li>
<li>protected-mode no</li>
<li>port</li>
<li>monitor mymaster 192.168.1.121</li>
<li>down time</li>
<li>failover time</li>
</ol>
</li>
<li>start with redis-server</li>
<li>reload daemon</li>
<li>restart sentinel</li>
<li>the change by sentinel in sentinel.conf</li>
</ol>
</li>
<li>slave node 2 : redis-c: 192.168.1.123<br /> repeate node1 steps.</li>
</ol>
</li>
<li>implement haproxy for redis [to identify master]
<ol style="list-style-type: lower-alpha;">
<li>already deployed haproxy</li>
<li>add block for tcp mode for redis with send and expect rules</li>
</ol>
</li>
<li>test from client
<ol style="list-style-type: lower-alpha;">
<li>when redis-b is down -- redis-a promote to master</li>
<li>when redis-a is down -- redis-c promote to master</li>
</ol>
</li>
</ol>
</li>
</ol>

Watch the video@  https://youtu.be/J27AcaVuAPM
 
You can download the config files @

Software used:
1. VM workstation 14 [https://www.vmware.com/products/workstation-pro.html]
2. Bandicam: [https://www.bandicam.com/]
3. Ubuntu 18.04 [https://releases.ubuntu.com/18.04/]
4. Microsoft Server 2016 [https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2016]




#redissentinel
#rediscluster
#redis
