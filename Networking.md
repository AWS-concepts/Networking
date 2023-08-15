# Networking

IP Address:
-----------
* This is a unique number given to a device in a network. It has two schemes
  * IPv4
  * IPv6 --> 128 bits -->hexadecimal --> `x:x:x:x:x:x:x:x`
* IPv4 is a 32 bit number organized into 4 octets (8 bits) addressed as x.x.x.x
* Execute ipconfig in command prompt.
![preview](networking88.png)
* ip address = network id + host id
```
ip: 192.168.0.118
sm: 255.255.255.0

--------------------

network id (255): 1,2,3 => 192.168.0
host id (0): 4 => 118
size: bits for host id => 1 octet => 8 => 2^8 - 2 => 256-2 = 254
192.168.0.0 (network id)
192.168.0.255 (broadcast)

ip: 192.168.0.111
sm: 255.255.0.0
---------------------

network id: 1,2 => 192.168
host id: 3,4 => 0.111
size: 2 octects =>16 => 2^16-2 65536-2 = 65534

192.168.0.0 (network id)
192.168.255.255 (broadcast)
```
* Octetes which have 255 in subnetmask is network id and ovctets which have 0's in SM is host id.
* In every network 2 ip addresses can't be used those are for broadcast and network id.
* Default gateway is router ip address. 
  
Network Principle
-----------------
* A Device can communicate directly with other device in same network.
* Route table connected to internet gateway and subnets which are using this route table are public subnets.
  
IP Addressing (Class Based)
---------------------------
* We have classes to define ip ranges
  * Class A
  * Class B
  * Class C
  * Class D
  * Class E
  
CIDR (Classless inter domain routing)
-------------------------------------
```
ip: 192.168.0.118
sm: 255.255.255.0

n (host id) = 1 octet = 8 bits
N (Network id) = 32 - n  = 24 bits
``` 
* Find subnet mask for a network of 500 devices
```
500

2^n - 2 ~= 500

2^n ~= 500

n = 9
N = 32 - 9 = 23

SM: 11111111.11111111.11111110.00000000
    255.255.254.0
```
* Find a subnet mask for a network with 8 devices
```
2^n -2 = 8
2^n ~= 10
n = 4
N = 28
SM: 11111111.11111111.11111111.11110000
     255.255.255.240
```
* In this notation ip is expressed as x.x.x.x/N
```
192.168.0.0/24
N (network id) = 24 bits
n (host id) = 32 - 24 = 8
ip: 192.168.0.x => 192.168.0.0 to 192.168.0.255
SM: 11111111.11111111.11111111.00000000


10.0.0.0/16
N = 16
n = 32 -16 = 16
ip: 10.0.x.x => 10.0.0.0 to 10.0.255.255
SM: 11111111.11111111.00000000.00000000

10.0.0.0/8
N = 8
n = 32-8 = 24
ip: 10.x.x.x => 10.0.0.0 to 10.255.255.255
SM: 11111111.00000000.00000000.00000000


192.168.0.0/22
N = 22
n = 32-22 = 10

ip: 192.168.(0+x).x => 192.168.(0+0).0 to 192.168.(0+3).255
                    => 192.168.0.0 to 192.168.3.255
SM: 11111111.11111111.11111100.00000000

172.16.0.0/26
N = 26
n = 32-26 = 6

ip: 172.16.0.(0+x) => 172.16.0.(0+0) to 172.16.0.(0+63)
                => 172.16.0.0 to 172.16.0.63
SM: 11111111.11111111.11111111.11000000


172.16.0.0/12
N = 12
n = 32-12 = 20
ip: 172.(16+x).x.x => 172.(16+0).0.0 to 172.(16+15).255.255
                => 172.16.0.0 to 172.31.255.255
SM: 11111111.11110000.00000000.00000000


192.168.64.0/22

N = 22
n = 10

ip: 192.168.(64+x).x => 192.168.(64+0).0 to 192.168.(64+3).255
                    => 192.168.64.0 to 192.168.67.255
SM: 11111111.11111111.11111100.00000000


172.16.0.0/15

N = 15
n = 17

ip:  172.(16+x).x.x => 172.16.0.0 to 172.17.255.255
SM: 11111111.11111110.00000000.00000000


192.168.8.0/22

N = 22
n = 10
  
ip: 192.168.(8+x).0 to 192.168.8.0 to 192.168.11.255
SM: 11111111.11111111.11111100.00000000
```
### Private vs Public Network
* Private Network: Network which cannot be accesed directly from internet
* Public Network: Network which can be accessed from internet
* Private network cidr ranges
   * 10.0.0.0/8: 10.0.0.0 to 10.255.255.255
   * 172.16.0.0/12: 172.16.0.0 to 172.31.255.255
   * 192.168.0.0/16: 192.168.0.0 to 192.168.255.255
* 
AWS Global Insfrastructure
--------------------------
1. **Region:** 
   * Geographical loaction idenified by AWS to build its datacenters.
   * It containes availability zones.
   * Every Region has a code `<continent>-<direction>-<number>`
2. **Availability zones:**
   * This is present within a region
   * Each region contains multiple azs.
   * Distance b/w each az can be 30-60kms
3. **Local Zone:**
   * This site built in different parts of the world.
   * Each local zone contains a parent region.
   * This local zone can be added to some marked Regions.
4. **Edge Location:**
   * This acts as point of presence location.
   * They help in establishing direct connectivity with AWS Global network (AWS Direct Connect)
5. **Wavelength Zone:**
   * This was designed for 5G networks.

AWS Networking major components:
--------------------------------
1. VPC
2. Subnets
3. Internet gateway
4. Route Table
5. Network Interface
6. Elastic IP
7. Security Group
8. Network ACL

**1. VPC(Virtual Private Component):**(compared to home network)
  * It is a virtual network which we can create within a AWS region.
  * We can create a private network of required size by using Classless Inter-Domain Routing (CIDR).
  * Every region has a default VPC.
  * max 5 vpcs per region
  
**2. Subnet:**
  * Subnet (sub networks) are parts of larger network.
  * A part in larger network is subnet.
  * AWS Subnet is subnet of VPC.
  * Which is created within the Availability zone.
  * We can create a resource and connect to the subnet.
  * Size is expressed in CIDR.
  * 200 subnets per vpc.
  * The smallest subnet in AWS is /28 and largest is /8
  * CIDRS can be expanded

**3. Internet Gateway:**(compared to modem)
  * This give dual connectivity which means VPC can access Internet and Internet can access resources connected to VPC.
  * If we want only one way connection then AWS have eggress only internet gateway.(Egress= outgoing, ingress= incoming)

**4. Route Table:**(compared to router)
  * This will act as Router.
  * When VPC is created Route table is created by default.
  * By default AWS will allow connections b/w all the subnets with in the VPC.
  * Router is a device which can transfer packets from one network to other.
  
**5. Network interface:**
  * Elastic network interface which assigns a private ip and private dns name to any resource connected (ec2)
  * A device is connected to the network using Network interface.
  * The ip that the network interface recieves can be used to access the device/system/server
  * A device can have multiple network interfaces
  * Network interfaces gets an ip address assigned to it by DHCP (Dynamic Host Configuration Protocol) server
  * In your home networks, wifi routers have built in DHCP which assigns the ip
  * DHCP can assign dynamic ip or static ip depending on configuration.

Security Group and Network ACL:
-------------------------------
* Security groups and Network ACLs can filter the network traffic.
* Security group operates at network interface level(instance level), where as Network ACL operates at subnet level
  ![preview](networking26.webp)

**Network ACL:**
  * when a vpc is create a default NACL will be created.
  * The default nacl rules allow all incoming and outgoing traffic.
  
**Seurity group** 
  * Rules around elastic network interface for incoming traffic (ingress) and outgoing traffic (egress)
  * The default rule is deny all.
  * Depending on what we need to allow we create rules.

Network ACL:
------------
* The following fields exist for every rule.
    * Rule Number
    * Protocol
    * Port
    * Source/Destination
    * Allow/Deny
* whenever a packet is recieved; the inbound rules  are evaluated in the order of priority.
* whenever a packet is sent; the rules in outbound are evaluated in the order of priority
```
  100  TCP  22  0.0.0.0/0  Deny
  110  TCP  22  192.168.0.0/16  Allow
  *     *    *       *        Deny



src: 3.4.5.6
dest: 192.168.0.39

http (tcp 80) => denied
ssh (tcp 22)  => denied

src: 192.168.1.42
dest: 192.168.0.39

ssh  denied



100  TCP  22  192.168.0.0/16  Allow
110  TCP  22  0.0.0.0/0  Deny
*     *    *       *        Deny



src: 3.4.5.6
dest: 192.168.0.39

http (tcp 80) => denied
ssh (tcp 22)  => denied

src: 192.168.1.42
dest: 192.168.0.39

ssh  Allowed
```
* Lets create a NACL which allows all communication within vpc and all external communication to anywhere. Allows only 22 and 80 port from anywhere.
![preview](networking27.png)
![preview](networking28.png)
![preview](networking29.png)
![preview](networking30.png)
![preview](networking31.png)
* To add subnets association to NACL we need to explicitly associate.
![preview](networking32.png)

Activity-1:
----------
* Create a VPC with web, app and db subnets.
* Ensure web subnet allows incoming
    * within vpc on any traffic
    * external sources for only 22, 80, 443
* Ensure db and app subnets allow incoming within vpc on any traffic no external communication is allowed.
![preview](networking33.png)
![preview](networking34.png)
![preview](networking35.png)

Security Groups:
----------------
* We write only Allow rules.
* 
Activity-2:
-----------
* Created 1 VPC, 6 Subnets
* While creating a vpc we get default route table .
* create a ec2 using our vpc and one of the subnets.
* And login into the ec2 and install apache2 and see the page.(it won't show the page).
* Create an internet gateway and attach it to the dedault route table.
* Now try to access apache page. it show.
* before it didn't because there is no internet access to theresoureces created within the network we created after attaching internet gateway to the route table then we get acsess to the internet. 

Public and Private Subnets:
---------------------------
* Public subnets is associated with route table which has route to internet gateway.
* Private subnets is associated with route table which as no route to internet gateway.
* Subnets will use default route table if there is no explicit connection.
* Route table connected to internet gateway and subnets which are using this route table are public subnets.
  
Activity-3:
-----------
* Created a vpc and 3 subnets (1 public, 1 private, 1 default)
* Created 2 routetables (1 privatert and 1 publicrt)
* created 1 internet gateway and attach internetgateway to publicrt by selecting route in publicrt .
* In publicrt select subnet association and add public subnet.
* In privatert select subnet association and add private subnet.
* And created 1 public ec2 with public subnet and 1 private ec2 with private subnet.(created these instances using import key process keypair )(In class sir created using pem key which is downloaded in local machine)
* Tried to connect private ec2(it won't connect).
* I tried to connect private ec2 from public ec2 because within the network we can access the private ec2 by login through public ec2 by sending id_rsa and id_rsa.pub from local machine to public ec2.
* Changed the file permissions `chmod 400 id_rsa id_rsa.pub`
* Then login into public ec2 and try to login into private ec2 with private ip of private ec2.(getting Permission denied (publickey) )
* Then tried to give command `ssh -i /home/ubuntu/id_rsa ubuntu@<privateip>`
![preview](networking1.png)
* Even we can login into private ec2 through public ec2 but we can't get access to the internet.
* Private subnets unable access internet.

Egress only Internet Gateway:
-----------------------------
* If all your subnets are private and if they need internet access, then we can use egress-only internet gateway.
  
NAT Gateway:(Network Address Translation)
-----------------------------------------
* NAT gateway is used to provide internet access to private subnets.
* AWS provides NAT in two ways:
    * **NAT Instance:** It is a EC2 machine which you put in public subnet and then we configure router to forward   the packets but if NAT instance goes down your private subnets won't get internet access.
    * A NAT instance provides NAT. You can use a NAT instance to allow resources in a private subnet to communicate with destinations outside the virtual private cloud (VPC), such as the internet or an on-premises network. The resources in the private subnet can initiate outbound IPv4 traffic to the internet, but they can't receive inbound traffic initiated on the internet.
    ![preview](networking2.png)
    * **NAT Gateway:** A NAT gateway is NAT service. You can use a NAT gateway so that instances in a private subnet can connect to services outside your VPC but external services cannot initiate a connection with those instances.
* NAT gateway requires an elastic ip and should be present in public subnet and router to the private subnets should have a route to the NAT

Activity-4:
-----------
* Create an VPC with 1 private and 1 public subnets.
* have ec2 instances for private and public subnets.
* Now create a NAT gateway in public subnet.
* While creating NAT gateway we should select public subnet and allocate elastic ip.
* After creating NAT gateway we should edit route in private route table.
* After that log into the private instance through public instance. and sudo apt update and try ping google.com we can see the result that private instance can access the internet same as our home network.
![preview](networking3.png) 

VPC Peering
-----------
* VPC Peering allows private communication b/w two VPCs belonging to different regions or different accounts.
* The destination network(vpc) should approve the peering request then in two VPCs peering connection objects will be created.
* Rules for connecting are there should not be overlapping cidr ranges.
* VPC Peering work flow.
  ![preview](networking17.webp)
* We can ping instance by using there public ips by using internet but its not safe. So we have to ping each other instance with private ips because we need private based communication.
  ![preview](networking4.png)
  ![preview](networking5.png)
* For that we are approaching VPC peering.
  ![preview](networking6.png)
  ![preview](networking7.png)
  ![preview](networking8.png)
  ![preview](networking9.png)
  ![preview](networking10.png)
  ![preview](networking11.png)
  ![preview](networking12.png)
* Now since there is infra to communicate, now modify route tables to forward packets to each other.
  ![preview](networking15.png)
  ![preview](networking16.png)
* Now ping from one ec2 to other using private ip
  ![preview](networking13.png)
  ![preview](networking14.png)
* Peering connection can work b/w only two networks not more than two.
* If we have connection b/w A & B networks and connection b/w B & C that doesn't mean we have connection b/w A & C.

Site to Site VPN(Virtual Private Network):
------------------------------------------
* VPN is used to connect two virtual netwoks where there CIDR range should not collide.
* There will be 2 VPN severs each in HQ and Branch office. there is an IP Secure Tunnel which transfers the packages which were logically encrypted b/w HQ nad Branch office. There will be encryption and decryption mechanism to secure the data.
* VPN Gateway can establish gateway b/w only one VPC where as Trasit Gateway will help you to communicate b/w multiple VPCS in the same region.
* Site to site VPN overview:
  ![preview](networking18.webp)
* Site to site VPN in AWS:
   ![preview](networking19.webp)
* Multiple VPCs in one region to on-prem:
  ![preview](networking20.webp)
* Multiple VPCs to multiple datacenters on-prem:
  ![preview](networking21.webp)

AWS VPC Endpoints:
------------------
* [referhere](https://docs.aws.amazon.com/whitepapers/latest/aws-privatelink/what-are-vpc-endpoints.html) for more detailed info about VPC Endpoints.
* VPC endpoints are virtual devices. They allow communication between instances in an Amazon VPC and services without imposing availability risks or bandwidth constraints on network traffic.
* There are 2 types:
    1. Interface Endpoints
    2. Gateway Endpoints
* If we want to access S3 or dynamo db from an instance within the private network(VPC) then we access it through internet then S3/Dynamo DB we exposed publicaly. To avoid that we use network interface which will have a private ip and we communicate s3/dynamo db by creating VPC interface endpoints.
* **Interface Endpoints** will allow to communicate with AWS services using global network and gives an elastic network interface in subnet.
 ![preview](networking22.webp)
* An interface endpoint is a collection of one or more elastic network interfaces with a private IP address that serves as an entry point for traffic destined to a supported service.
* **Gateway Endpoints** allows you to route the traffic to access a particular service using Gateway.
  ![preview](networking23.png)
* If you want a private conection for a custom service (email, temperature) then use private link.
  ![preview](networking24.webp)

Enabling DNS Names in VPC
--------------------------

* Create a VPC with atleast one public subnet.
* Create an ec2 instance in public subnet, ensure the security group rule is open for 22 and 80
* By default in vpc the Public DNS Names are not enabled.
  ![preview](networking36.png)
  ![preview](networking38.png)
* To enable public DNS names.
  ![preview](networking37.png)
  ![preview](networking39.png)

### DHCP Options Set
* DHCP Options set is used to set the DNS Servers in VPC.
* When a company wants to use thier own DNS rather than AWS provided DNS.
 ![preview](networking40.png)
 ![preview](networking41.png)
 ![preview](networking42.png)

Activity-5:Create an ec2 instance with some predefined private ip.
------------------------------------------------------------------
* Create a Network interface in the subnet (zone) where your ec2 instance is running.
* Choose Custom for private ip address.
* Attach this network interface to your ec2 instance.
  ![preview](networking43.png)
  ![preview](networking44.png)
  ![preview](networking45.png)
  ![preview](networking46.png)
  ![preview](networking47.png)
  ![preview](networking48.png)
  ![preview](networking49.png)
### Lab Prep
* Create an ec2 instance and ensure you execute the following steps.
```
sudo apt update
sudo apt install apache2 stress -y
sudo apt install php libapache2-mod-php php-mysql -y
sudo -i 
echo "<?php phpinfo(); ?>" > /var/www/html/info.php
```
* Navigate to `http://publicip/info.php`.
  ![preview](networking53.png)
* Create an AMI.
    ![preview](networking54.png)
    ![preview](networking50.png)
    ![preview](networking51.png)
    ![preview](networking52.png)
* After creating the AMI delete the EC2 Instance.
### Load balancing
* Load balancing in cloud is dynamic in nature.

#### OSI Model of Networking:
* It has 7 layers.
  ![preview](networking55.webp)
* Layers & Protocols
  ![preview](networking56.png)

#### Layer4 and Layer7 Loadbalancing
* Load Balancing can be done at Layer 4 and Layer 7
* Layer 4:
   * Aware: IP, Port, TCP/UDP, MAC
* Layer 7:
  * Aware: IP, Port, TCP/UDP, MAC, HTTP, SSL/TLS (security)

#### Loadbalancers in AWS:
* In AWS we have following loadbalancers:
   * AWS Classic Loadbalancer: Both L4 and L7 loadbalancing.
   * AWS Application Loadbalancer: works on L4 loadbalancing.
   * AWS Network Loadbalancer: works on L7 Loadbalancing.
#### AWS LoadBalancing options in single region:
* Layer 4 Load Balancing using Network load balancer
* Layer 7 load balancing uing ALB using path based Routing 
#### Layer 4 loadbalancing:
* AWS has Network Load Balancer which can perform layer 4 load balancing
* Create a VPC with 2 subnets in 2 different zones.
* Create an EC2 intances in subnet1 and subnet2.
* Ensure 22 and 80 ports are open for all.
![preview](networking57.png)
![preview](networking58.png)
![preview](networking59.png)
![preview](networking60.png)
![preview](networking61.png)
![preview](networking62.png)
![preview](networking63.png)
![preview](networking64.png)
![preview](networking65.png)
![preview](networking66.png)
![preview](networking67.png)
![preview](networking68.png)
![preview](networking69.png)
![preview](networking70.png)
![preview](networking71.png)
![preview](networking72.png)
#### Layer 7 loadbalancing:
* AWS has Application Load Balancer which can perform layer 7 load balancing.
* login into web1 and execute the following
  ```
  # web1
  # root
  mkdir -p /var/www/html/images
  echo "<h1>images</h1>" > /var/www/html/images/index.html
  ```
* access the application by using `http://publicip/images/index.html`
![preview](networking74.png)
* Login into web2 and execute the following.
  ```
  # web2
  # root
  mkdir -p /var/www/html/music
  echo "<h1>music</h1>" > /var/www/html/music/index.html
  ```
*  access the application by using `http://publicip/music/index.html`
![preview](networking73.png)
* http health checks:
   * Status Codes
![preview](networking75.webp)
  * Interval: how frequently load balancer will perform health checks?
  * Healthy threshold: How many consecutive health checks should be passed to consider the instance healthy
  * UnHeathy threshold: How many consecutive health checks should be failed to consider the instance unhealthy
* Target Group for every application component running independently i.e. we will be creating two target groups images and music
* Lets create a Application Load Balancer.
   * Create 2 target groups for 2 instances 1 for images and other for music.
![preview](networking76.png) 
   * After completion of creating select the default target group for loadbalancer.
   * Path Based Routing: [Refer Here](https://repost.aws/knowledge-center/elb-achieve-path-based-routing-alb)
   * Manage rules in listeners.
![preview](networking77.png)
![preview](networking78.png)
![preview](networking79.png)
![preview](networking80.png)
![preview](networking81.png)
![preview](networking82.png)
* Similarly create other rule for images.
![preview](networking83.png)
![preview](networking84.png)
![preview](networking85.png)

#### Multiple regions
* Copy AMI image from soeul to other region
* Create same infrastructure in that region
* Create two vpcs in two regions and ensure we have ec2 instances with some application running on http.
* Create a Network/Application Load Balancer for each region.
![preview](networking86.png)
![preview](networking87.png)

### DNS (Domain Naming System):
* DNS provides services which includes names to ip addresses (Domain names)
* DNS Servers can be
   * public
   * internal
* DNS servers are used to store records mapping domain/host names to ip addresses.
* Servers which store these records are called as Name Servers.
* DNS Servers will have different record types
   * A Record: name to ip mapping
   * CName Record: alias to name
  * other record types [Refer Here](https://www.site24x7.com/learn/dns-record-types.html)
* 
* DNS can be setup on client system
   * windows: open `c:\windows\system32\drivers\etc\hosts` and entries
   * other: open `/etc/hosts` and entries
* DNS Servers will have DNS zones
* DNS servers work on port 53
* DNS servers which are used for public access are hosted by organizations and they are generally called as hosted zones
* Domains can be purchased by many third parties, In aws also we can register domains.
* For multi region solutions we use Route 53 Routing policies [Refer Here](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html) 
