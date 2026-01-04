# Hot Standby Router Protocol (HSRP)
## Lab topology
<img width="1374" height="1054" alt="image" src="https://github.com/user-attachments/assets/592167ea-1c15-4d41-8bc2-12e2361e89b4" />



## Device requirements

* Two Mikrotik
* Two Cisco IOSv router
* One Cisco switch
## Device IP configuration
### R1
```
enable
configure terminal
hostname R1
enable secret cisco
interface GigabitEthernet0/0
 ip address 192.168.1.2 255.255.255.0
interface GigabitEthernet0/1
 ip address 192.168.2.1 255.255.255.0
```
### R2
```
enable
configure terminal
hostname R2
enable secret cisco
interface GigabitEthernet0/0
 ip address 192.168.3.2 255.255.255.0
interface GigabitEthernet0/1
 ip address 192.168.2.2 255.255.255.0
```
## HSRP Configuration
### R1
```
interface GigabitEthernet0/1
 standby 1 ip 192.168.2.254
 standby 1 priority 125
 standby 1 preempt
```
### R2
```
interface GigabitEthernet0/1
 standby 1 ip 192.168.2.254
 standby 1 preempt
```
### On Switch
```
S1(config)#no ip igmp snooping
```
## Add static route both routers
### R1
```
ip route 0.0.0.0 0.0.0.0 192.168.1.1
```
### R2
```
ip route 0.0.0.0 0.0.0.0 192.168.3.1
```
## Test Configuration
### PC1
<pre>
 PC1> tracer 8.8.8.8
trace to 8.8.8.8, 8 hops max, press Ctrl+C to stop
 1   192.168.2.1   5.161 ms  1.136 ms  0.388 ms
 2   192.168.1.1   1.539 ms  0.720 ms  0.596 ms
 3   172.24.224.1   1.526 ms  1.115 ms  0.885 ms
 4   192.168.1.1   13.193 ms  4.469 ms  4.906 ms
 5   10.250.0.1   15.985 ms  18.387 ms  12.674 ms
 6   36.37.255.144   12.404 ms  12.525 ms  24.100 ms
 7   100.99.8.12   13.245 ms  11.778 ms  12.315 ms
 8   100.99.8.11   18.679 ms  25.143 ms  29.854 ms
PC1>

</pre>
Turn off R1 and wait 30 secons then try to tracer in PC1 again the result to change from 192.168.2.1 to 192.168.2.2 as below
<pre>
PC1> tracer 8.8.8.8
trace to 8.8.8.8, 8 hops max, press Ctrl+C to stop
 1   192.168.2.2   1.694 ms  0.811 ms  0.454 ms
 2   192.168.3.1   1.612 ms  1.393 ms  0.614 ms
 3   172.24.224.1   1.522 ms  1.125 ms  1.050 ms
 4   192.168.1.1   4.561 ms  4.532 ms  6.903 ms
 5   10.250.0.1   12.530 ms  12.326 ms  11.451 ms
 6   36.37.255.144   11.381 ms  12.201 ms  11.615 ms
 7     *  *  *
 8   100.99.8.11   14.579 ms  11.956 ms  11.927 ms
</pre>
