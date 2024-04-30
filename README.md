# Networking-Basics
# Networking Basic Notes

# Network Sharing(Not that important part)

## File Sharing on same network

1. when you are connected to same network you can use scp cmd.
2. One simple file sharing tool is the **scp** command. The scp command stands for secure copy, it works exactly the way the cp command does, but allows you to copy from one host over to another host on the same network. It works via SSH so all your actions are using the same authentication and security as SSH.

**To copy a file over from local host to a remote host**

```
$ scp myfile.txt username@remotehost.com:/remote/directory
```

**To copy a file from a remote host to your local host**

```
$ scp username@remotehost.com:/remote/directory/myfile.txt /local/directory
```

**To copy over a directory from your local host to a remote host**

```
$ scp -r mydir username@remotehost.com:/remote/directory
```

## Simple HTTP Server

Python has a super useful tool for serving files over HTTP. This is great if you just want to create a quick network share that other machines on your network can access. To do that just go to the directory you want to share and run:

```
$ python -m SimpleHTTPServer 8080(ANY PORT)
```

## NFS(Network File System)

NFS allows a server to share directories and files with one or more clients over the network.

**Setting up NFS client**

```
$ sudo service nfsclient start

$ sudo mount server:/directory /mount_directory
```

## Samba

In the early days of computing, it became necessary for Windows machines to share files with Linux machines, thus the Server Message Block (SMB) protocol was born.

SMB was used for sharing files between Windows operating systems (Mac also has file sharing with SMB) and then it was later cleaned up and optimized in the form of the Common Internet File System (CIFS) protocol.

**Install Samba**

```
$ sudo apt update

$ sudo apt install samba
```

# Network Basics

## OSI Model(Open System Interconnection)

### **TCP/IP Model**

- **Application Layer :**

> This layer uses:
> 
> - HTTP (Hypertext Transfer Protocol) - used for the webpages on the Internet.
> - SMTP (Simple Mail Transfer Protocol) - electronic mail (email) transmission
- **Transport Layer :**

> Here How your data will be transmitted includes correct checking ports, delivering packets. This layer uses:
> 
> - TCP (Transmission Control Protocol) - reliable data delivery
> - UDP (User Datagram Protocol) - unreliable data delivery
- **Network Layer :**

> This layer specifies how packets will move between hosts and network. This layer uses,
> 
> - IP (Internet Protocol) - Helps route packets from one machine to another.
> - ICMP (Internet Control Message Protocol) - Helps tell us what is going on, such as error messages and debugging information.
- **Link Layer**

> This layer specifies how to send data across a physical piece of hardware. Such as data travelling through Ethernet, fiber, etc.
> 

## Network Addressing

### Application layer:

we will see how data travels through tcp/ip model with imagination now

so when we send data through our email client suppose your client is Gmail, so you are sending mail through Gmail after hitting send the data packet will go to application layer where it will encapsulate the data and it will contact transport layer through specific port and through that port data will be transferred. Data is sent to transportation layer to be encapsulated into segments

### Transport Layer:

it helps in transfer our data in way network can read data. it breaks data into chunks and then transfers, it gets easy as you send data into segments as they transports, the chunks will be put together again. In addition to segments transport layer also adds source and destination to the data packets so while receiving packets, it will know what ports to use(at receiver)

TCP handshake.

- The client (connecting process) sends a SYN segment to the server to request a connection
- Server sends the client a SYN-ACK segment to acknowledge the client's connection request
- Client sends an ACK to the server to acknowledge the server's connection request

Once this connection is established, data can be exchanged over a TCP connection. The data is sent over in different segments and are tracked with TCP sequence numbers so they can be arranged in the correct order when they are delivered

### Link Layer

In the link layer our packet is encapsulated into somethoing called frame and then again this layer attaches the source MAC address to the packet and also destination MAC address but the question is how does the internet knows receivers mac Address cz its not on INTERNET??

Here COMES ARP in PICTURE!

### ARP(Address Resolution Protocol)\

Address resolution protocol finds the mac address associated with IP address if the receiver is not on SAME network we could use ROUTING system. In routing system we find the ip of next router that would receive the packet since it is in same network we use ARP

to determine arp system first looks at ARE-LOOKUP table if it is not there then it uses ARP

so to determine MAC address ARP tool broadcasts message on network (to all the hosts present on network)  saying who is 10.10.14.1 then if the host with ip 10.10.14.1 present in a network it replies with an ARP packet which contains IP address with MAC address

now that all the data we have like ip address and mac address our link layer forwards this frame through our network interface card!

### So Here’s how data will travel though all the layers as follow in simplified version

1. Pete sends Patty an email: this data gets sent to the transport layer.
2. The transport layer encapsulates the data into a TCP or UDP header to form a segment, the segment attaches the destination and source TCP or UDP port, then the segment is sent to the network layer.
3. The network layer encapsulates the TCP segment inside an IP packet, it attaches the source and destination IP address. Then routes the packet to the link layer.
4. The packet then reaches Pete's physical hardware and gets encapsulated in a frame. The source and destination MAC address get added to the frame.
5. Patty's receives this data frame through her physical layer and checks each frame for data integrity, then de-encapsulates the frame contents and sends the IP packet to the network layer.
6. The network layer reads the packet to find the source and destination IP that was previously attached. It checks if its IP is the same as the destination IP, which it is! It de-encapsulates the packet and sends the segment to the transport layer.
7. The transport layer de-encapsulates the segments, checks the TCP or UDP port numbers and makes a connection to the application layer based on those port numbers.
8. The application layer receives the data from the transport layer on the port that was specified and presents it to Patty in the form of the final email message

### DHCP(Dynamic Host Configuration Protocol)

It basically assigns IP address, Subnet Mask and Getway's to our machine

for example, let's say you have a cell phone and you want to get a cell phone number to start talking to people. You have to call up your phone carrier and they will give you a number. As long as your pay your bills you can keep using your phone. DHCP is the phone carrier in this case, it gives you an IP address so that you can talk to other IP addresses

DHCP is great for lot of reasons it does not let network administrator worry about assigning ip address to machine it also prevents from assigning duplicate ip address, every physical network should have its own DHCP server in home our ROUTER acts as a DHCP server which assigns IP address to our machine!

The way DHCP gets all your dynamic host information is:

1. DHCP DISCOVER - This message is broadcasted to search for a DHCP server.
2. DHCP OFFER - The DHCP server in the network replies with an offer message. The offer contains a packet with DHCP lease time, subnet mask, IP address, etc.
3. DHCP REQUEST - The client sends out another broadcast to let all DHCP servers know which offer it accepted.
4. DHCP ACK - Acknowledgement is sent by the server.
