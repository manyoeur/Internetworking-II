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

