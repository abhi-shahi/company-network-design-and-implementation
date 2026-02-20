##SALE switch
en
conf t
hostname SALE-sw
line console 0
password cisco
login
exit

enable pass cisco
no ip domain lookup
banner motd @@ no unauthorized access@@
service password-encryption


vlan 10
name sales
vlan 402
name blackhole
exit

int range fa0/3-24
sw mo acc
sw acc vlan 10

int range g0/1-2
sw mo acc
sw acc vlan 402
shutdown


int range fa0/1-2
sw mo tr


##hr switch
en
conf t
hostname hr-sw
line console 0
password cisco
login
exit

enable pass cisco
no ip domain lookup
banner motd @@ no unauthorized access@@
service password-encryption

vlan 20
name HR
vlan 402
name blackhole
exit

int range fa0/3-24
sw mo acc
sw acc vlan 20

int range g0/1-2
sw mo acc
sw acc vlan 402
shutdown

int range fa0/1-2
sw mo tr


 
##finance switch
en
conf t
hostname finance-sw
line console 0
password cisco
login
exit

enable pass cisco
no ip domain lookup
banner motd @@ no unauthorized access@@
service password-encryption

vlan 30
name FINANCE
vlan 402
name blackhole
exit

int range fa0/3-24
sw mo acc
sw acc vlan 30
switchport port-security
switchport port-security maximum 1
switchport port-security mac-address sticky
switchport port-security violation shutdown

int range g0/1-2
sw mo acc
sw acc vlan 402
shutdown

int range fa0/1-2
sw mo tr

int fa0/5
no switchport port-security
no switchport port-security maximum 1
no switchport port-security mac-address sticky
no switchport port-security violation shutdown





##admin switch
en
conf t
hostname admin-sw
line console 0
password cisco
login
exit

enable pass cisco
no ip domain lookup
banner motd @@ no unauthorized access@@
service password-encryption

vlan 40
name ADMIN
vlan 402
name blackhole
exit

int range fa0/3-24
sw mo acc
sw acc vlan 40

int range g0/1-2
sw mo acc
sw acc vlan 402
shutdown

int range fa0/1-2
sw mo tr




##ict switch
en
conf t
hostname ICT-sw
line console 0
password cisco
login
exit

enable pass cisco
no ip domain lookup
banner motd @@ no unauthorized access@@
service password-encryption

vlan 50
name ICT
vlan 402
name blackhole
exit

int range fa0/3-24
sw mo acc
sw acc vlan 50

int range g0/1-2
sw mo acc
sw acc vlan 402
shutdown

int range fa0/1-2
sw mo tr




##SRV switch
en
conf t
hostname SRV-sw
line console 0
password cisco
login
exit

enable pass cisco
no ip domain lookup
banner motd @@ no unauthorized access@@
service password-encryption

vlan 60
name SRV-ROOM
vlan 402
name blackhole
exit

int range fa0/3-24
sw mo acc
sw acc vlan 60

int range g0/1-2
sw mo acc
sw acc vlan 402
shutdown

int range fa0/1-2
sw mo tr

!!static ip to server room devices

dhcp: 172.16.3.130 255.255.255.240
default gateway 172.16.3.129 dns:172.16.3.131

admin pc: 172.16.3.132 255.255.255.240
default gateway 172.16.3.129 dns: 172.16.3.131

dns server:172.16.3.131 255.255.255.240
default gateway 172.16.3.129









##MULTILAYER-SW-1
en
conf t
hostname MULTILAYER-SW-1
line console 0
password cisco
login
exit

enable pass cisco
no ip domain lookup
banner motd @@ no unauthorized access@@
service password-encryption

hostname admin password cisco
ip domain-name cisco.net
crypto key generate rsa
1024

line vty 0 15
login local 
transport input ssh 
exit

ip ssh ver 2

int range g1/0/3-8
sw mo tr
vlan 10
name sales 
vlan 20
name HR
vlan 30
name FINANCE
vlan 40 
name ADMIN
vlan 50 
name ICT
vlan 60
name SRV-ROOM



int range g1/0/1-2
no switchport




int g1/0/1
ip add 172.16.3.145 255.255.255.252
no shut
int g1/0/2
ip add 172.16.3.149 255.255.255.252
no shut


!!ospf routing
ip routing
router ospf 10
router-id 2.2.2.2
network 172.16.1.0 0.0.0.127 area 0
network 172.16.1.128 0.0.0.127 area 0
network 172.16.2.0 0.0.0.127 area 0
network 172.16.2.128 0.0.0.127 area 0
network 172.16.3.0 0.0.0.15 area 0
network 172.16.3.128 0.0.0.15 area 0

network 172.16.3.144 0.0.0.3 area 0
network 172.16.3.148 0.0.0.3 area 0

do wr



int vlan 10
ip add 172.16.1.1 255.255.255.128
ip helper-address 172.16.3.130

int vlan 20
ip add 172.16.1.129 255.255.255.128
ip helper-address 172.16.3.130

int vlan 30
ip add 172.16.2.1 255.255.255.128
ip helper-address 172.16.3.130

int vlan 40
ip add 172.16.1.129 255.255.255.128
ip helper-address 172.16.3.130

int vlan 50
ip add 172.16.3.1 255.255.255.128
ip helper-address 172.16.3.130

int vlan 60
ip add 172.16.3.129 255.255.255.240
ip helper-address 172.16.3.130

ip route 0.0.0.0 0.0.0.0 gig1/0/1
ip route 0.0.0.0 0.0.0.0 gig1/0/2 70
do wr

!!ssh trouble shoot as login local was not working.There was issue is with local username authentication.

conf t
line vty 0 15
no login local
password test123
login
end






##MULTILAYER-SW-2
en
conf t
hostname MULTILAYER-SW-2
line console 0
passwo
rd cisco
login
exit

enable pass cisco
no ip domain lookup
banner motd @@ no unauthorized access@@
service password-encryption

hostname admin password cisco
ip domain-name cisco.net
crypto key generate rsa
1024

line vty 0 15
login local 
transport input ssh 
exit
ip ssh ver 2

int range g1/0/1-2
no switchport

int g1/0/1
ip add 172.16.3.153 255.255.255.252
no shut
int g1/0/2
ip add 172.16.3.157 255.255.255.252
no shut

!!ospf config
ip routing
router ospf 10
router-id 1.1.1.1
network 172.16.1.0 0.0.0.127 area 0
network 172.16.1.128 0.0.0.127 area 0
network 172.16.2.0 0.0.0.127 area 0
network 172.16.2.128 0.0.0.127 area 0
network 172.16.3.0 0.0.0.15 area 0
network 172.16.3.128 0.0.0.15 area 0

network 172.16.3.152 0.0.0.3 area 0
network 172.16.3.156 0.0.0.3 area 0

do wr

ip route 0.0.0.0 0.0.0.0 gig1/0/2
ip route 0.0.0.0 0.0.0.0 gig1/0/1 70



!!ssh trouble shoot as login local was not working.There was issue is with local username authentication.

conf t
line vty 0 15
no login local
password test123
login
end






##CORE ROUTER 1
en
conf t
hostname CORE-R1
line console 0
passwo
rd cisco
login
exit

enable pass cisco
no ip domain lookup
banner motd @@ no unauthorized access@@
service password-encryption

username admin password cisco
ip domain-name cisco.net
crypto key generate rsa
1024

line vty 0 15
login local 
transport input ssh 
exit

int g0/0
ip add 172.16.3.146 255.255.255.252
no shut

int g0/1
ip add 172.16.3.154 255.255.255.252
no shut

int se0/3/0
ip add 195.136.17.1 255.255.255.252
no shut
int se0/3/1
ip add 195.136.17.5 255.255.255.252
no shut

!!ospf on corerouter-1
ip routing
router ospf 10
router id 3.3.3.3


network 172.16.3.144 0.0.0.3 area 0
network 172.16.3.152 0.0.0.3 area 0

network 195.136.17.0 0.0.0.3 area 0
network 195.136.17.4 0.0.0.3 area 0
do wr

exit
access-list 1 permit 172.16.1.0 0.0.0.127
access-list 1 permit 172.16.1.128 0.0.0.127
access-list 1 permit 172.16.2.0 0.0.0.127
access-list 1 permit 172.16.2.128 0.0.0.127
access-list 1 permit 172.16.3.0 0.0.0.127
access-list 1 permit 172.16.3.128 0.0.0.15



IP NAT inside source list 1 int s0/3/0 overload
ip nat inside source list 1 int s0/3/1 overload

int g0/0
ip nat inside
int g0/1
ip nat inside

int s0/3/0
ip nat outside
int s0/3/1
ip nat outside

do wr

ip route 0.0.0.0 0.0.0.0 se 0/3/0
ip route 0.0.0.0 0.0.0.0 se 0/3/1 70



##CORE ROUTER 2
en
conf t
hostname CORE-R2
line console 0
password cisco
login
exit

enable pass cisco
no ip domain lookup
banner motd @@ no unauthorized access@@
service password-encryption

username admin password cisco
ip domain-name cisco.net
crypto key generate rsa
1024

line vty 0 15
login local 
transport input ssh 
exit

int g0/0
ip add 172.16.3.150 255.255.255.252
no shut

int g0/1
ip add 172.16.3.158 255.255.255.252
no shut

int se0/3/0
ip add 195.136.17.9 255.255.255.252
no shut
int se0/3/1
ip add 195.136.17.13 255.255.255.252
no shut

exit

router ospf 10
router id 4.4.4.4


network 172.16.3.148 0.0.0.3 area 0
network 172.16.3.156 0.0.0.3 area 0

network 195.136.17.8 0.0.0.3 area 0
network 195.136.17.12 0.0.0.3 area 0
do wr

access-list 1 permit 172.16.1.0 0.0.0.127
access-list 1 permit 172.16.1.128 0.0.0.127
access-list 1 permit 172.16.2.0 0.0.0.127
access-list 1 permit 172.16.2.128 0.0.0.127
access-list 1 permit 172.16.3.0 0.0.0.127
access-list 1 permit 172.16.3.128 0.0.0.15



IP NAT inside source list 1 int s0/3/0 overload
ip nat inside source list 1 int s0/3/1 overload

int g0/0
ip nat inside
int g0/1
ip nat inside

int s0/3/0
ip nat outside
int s0/3/1
ip nat outside

do wr


ip route 0.0.0.0 0.0.0.0 se 0/3/1
ip route 0.0.0.0 0.0.0.0 se 0/3/0 70
do wr

!!ssh trouble shoot as login local was not working.There was issue is with local username authentication.
conf t
line vty 0 15
no login local
password test123
login
end





##isp-router-1
int se0/3/0
ip add 195.136.17.2 255.255.255.252
no shut

int se0/3/1
ip add 195.136.17.10 255.255.255.252
no shut

router ospf 10
router id 7.7.7.7
int se0/3/0
ip ospf 10 area 0
int se0/3/1
ip ospf 10 area 0
do wr




##isp-router-2
int se0/3/0
ip add 195.136.17.6 255.255.255.252
no shut

int se0/3/1
ip add 195.136.17.14 255.255.255.252
no shut

do wr

router ospf 10
router id 8.8.8.8
int se0/3/0
ip ospf 10 area 0
int se0/3/1
ip ospf 10 area 0
do wr






