#How to setup custom dns server in aws  VPC and configure it so that EMR in private subnet gets ip`s from THIS  DNS server INSTEAD of AmazonProvodedNS service 

.) We will be using an EC2 instance and configuring it to, work as DNS server

.) First of all, launch an ec2 instance in your public subnet in  vpc ( say vpc CIDR is  10.0.0.0/16 in private subnet  CIDR = 10.0.1.0/28 and public subnet CIDR= 10.0.0.0/24 ).  Use appropriate security groups, so that u can ssh it from your terminal and also open port 53 udp for DNS Queries to reach to it.)

.)Now, go to VPC->DHCP OPTIONS SET, create  a new DHCP OPTIONS Set with Name Tag = "xman dhcp option set" , Domain Name ="xman.local", Domain Name Server ="10.0.0.145, AmazonProvidedDNS"

.) In VPC sectino do the following task as well
	1) selet VPC ->click Attch button -> dhcp option set ->select "xman  dhcp option set"
	2) selet VPC ->click Attch button -> DNSHostames -> set no
	3)  selet VPC ->click Attch button ->DNSresolution->set yes

.)Create a s3 endpoint and attach it to private subnet"s routing table of this vpc(as emr needs an s3 endoint )
.)Create a nat instance in public subnet and attach it to private subnet's routing table.Also verify you have internet gateway attached to main routing table or public subnet routing table.


.)SSH into instance and check internet connectivity.
.)create a bind Server with following cmd:
$ sudo apt-get update
$ sudo apt-get install bind9 bind9utils bind9-doc

.)Now edit the following three files as shown 
	1) /etc/named.conf
	2) /var/named/xman.zone
	3) /var/named/reverse_xman.zone
NOTE:
In zone files entery ALL ip's of private subnet to be mentioned ....as shown.

.)After this run $ systemctl restart named.service


""""IF THIS GIVES NO ERROR YOU R  DONE WIH SETTING UP CUSTOM DNS SERVER FOR EMR IN PRIAVTE SUBNET """
LAUNCH EMR IN THIS PRIVATE SUBNET WITH LESS THAN 11 NODES AND YOUR EMR WILL HAVE IP LIKE IP-XX-XX-XX-XX.xman.local



....
..
.
!
