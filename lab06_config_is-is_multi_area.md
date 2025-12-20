# Configure IS-IS
## I. Network topology
<img width="1608" height="612" alt="image" src="https://github.com/user-attachments/assets/eeec5614-df27-4467-a1e2-615e541b8469" />


# Lab prerequisite
In this lab you will need:
* GNS3
* Cisco IOSv ( you can download here [link](https://drive.google.com/drive/folders/13J-2qJd0hueUc6vDdouA8ZVSIBcX6im-?usp=sharing) if you don't have)

## II. Basic configuration
### 1. Set hostname and enable secret password to R1
```
enable
configure terminal
hostname R1
enable secret cisco
```
### 2. Set hostname and enable secret password to R2
```
enable
configure terminal
hostname R2
enable secret cisco
```
### 3. Set hostname and enable secret password to R3
```
enable
configure terminal
hostname R3
enable secret cisco
```
### 4. Set hostname and enable secret password to R4
```
enable
configure terminal
hostname R4
enable secret cisco
```
### 5. Set hostname and enable secret password to R5
```
enable
configure terminal
hostname R5
enable secret cisco
```
## III. Assign IP Address to Routers
### 1. Assign IP address to R1
```
interface Gig 0/0
 no shutdown
 description R1 to R2
 ip address 12.12.12.1 255.255.255.252
 ipv6 address 2001:db8:acad:1212::1/64
interface Gig0/1
 no shutdown
 description R1 to PC1
 ip address 192.168.1.1 255.255.255.0
 ipv6 address 2001:db8:acad:1111::1/64
```
### 1. Assign IP address to R2
```
interface Gig 0/0
 no shutdown
 description R2 to R1
 ip address 12.12.12.2 255.255.255.252
 ipv6 address 2001:db8:acad:1212::2/64
interface Gig0/1
 no shutdown
 description R2 to  R3
 ip address 23.23.23.1 255.255.255.252
 ipv6 address 2001:db8:acad:2323::1/64
interface Gig0/2
 no shutdown
 description R2 to  PC2
 ip address 192.168.2.1 255.255.255.0
 ipv6 address 2001:db8:acad:2::1/64
```
### 3. Assign IP address to R3
```
interface Gig 0/1
 no shutdown
 description R3 to R2
 ip address 23.23.23.2 255.255.255.252
 ipv6 address 2001:db8:acad:23.23::2/64
interface Gig0/0
 no shutdown
 description R3 to  R4
 ip address 34.34.34.1 255.255.255.252
 ipv6 address 2001:db8:acad:3434::1/64
```
### 4. Assign IP address to R4
```
interface Gig 0/0
 no shutdown
 description R4 to R3
 ip address 34.34.34.2 255.255.255.252
 ipv6 address 2001:db8:acad:3434::2/64
interface Gig0/1
 no shutdown
 description R4 to  R5
 ip address 45.45.45.1 255.255.255.252
 ipv6 address 2001:db8:acad:4545::1/64
interface Gig0/2
 no shutdown
 description R4 to  PC4
 ip address 192.168.4.1 255.255.255.0
 ipv6 address 2001:db8:acad:4::1/64
```
### Assign ip address to R5
```
interface Gig 0/0
 no shutdown
 description R5 to R4
 ip address 45.45.45.2 255.255.255.252
 ipv6 address 2001:db8:acad:4545::2/64
interface Gig0/1
 no shutdown
 description R5 to  PC5
 ip address 192.168.5.1 255.255.255.0
 ipv6 address 2001:db8:acad:5::1/64
```
## IV. Configure IS-IS Protocol
### R1
```
router isis
net 49.0012.0000.0000.0001.00
exit
interface gig 0/0
 ip router isis bbu_c101
 isis circuit-type level-1
interface gig 0/1
 ip router isis
 isis circuit-type level-1
end
write
```
### R2
```
router isis bbu_c101
net 49.0012.0000.0000.0002.00
exit
interface gig 0/0
 ip router isis
 isis circuit-type level-1
interface gig 0/1
 ip router isis
 isis circuit-type level-2-only
interface gig 0/2
 ip router isis
 isis circuit-type level-1
end
write
```
### R3
```
router isis
net 49.0003.0000.0000.0003.00
is-type level-1-2
exit
interface gig 0/0
 ip router isis
interface gig 0/1
 ip router isis
end
write
```
### R4
```
router isis
net 49.0045.0000.0000.0004.00
exit
interface gig 0/0
 ip router isis
 isis circuit-type level-2-only
interface gig 0/1
 ip router isis
 isis circuit-type level-1
interface gig 0/2
 ip router isis
 isis circuit-type level-1
end
write
```
### R5
```
router isis
net 49.0045.0000.0000.0005.00
exit
interface gig 0/0
 ip router isis
 isis circuit-type level-1
interface gig 0/1
 ip router isis
 isis circuit-type level-1
end
write
```
## V. Verify IS-IS Configuration
```
R1(config-if)#do show ip route isis
```

Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
       D - EIGRP, EX - EIGRP external, O - OSPF, IA - OSPF inter area
       N1 - OSPF NSSA external type 1, N2 - OSPF NSSA external type 2
       E1 - OSPF external type 1, E2 - OSPF external type 2
       i - IS-IS, su - IS-IS summary, L1 - IS-IS level-1, L2 - IS-IS level-2
       ia - IS-IS inter area, * - candidate default, U - per-user static route
       o - ODR, P - periodic downloaded static route, H - NHRP, l - LISP
       a - application route
       + - replicated route, % - next hop override, p - overrides from PfR

Gateway of last resort is 12.12.12.2 to network 0.0.0.0

i*L1  0.0.0.0/0 [115/10] via 12.12.12.2, 00:04:10, GigabitEthernet0/0
i L1  192.168.2.0/24 [115/20] via 12.12.12.2, 00:05:26, GigabitEthernet0/0
