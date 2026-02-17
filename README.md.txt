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

do wr

int range g1/0/1-2
no switchport




int g1/0/1
ip add 172.16.3.145 255.255.255.252
no shut
int g1/0/2
ip add 172.16.3.149 255.255.255.252
no shut






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

hostname admin password cisco
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


exit



##CORE ROUTER 2
en
conf t
hostname CORE-R2
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





##isp-router-1
int se0/3/0
ip add 195.136.17.2 255.255.255.252
no shut

int se0/3/1
ip add 195.136.17.10 255.255.255.252
no shut



##isp-router-2
int se0/3/0
ip add 195.136.17.6 255.255.255.252
no shut

int se0/3/1
ip add 195.136.17.14 255.255.255.252
no shut





