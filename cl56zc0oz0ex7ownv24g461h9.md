# OSI Model & Its Layers in Computer Network

# OSI Model
The Open Systems Interconnection (OSI) model defines a networking framework to implement protocols in layers, with control passed from one layer to the following and how information from a software application in one computer moves through a physical medium to the software application in another computer. It consists of **seven layers**, and each layer performs a particular network function.

##  Layer architecture of OSI Model
There are **seven** OSI layers. Each layer has different functions.

1. ### Application Layer
   - The interaction with the user or the user application takes place at this stage.

2. ### Presentation Layer
   - The data is converted into the syntax or semantics that an application understands.
   - Before passing on the data any further, the data is formatted at this stage.
   - Functions including compression, encryption, etc. are also done at this layer of the model.
   - It serves as a data translator for the network.

3. ### Session Layer
    - The connection between the computers connected in a network is managed at this layer.
    - Authentication and authorization happen at this layer.
    - This layer can also terminate or end any session or transmission which is complete.

4. ### Transport Layer
    - The delivery of data packets is managed by the transport layer.

5. ### Network Layer
    - It acts as a network controller.

6. ### Data-Link Layer
    - Access to get the data is achieved at this layer.

7. ### Physical Layer
    - It comprises the raw data which is further transmitted to the higher layers of the structure.


# TCP/IP Model
The OSI Model we just looked at is just a reference/logical model. The TCP/IP model is a concise version of the OSI model also known as the Internet Protocol Suite. It contains **four layers**, unlike **seven layers** in the OSI model.

## Layer architecture of TCP/IP Model
1. Application Layer
1. Transport Layer
1.  Network Layer
1.  Data-Link Layer

## Application Layer

![Blue and Pink Geometric Technology Keynote Presentation.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656789285743/LcDGYVmvo.png align="left")

This layer performs the functions of the top three layers of the OSI model: **Application**, **Presentation**, and **Session Layer**. It is responsible for node-to-node communication and controls user-interface specifications. Some of the protocols present in this layer are HTTP, HTTPS, FTP, TFTP, Telnet, SSH, SMTP, SNMP, NTP, DNS, DHCP, NFS, X Window, LPD.

### Client–server Architecture

![801.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656826566706/qX-ejr0sv.png align="left")
- A server is a system that controls the website you are hosting.
- The Application layer has two parts: The **Client** part and the **Server** part. These are known as processes and they communicate through each other.
- Clients are the ones who are using/consuming these resources like we are making a request to google.
- A collection server is known as a **Datacenter**.
- Datacenter is a collection of a huge number of computers. It may have static IP addresses. They have a good internet connection and high upload speed.

### Peer-to-Peer Architecture

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656828227851/ULacM2yl9.png align="left")
- There is no dedicated server like a Client-server, they are just connected with each other.
- The key advantage is that you can scale it rapidly.
- Every single computer can be named as a client as well as a server.
- It is a kind of decentralized network.

### DNS - Domain Name System

![what-is-dns.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656831701839/F66schbhG.png align="left")

DNS is a hostname for IP address translation service. DNS is a distributed database implemented in a hierarchy of name servers. It is an application layer protocol for message exchange between clients and servers. 
When we type "google.com", HTTP protocol takes that domain name and uses DNS to find the IP address and afterward, it connects to that server.

![image(1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656832241430/e3sd16HoO.png align="left")

#### Top-level domain (TLD) 
It is responsible for com, org, edu, etc, and all top-level country domains like uk, fr, ca, in, etc. They have info about authoritative domain servers and know the names and IP addresses of each authoritative name server for the second-level domains.

#### Domain Name Server

![Figure_6_3_Resolver_DNS_cache.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1656832963572/FPm4omi2p.jpg align="left")

The client machine sends a request to the local name server, which, if the root does not find the address in its database, sends a request to the root name server, which in turn, will route the query to a top-level domain (TLD) or authoritative name server. The root name server can also contain some hostname to IP address mappings. The Top-level domain (TLD) server always knows who the authoritative name server is. So finally the IP address is returned to the local name server which in turn returns the IP address to the host. 

## Transport Layer

![Blue and Pink Geometric Technology Keynote Presentation(4).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656952599859/eA24TdHfo.png align="left")

Transport Layer is the second layer in the TCP/IP model and the fourth layer in the OSI model. It is an end-to-end layer used to deliver messages to a host. It is termed an end-to-end layer because it provides a **point-to-point** connection rather than a **hop-to-hop** (Network Layer), between the source host and destination host to deliver the services reliably. 

- The unit of data encapsulation is a **segment**.
- The transport layer takes services from the Network layer and provides services to the Application layer.
- The transport layer attaches the socket port numbers to the segments.
- Some of the protocols present in this layer are TCP, UDP, SCTP, DCCP, ATP, FCP, RDP, RUDP, SST, SPX.



### Responsibilities of a Transport Layer
- Flow control
- Congestion Control
- Multiplexing and Demultiplexing
- Process to process delivery
- End-to-end Connection between hosts
- Data integrity and Error correction

### Transport Layer Protocols

#### User Datagram Protocol (UDP) 
UDP is a communications protocol that is primarily used to establish low-latency and less-tolerating connections between applications. UDP speeds up transmissions by enabling the transfer of data before an agreement is provided by the receiving party.
That is why it is an unreliable and connectionless protocol.

#####  **UDP Header Composition**

![image(3).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656919006296/fbgCImqFt.png align="left")

The User Datagram Protocol header has four fields, each of which is 2 bytes. They are the following:
- source port number
- destination port number
- length
- checksum

##### **Applications of UDP**
- Lossless data transmission
- Gaming, voice, and video
- Multicasting and routing update protocols
- Services that don't need fixed packet transmission
- Fast applications

#### Transmission Control Protocol (TCP)

![tcp3.PNG](https://cdn.hashnode.com/res/hashnode/image/upload/v1656919726691/2NYA5-enx.PNG align="left")

It lies between the Application and Network Layers. The application layer sends lots of raw data and TCP  segments this data divides into chunks, adds a header, etc. It may also collect the data from the network layer and the small chunks are put into one in the receiving end.

##### Features of TCP
- Segment Numbering System
- Flow Control
- Error Control
- Congestion Control
- Full Duplex (Bi-directional)

## Network Layer

![Blue and Pink Geometric Technology Keynote Presentation(6).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656953307108/jGo0zJJIi.png align="left")

Network Layer is the third layer in the TCP/IP model and the fifth layer in the OSI model. It acts as a network controller. Its main function is to transfer network packets from the source to the destination. It is involved both the source host and the destination host.
It provides a hop-to-hop connection, between the source host and the destination host to deliver the services reliably. The unit of data encapsulation is a segment.

- If the packets are too large for delivery, they are broken down into smaller packets. 
- The source and destination addresses are added to the data packets inside the network layer. 
- It uses routing.

### Responsibilities of a Network Layer
- Flow control
- Congestion Control
- Error Control

### Network Layer Protocol
#### Internet Protocol (IP)
Internet Protocols are a set of rules that governs the communication and exchange of data over the internet. Both the sender and receiver should follow the same protocols in order to communicate the data. 
- It is a **connectionless** and **unreliable protoco**l and to make it reliable and to make it reliable, it must be paired with a reliable protocol such as TCP at the transport layer.
- Internet protocol transmits the data in form of a datagram.

### Control Plane
The control plane is the part of a network that controls how data packets are forwarded - meaning how data is sent from one place to another also can be referred to as the process of creating a routing table.

- Routers are referred to as Nodes.
- Link between routers is referred to as Edges.

### Subnet Masking
- Subnet masking is going to mask the network port of the IP address and leaves us to the host part.
- You can set your subnet length.
- 20.0.0.0./35 - This means the first 35 bits are your subnet part.

### Network Address Translation (NAT)
It is a method of mapping an IP address space into another by modifying network address information in the IP header of packets while they are in transit across a traffic routing device. NAT generally operates on a router or firewall. 

 ## Data-Link Layer

![DatalinkLayer-660x335.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1656950715965/S-lSfaIfG.png align="left")

Data-Link Layer is the fourth layer in TCP/IP model and the sixth layer in the OSI model. It is a node-to-node delivery of data. The data packets that we receive from the network layer. The Data-Link Layer is responsible to send these packets over a physical link.
- The unit of data encapsulation **frames**.
- In Data-Link Layer the devices communicate with each other using ** Data-Link Layer Addresses** also referred as **MAC Addresses**.
- Some of the protocols present in this layer are SDLC, HDLC, SLIP, PPA, LAP, LCP, and NCP.

### Sub-layers of Data Link Layer

- Logical Link Control (LLC)
- Media Access Control (MAC)

### Responsibilities of a Data-Link Layer
-  Access Control
- Flow Control
- Error Control
- Addressing
- Framing




**Thank you so much for taking your valuable time for reading**

Inspired by @[Kunal Kushwaha](@kunalk) and his tutorials and Thank you @[Saiyam Pathak](@Saiyampathak) for the DevOps roadmap, I took the initiative to learn in public and share my work with others. I tried my level best in squeezing as much information as possible in the easiest manner. Hope you learnt something new today :)

Resource Link:
- [Kunal Kushwaha's youtube tutorials](https://youtu.be/7l_n97Mt0ko)

Any feedback for further improvement will be highly appreciated!
























































































