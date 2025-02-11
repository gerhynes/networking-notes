

### Sources
- [Tech Fundamentals](https://learn.cantrill.io/p/tech-fundamentals)
## OSI Model
The OSI(Open Systems Interconnection) model is a conceptual framework that describes the communication functions or a telecommunication or computer system.

The model is divided into seven layers, each of which performs a specific function in the transmission of data between networked devices.

These layers are designed to provide a standardized approach to network communication, allowing different types of devices to communicate with each other regardless of their underlying hardware or software.

- Layer 6 - Presentation
- Layer 5 - Session
- Layer 4 - Transport
- Layer 3 - Network
- Layer 2 - Data Link
- Layer 1 - Physical

Layers 1,2 and 3 are the Media layers. These deal with how data is moved between points A and B.

Layers 4,5,6 and 7 are the Host layers. These deal with how the data is divided and reassembled for transport, and how it's formatted to be understood by both sides of a network connection.

### Layer 1 - Physical
The physical medium can be copper (electrical), fibre (light), or WiFi (radio frequencies). 

Layer 1 specifications define the transmission and reception of raw Bit Streams between a device and a shared physical medium. It defines things like voltage level, timing, rates, distances, modulation, and connectors.

If a device is at a specific layer, it only contains functionality for that layer and below. So a Layer 1 device just understands layer 1.

A hub allows for more devices to be connected simultaneously. Anything which the hub receives on any of its ports is retransmitted to all of the other ports, including any errors or collisions.

At layer 1, there are no individual device addresses, all data is processed by all devices. It's a broadcast medium.

If multiple devices transmit at once, a collision will occur. Layer 1 has no media access control and no collision detection.

A layer 1 network has one broadcast and one collision domain, and so is not scalable. It's still fundamental to networking, since it's how devices communicate at a physical level.

### Layer 2 - Data Link
Layer 2 is responsible for the reliable transfer of data between two nodes on the same network. This layer defines protocols for the access and use of the physical network, such as addressing, flow control, and error detection and correction.

Layer 2 introduces a unique hardware address, a MAC address, for every device on a network. The address is uniquely assigned to a specific piece of hardware.

A MAC address is formed of two parts: OUI (organizationally unique identifier) assigned to companies that manufacture networked devices, and NIC (network interface controller). A MAC address should be globally unique.

Frames are a format for sending information over a Layer 2 network.  A frame has the following components:
- the Preamble (56 bits) and Start Frame Delimiter (8 bits), allows devices to know it's the start of a frame
- Destination and Source MAC addresses
- EtherType (16 bits) specifies which Layer 3 protocol put data in the frame
- Payload (46-1500 bytes) contains the data the frame is sending, generally provided by Layer 3, such as IP packet
- Frame Check Sequence (32 bits) used to identify any errors in the frame

Destination MAC address, Source MAC address and EtherType make up the MAC Header.

Encapsulation is the process of taking some data and wrapping it in something else, in this case a frame. As data passes down the OSI model it's encapsulated in more and more different components.

Layer 2 contains collision detection. If a collision is detected, then a jam signal is sent by all the devices that detected it, and a random backoff occurs.

At Layer 2, a Switch takes the place of a Layer 1 Hub, maintaining a MAC address table, tracking what's connected to each of its ports. Switches are intelligent, interpreting the frames and making forwarding decisions based on the source and destination MAC addresses. A Switch won't forward collisions, and so collisions are limited to one port.

### Layer 3 - Network
Layer 3 is responsible for the end-to-end delivery of data between devices on different network segments.

Using only Layer 2, only those local area networks joined by direct point to point link using the same Layer 2 protocol could communicate.

Internet Protocol (IP) is a Layer 3 protocol which adds cross-network IP addressing and routing to move data between local area networks without direct P2P links.

IP packets are moved by routers (L3 devices) step by step from source to destination via intermediate networks, encapsulated in different frames along the way.

With an IP packet, the source and destination addresses could be on opposite sides of the planet. Packets remain the same as they move between networks but they are places inside different frames.

There are two versions of the Internet Protocol in use. Version 4 has been around for decades. Version 6 adds more scalability.

An IP packet contains:
- Source IP address
- Destination IP address
- Protocol field
- TTL (time to live) - how many hops the packet can loop through
- Data - from Layer 4
- And other fields

An IP address, such as 133.33.3.7, has a **network** part (133.33), which states which network the address belongs to, and a **host** part (3.7), which represents hosts on that network.

If the network part of two addresses matches, they're on the same network. If not, they're on different networks.

An IP address is 32 bits (4 sets of 8 bits, or octets). The first 16 bits are for the network and the second are for hosts.

IP addresses are assigned using Dynamic Host Configuration Protocol (DHCP), or humans. IP addresses need to be unique, both on the local network and globally.

Subnet masks allow a host to determine if an IP address it needs to communicate with is local or remote, which influences if it needs to use a gateway or can communicate locally. On your local network, your router is the default gateway.

A subnet mask is configured on a host device in addition to an IP address, such as 255.255.0.0. A subnet mask is a dotted decimal version of a binary number which indicates which part of an IP address is Network (1) and which part is Host (0). The starting address of the local network is all 0s for the Host part and the ending is all 1s in the Host part.

Every router will have at least one route table. A route table is a collection of routes, with a destination field and next hop/target field. The router compares each packet's destination IP and route table for matching destinations. More specific prefixes are preferred (0 lowest, 32 highest). The packet is forwarded in to the next hop/target. Packets are routed, hop by hop, across the internet from source to destination.

The Address Resolution Protocol (ARP) is generally used when you have a Layer 3 packet that you want to encapsulate inside a frame and send to a MAC address. You will need a protocol that can find a MAC address for an IP Address.

ARP performs a Layer 2 broadcast of all frames asking who has a specific IP address. The receiving device uses ARP to respond with the MAC address for the device in question.

Packets can be delivered out of order. They may not take the same route from source to destination.

### Layer 4 - Transport
Layer 4, the Transport layer, is responsible for ensuring that data is delivered reliably and efficiently from one device to another. It does this by establishing connections between applications on different devices, and managing flow control and error recovery.

Several features are mixed between the Transport and Session layers.

Layer 4 address several of the issues with Layer 3. At Layer 3, each packet is routed independently. Routing decisions are per packet and different routes can result in out of order packets at the destination since Layer 3 provides no ordering mechanism.

Packets can also be lost en route since Layer 3 communication is not guaranteed to be reliable. Per packet routing can introduce delays to packets en route. Different packets can experience different delays.

With Layer 3 there are no communication channels. Packets have a source and destination IP but no method of splitting by APP or Channel. 

IP also has no flow control. If the source transmits faster than the destination can receive it can saturate the destination, causing packet loss.

Layer 4 defines two main protocols: the Transmission Control Protocol (TCP) and the User Datagram Protocol (UDP). Both of these run on top of IP and add additional features.

You would pick TCP when you want reliability, error correction and ordering of data. It is slower but more reliable and is used by important protocols such as HTTP, HTTPS, SSH and more.

TCP is a connection-oriented protocol. You set up a connection between two devices and once set up it creates a bidirectional channel of communication.

UDP is faster because it doesn't have the TCP overhead required for the reliable delivery of data. This makes it less reliable.

TCP Segments are like frames and packets. They are encapsulated within IP packets. Segments don't have source or destination IP addresses because they use those of the packets.

TCP Segments add source and destination ports and this gives the combined TCP/IP protocol the ability to have multiple streams of conversations at the same time between two devices.

Within the segment is a unique sequence number, which is incremented with each segment that's sent. This allows the TCP segments to be correctly ordered once received. 

The acknowledgement field is the way that one side can indicate that it has received up and including a certain sequence number. Every segment that's received has to be acknowledged.

The TCP window defines the number of bytes that you indicate you're willing to receive between acknowledgements. Once reached, the sender will pause until you acknowledge that amount of data.

The checksum is used for error checking. It means that the TCP layer is able to detect errors and can arrange for retransmission of data. The urgent pointer lets one type of segment take priority, such as for control traffic.

All of the above fields are in the TCP header. The rest of the segment is used for data.

TCP is a connection-based protocol. A connection is established between two devices using an arbitrary (ephemeral) port on a client and a known port on a server. Once established, the connection is bi-directional. The connection is a reliable communication channel provided via the segments encapsulated in IP packets.

TCP gives you a set of reliable, ordered segments between two devices. This can be conceptualized as a connection or channel but it's really two sets of segments, from client to server and from server to client.

The TCP Flags field contains flags that can be set to alter the connection, such as FIN to close, ACK for acknowledgement, SYN to synchronize sequence numbers.

A TCP connection is established using a three-way handshake. A client needs to send a segment to the server. This contains a random sequence number, unique in this direction of travel for segments. The segment has SYN sequence set to this Initial Sequence Number (ISN). The server picks its own random ISN sequence and sends a segment with SYN and ACK. ACK is set to the ISN + 1, that is saying it has received up to ISN, now send ISN + 1. This type of segment is called a SYN ACK, its used to synchronize sequence numbers but also acknowledge receipt of the client sequence number. The client then sends a segment with ACK set to the server's sequence number + 1. It then increments its own client sequence number by 1 and puts that as the sequence before sending this data in an ACK segment. Client and server are now synchronized and data can flow. Every time they send data, they increment the sequence number and acknowledge the sequence number + 1.

There are two types of capability levels from a security perspective. A stateless firewall (such as an AWS Network Access Control List ACL) doesn't understand the state of a connection. You need two rules. A rule allowing the outbound segments and another rule allowing the inbound response segments. If you wanted to secure a web server, you need to secure both inbound and outbound traffic. 

A stateful firewall understand the state of the TCP segments. It views the initial traffic and the response traffic as one thing. If you allow the initial connect, you automatically allow the response. In AWS this is how a Security Group works.

### Network Address Translation (NAT)
Network Address Translation (NAT) is a fundamental technology in computer networking that allows multiple devices to share a single public IP address. 

NAT is designed to address the growing shortage of IPv4 addresses. NAT also provides some additional security benefits. 

NAT translates private IPv4 addresses into public IPv4 addresses.

- **Static NAT** - 1 private to 1 (fixed) public address (IGW) 
- **Dynamic NAT** - 1 private to 1st available public address
- **Port Address Translation (PAT)** - many private to 1 public (NATGW)

Since IPv6 adds so many more addresses, we don't need any form of private addressing or translation.

With static NAT, the router ( a NAT device) maintains a NAT table, which maps 1 private IP to 1 public IP. Packets are generated as normal with a private source IP and an external destination IP. If a public IP has been allocated to the private IP, the source address of the packet is translated as it passes through the NAT device. This results in a new packet with a valid public IP address as the source. This process works similarly in both directions. Inbound packets are checked, and if a matching entry is found in the NAT table, the public IP is translated into the matching private IP.

With dynamic NAT, devices aren't allocated a permanent public IP. Instead they're allocated one temporarily from a pool as required. Multiple devices can use the same allocation as long as thee is no overlap. Only one private IP will be mapped to the public IP at any time. It's still a 1:1 mapping for the duration of the allocation.

Port Address Translation (PAT) is what allows a large number of private devices to share one public IP address. It's how the AWS NATGateway (NATGW) functions, with a many:1 private to public architecture. It's also how your home router works. PAT uses both IP addresses and ports to allow multiple devices to share the same public IP. The NAT device records the source IP and source port. It replaces the source IP with the single public IP and a public source port allocated from a pool, which allows IP overloading (many to one).

#### IP Addressing
The IPv4 standard (RFC791) has been around since 1981. It supports c.4.294 billion IP addresses. Originally managed by the Internet Assigned Numbers Authority (IANA). More recently, parts of the address space have been delegated to regional authorities.

All public IPv4 addressing is allocated. Part of the address space is private and can be used and reused freely.

Private IP addresses are defined in the standards document RFC1918. This defines three ranges of IPv4 addresses which you're free to use internally (they can't be routed across the internet):

10,0,0,0 - 10.255.255.255 - 1 Class A Network, 16.7 million IPv4 addresses generally used in cloud environments and divided up into smaller subnetworks.

172.16.0.0 - 172.31.255.255 - 16 Class B Networks, 16 x 65,536 IPv4 addresses. Used for the default VPC in AWS.

192.168.0.0 - 192.168.255.255 - 256 Class C Networks, 256 x 256 IPv4 addresses.

The IPv4 address space provides 4.294 billion addresses, which is less than one per person on earth. This is a difficulty for providers with huge IP addresses requirements.

By contrast, the IPv6 address space provides 340 trillion, trillion, trillion addresses. 

#### Subnetting
Subnetting is the process of breaking networks up into smaller and smaller pieces.

Classless Inter-Domain Routing or CIDR lets you take networks and break them down. It expresses the size of a network via a prefix, for example a /16 network, 10.16.0.0/16. The larger the prefix value, the smaller the network.

A single /16 network can be split into two /17 networks, from 10.16.0.0 to 10.16.127.255 and 10.16.128.0 t0 10.16.255.255. This can then be repeated as /18 networks.

Networks are usually split into 2, 4, 8 smaller networks. While unusual, odd number splits are valid.

The process is: split the network, increment the prefix, subnet 1 starts at the start, subnet 2 starts midway.

#### Distributed Denial of Service (DDoS) Attacks
A DDoS attack is a type of cyber attack where multiple compromised devices, often from different locations, are used to flood a targeted website or network with traffic, causing the site or network to become unavailable to its users.

Since it is distributed, it is hard to block individual IPs/ranges.

DDoS attacks tend to be either:
- Application attacks - HTTP floods. These take advantage of the imbalance of processing between client and server (requesting is easier than constructing a response).
- Protocol attacks - SYN flood. This takes advantage of the connection based nature of requests. SYN floods spoof a source IP address and initiate the connection attempt with the server. The server tries to respond but will time out, since the source IP is spoofed, and consume resources.
- Volumetric attacks - DNS amplification This relies on how certain protocols such as DNS only take small amounts of data to make the request but can deliver a large amount of data in response.

DDoS attacks often involve large armies of compromised machines (botnets).

Web servers are normally provisioned based on normal load plus a buffer or are built to auto scale. The site's hosting environment and the public internet are connected via a data connection with a certain capacity, a certain amount of data or number of connections. The site's users establish TCP connections with the frontend servers. Connections are created, data is transferred and once finished connections are terminated.

In an application layer attack, an attacker controls a network of compromised devices (botnet) via a control location (often using a VPN to disguise their real location). Application layer attacks exploit the fact that requests are cheap for clients to make but computationally expensive for servers to deliver. A flood of requests can overwhelm a server. Legitimate users of the web app are prevented from accessing the site because they have to compete with the attack for access. The performance of the application is reduced to failure levels.

A protocol based attack follows the same basic architecture. A botnet generates a huge number of spoofed SYNs (connection initiations). The server sees these as normal and sends SYN-ACKs back to the spoofed IPs. The SYN-ACKs will be ignored and the connections will stay in a hung state consuming networks resources and the servers won't be able to service legitimate connections.

A volumetric or amplification attack can use a much smaller botnet, which exploits a protocol where a response is significantly larger than the request. In this case making a spoofed request to DNS. The DNS servers respond to the spoofed IP, the frontend servers of our application, which is overwhelmed by the amount of data, preventing legitimate customers from accessing the service.

#### VLANs, TRUNKS and QinQ
A Virtual Local Area Network (VLAN) is a way to divide a single physical network into multiple logical networks. They are commonly used to improve performance, security and manageability. Trunks are a way to carry VLAN traffic between network switches, while QinQ isa  VLAN stacking technique that allows multiple VLAN tags to be added to a single Ethernet frame.

802.1Q changes the format of a standard ethernet frame by adding a new 32 bit field. This is used for the VLANID (VID). Any 802.1Q frame can be a member of more than 4,000 VLANs. 802.1Q allows multiple VLANS to operate over the same L2 physical network. Each has a separate broadcast domain and is isolated from all others.

802.1AD (QinQ, Provider Bridging, Stacked VLANs) adds another VLAN field. The first is the S-Tag (service tag), the second is the C-Tag (customer tag). QinQ allows ISPs or carriers to use VLANs across their network, while carrying customer traffic which might also be using multiple VLANs.

A Trunk or Trunk Port is a connection between two 802.1Q capable devices. It carries all VIDs with tagging.

VLANs let you create separate L2 network segments. Traffic is isolated in these VLANs until you configure a router between them. VLANs offer separate broadcast domains.

#### Decimal to Binary Conversion (IP Addressing)
IP addresses are typically represented in decimal notation, with four decimal numbers separated by periods, such as 133.33.33.7. This is actually a 32 bit binary number, four sets of 8 bits. So a computer sees 10000101.00100001.00100001.00000111

#### SSL and TLS
Secure Socket Layer (SSL) and Transport Layer Security (TLS) are two cryptographic protocols used to provide secure communication over the internet. 

SSL and TLS provide privacy and data integrity between client and server. They ensure client-server communications are encrypted. They use a combination of symmetric and asymmetric encryption to encrypt data, ensuring that information transmitted over the internet is secure and cannot be intercepted by unauthorized parties. They verify the identity of the server you're connecting to. Finally, they provide a reliable connection protecting against alteration of data in transit.

When a client initiates communication with a sever and TLS is used, there are three main stages to initiate secure communication:
- cipher suites are agreed
- authentication happens
- keys are exchanged

TLS begins with an established TCP connection. A cipher suite is a set of protocols used by TLS, including a key exchange algorithm, a bulk encryption algorithm, and a message authentication code algorithm or MAC. The client and server have to agree on a cipher suite to use.

The client does a Client Hello. This contains:
- SSL/TSL version
- list of supported cipher suites
- Session ID
- extensions

The server responds with a Server Hello, including:
- SSL/TLS version
- a cipher suite from the client list
- Server Certificate - includes the server's public key

The public key can be used to encrypt data which only the server can decrypt using its private key.

During the authentication phase, the client ensures that the server certificate is authentic, verifying the server as legitimate. Previously, the server generated a public/private key pair and a Certificate Signing request (CSR) and a Public Certificate Authority (CA), trusted by your OS and browser, generated a signed certificate. Your OS/browser can verify that the CA signed the certificate. 

During authentication, the client validates that  the server certificate it received in the Server Hello was signed by a trusted CA, that the certificate hasn't expired, and that the DNS name matches the name on the certificate. The client verifies that the server has the private key by sending it some random data encrypted with the public key from the certificate.

In the key exchange phase, we move from asymmetric to symmetric keys in a secure way and begin the encryption process. This makes it much easier computationally to encrypt and decrypt data at high speeds.

Starting with a valid public key and matching private key on the server, the client generates a pre-master key, encrypts this using the server's public key, and sends it through to the server. The server decrypts this with its private key and now both sides have the same pre-master key.

Both sides use the same process on the same pre-master key to generate the master secret which is used to generate the ongoing sessions keys which encrypt and decrypt data. The master secret is used over the lifetime of the connection to create many session keys, which are sued to encrypt and decrypt data.

Both sides confirm the handshake and from then on communication between the client and server is encrypted.

#### Border Gateway Protocol (BGP)
Border Gateway Protocol (BGP) is routing protocol used to exchange routing information between different networks on the internet. It's the protocol that enables the internet to function as a global network of interconnected networks and is responsible for choosing the best path for network traffic to follow from one network to another.

BGP as a system is made up of lots of self-managing networks, known as Autonomous Systems (AS), routers controlled by one entity. Autonomous Systems are black boxes which abstract away the inner details and only concern themselves with network routing in and out of the AS.

Autonomous System Numbers (ASNs) are the way that BDP identifies different entities within the network. ASNs are unique and allocated by IANA (0-65536), 64512-65534 are private.

BGP is designed to be reliable and distributed, operates over TCP/179, and includes error correction and flow control. It's not automatic and peering needs to be manually configured.

BGP is a path-vector protocol. It exchanges the best path and destination between peers. The path is called the ASPATH.

iBGP Internal BGP - routing within an AS
eBGP External BGP - routing between ASs

BGP defaults to the shortest path even if a longer path would give better performance. An AS will advertise all the shortest paths it knows to all of its peers. The AS prepends its own ASN onto the path. This creates a source to destination path which BGP routers can learn and propagate. AS path prepending can be used to artificially make a slower path look longer, making the more performant path preferred. 

#### Stateful vs Stateless Firewalls
Firewalls are an essential component of network security that help protect against unauthorized access and attack. There are two primary types of firewalls: stateful and stateless. 

A stateful firewall is designed to monitor and track the state of network connections, keeping track of the status of individual network sessions to make more informed decisions about what network traffic to allow or deny. 

A stateless firewall examines each individual network packet in isolation and makes decisions based on predetermined rules, without any awareness of the state of the network connection.

Every HTTPS (TCP/443) connection has two parts, a request and response. From the client's perspective the request is an outbound connection, from the server's perspective it's an inbound connection. And the response is the inverse, outbound for the server and inbound for the client. 

For a stateless firewall, it views the request and response as two connections, requiring two firewall rules (1 in, 1 out). For a stateful firewall, it identifies the request and response components of a connection as being related, since the ports and IPs are the same. It only needs to decide whether to allow the request, and if it does, then the response is automatically allowed. This reduces admin overhead the the risk of mistakes.

#### Jumbo Frames and MTU
Jumbo Frames are a network configuration option that allows for the transmission of larger-than-standard Ethernet frames over a network. Traditional ethernet frames have a maximum transmission unit (MTU) of 1,500 bytes but Jumbo Frames can allow for frames up to 9,000 bytes. 

Using Jumbo Frames can improve network performance by reducing the overhead associated with transmitting data, as larger frames result in fewer packets and less network congestion.

- Traffic outside of a single VPC - doesn't support jumbo frames
- Traffic over an inter-region VPC peering connection - doesn't support
- Same region peering - does support
- Traffic over VPN connections - doesn't support
- Traffic over an internet gateway doesn't support
- Direct connect - does support
- TGW - does support (up to 8,500 bytes)

#### Application (Layer 7) Firewalls
Layer 7 firewalls (application firewalls) are a type of firewall that provides advanced security features by inspecting and filtering data at the application level of the OSI model. 

Traditional firewalls (at layers 3,4,5), such as packet filtering or stateful inspection firewalls, operate at the network and transport layers and are only capable of filtering traffic based on IP addresses, port numbers, and protocol types. 

By contrast, layer 7 firewalls have the ability to analyze the content of network traffic, including application protocols such as HTTP, FTP, and SMTP, and can make granular decisions about which traffic should be allowed or blocked.

#### IP Sec VPN Fundamentals
IPSEC (Internet Protocol Security) is a suite of protocols used to secure IP communications by authenticating and encrypting each IP packet. It sets up secure tunnels across insecure networks between two peers (local and remote)

IPSEC is widely used to secure Virtual Private Networks (VPNs), remote access connections, and other types of network traffic.

Secure tunnels are created and torn down as required for Interesting Traffic (defined by certain rules). Although they use the public internet for transit, the data inside the tunnels is encrypted and secure.

IPSEC has two main phases:
- Internet Key Exchange (IKE) Phase 1 (slow and heavy)
	- protocol for how keys are exchanged
	- uses asymmetric encryption to agree on and create shared symmetric key
	- IKE SA created (phase 1 tunnel)
- IKE Phase 2 (faster and more agile)
	- uses the keys agreed in phase 1
	- agree encryption method and keys used for bulk data transfer
	- creates IPSEC SA phase 2 tunnel (architecturally running over phase 1)

Policy-based VPNs use rule sets to match traffic. Based off this, the traffic is sent over a pair of SAs (one for each direction of traffic).

Route-based VPNs use target matching (based on prefixes).

#### Fibre Optic Cable
Fibre optic cables use a thin glass/plastic core surrounded by protective layers. The core is about the diameter of a human hair. The cables transit light over the cable. This allows the cable to cover much larger distances and achieve much higher speeds than copper cables (Tbits/s).

Fibre optic cables are resistant to EMI and less prone to water ingress. Fibre usually offers a more consistent experience than copper.

Cable choice directly influences distance, speed and features. Connectors influence what hardware the cable can be connected to and can limit or influence the transmission characteristics. 

Fibre diameter is expressed as x/y, the diameter of the core and the diameter of the cladding both in microns, such as 9/125.

The cladding is a boundary surrounding the core to contain the light, allowing data to move through the length of the core.

The buffer is a combination of internal coating and strengthening fibres designed to absorb shock and provide additional strength for the core(s). This is surrounded by a cable jacket.

Single mode fibre generally has a very small core, uses yellow jackets, and supports one single ray of light. The light, from a laser, follows a relatively straight path down the core resulting in less distortion. It is good for long distances and can achieve excellent speeds over long distances.

Multimode cable uses a bigger core, supporting a wider range of wavelengths of light at the same time. It usually has an orange or aqua jacket. Over short distances it is faster but has more distortion over distance. 

Fibre optic transceivers, known as SFP transceiver modules (Single form-factor pluggable (SFP) or MINI GBIC), generates and sends or receives light to and from fibre. These can be multi-mode or single-mode, and are optimized for cable type. 

### Security
#### Encryption
Encryption is the process of converting data into a form that is unreadable to unauthorized users. It is essential to protecting the confidentiality and integrity of data both in transit and at rest. In transit encryption is used to protect data as it travels across networks, while at rest encryption is used to protect data stored on physical or digital storage media.

Plaintext just means unencrypted data. It doesn't have to be text, It could be documents, images or applications. An algorithm (such as blowfish, AES, RC4) takes plaintext and a key and produces ciphertext.

Encryption can be achieved using symmetric or asymmetric keys. Symmetric encryption uses the same key for both encryption and decryption, while asymmetric encryption uses a pair of public and private keys for encryption and decryption respectively. Symmetric encryption is generally used when only one party is involved, such as you encrypting your laptop. Asymmetric encryption is generally used when two or more parties are involved to get around the security difficulties of sharing the same key between parties.

Digital signing is a technique that uses asymmetric keys to verify the authenticity and integrity of data, where you can confirm that the expected public key was used.

Steganography is the practice of hiding information within other information. For example, sending an encrypted message might invite unwanted attention from authoritarian governments and so you might want to hide your ciphertext inside a plaintext image.

#### Envelope Encryption
Envelope Encryption is a technique used to secure data by encrypting it with multiple layers of keys. A symmetric data encryption key is generated for each piece of data or user, and it is then encrypted with a separate key known as the key encryption key (KEK). The KEK is typically an asymmetric key, allowing for greater security and flexibility in key management. The encrypted data and encrypted data encryption key (DEK) are then stored separately, providing an additional layer of security.

DEKs are almost always symmetric keys because it is far faster to encrypt data with a symmetric key. KEKs can be symmetric or asymmetric. One KEK could be used to generate millions of DEKs, each of them used to encrypt a single object.

Asymmetric keys are flexible. The public part is public and can be widely distributed. But they are also slow.

Symmetric keys are fast but difficult to securely move.

Envelope encryption can be the best of both if you use asymmetric KEKs. Symmetric keys are generally used to encrypt/decrypt when speed is a priority, and these can normally be secured with asymmetric keys for flexibility.

#### Hardware Security Modules
Hardware Security Modules (HSMs) are physical devices designed to provide a high level of security and cryptographic processing to protect sensitive data and keys. 

HSMs are used to safeguard digital certificates, encryption keys, and other cryptographic secrets. They provide tamper-resistant protection against unauthorized access, physical attacks, and other forms of tampering. 

HSMs are often used in high-security environments, such as banking, government, and military applications, where the protection of sensitive data is critical.

Data is sent to the HSM for encryption or decryption. Requests are made to generate data encryption keys or to sign data. All operations happen on the HSM. Since authentication takes place inside the device there is an isolated security blast radius. Keys are stored securely inside a secure enclave. Keys never leave and operations are performed solely on the HSM.

HSMs are tamper-proof and are hardened against physical or logical attacks. Access is via industry-standard APIs, such as PKCS#11, Java Cryptography Extension (JCE), Microsoft CryptoNG (CNG) libraries.

SSL/TLS processing can be offloaded to the HSM to be more secure and efficient. HSMs can also be used for PKI Signing certificates.

#### Hash Functions and Hashing
Hash functions are algorithms that transform input data into a fixed-length string of character, a hash or message digest. Hashing is the process of applying a hash function to data to produce a unique and irreversible representation of the original data.

Hash functions (such as MD5 and SHA2-256) are widely used in computer security and cryptography for data integrity and authentication, digital signatures, password storage, and more. The main characteristics of a hash function are its one-way property, where it is easy to compute a hash value given the input data but computationally infeasible to reconstruct the original data from the hash value, and its collision resistance, where it is highly unlikely for two different inputs to produce the same hash value. 

#### Digital Signatures
Digital signatures are electronic signatures used to authenticate the integrity and authenticity of digital messages or documents. They provide a way to ensure that the content of the message or document has not been tampered with and that the sender of the message is who they claim to be. 

Digital signatures are created using cryptographic algorithms and are based on public-key cryptography. The digital signature is a mathematical calculation that is unique to the message or document and the sender's private key. 

Data is encrypted using the public key and can only be decrypted using the corresponding private key. The private key can be used to sign something and this signature can then be verified using the public key.

Digital signing
- verifies the integrity (what) and authenticity (who) of the data
- a hash of the data is taken, the original data remains unaltered (integrity)
- this allows normal use without worrying about hashing/keys
- digitally signing the hash (using a private key) authenticates the hash
- the public key can be widely distributed and trusted
- the hash cannot be changed as nobody else has the private key
- the original document cannot be changed as this would invalidate the hash

### DNS and DNSSEC
To communicate with a web application, your computer and any networking in between you and the server, needs the IP address of the web app's server. The Domain Name System (DNS) links human-readable names to IP addresses. 

Many large scale failures on the internet are caused by DNS failures or badly implemented DNS infrastructure. 

#### DNS Architecture
Since virtually everyone using the internet needs to use DNS, there is a huge scaling problem. DNS is a huge database of ~341 million domains.

- DNS Zone, think of this like a database of records like `*.netflix.com`
- ZoneFile, the file storing the zone on disk
- Name Servers (NS), a DNS server which host one or more zones, storing ZoneFiles
- Authoritative, contains real/genuine records
- Non-authoritative/cached, copies of records/zones stored elsewhere to speed things up

DNS follows a hierarchical design.

DNS starts with the DNS Root, hosted on the DNS name servers. The DNS root zone runs on the DNS root servers. Every DNS client knows about and trusts it.

There are 13 root server IP addresses which host the root zone. These are distributed geographically and the hardware is managed by independent organizations. 

The root zone only stores high level information on the Top Level Domains (TLDs) of DNS. TLDs can be generic `.com` or country-code specific, `.uk`, `.au`. IANA delegates the management of these TLDs to other organizations known as registries. The job of the root zone is to point to these TLD registries. 

The TLD servers point to authoritative name servers, which host one or more zones for a domain, such as `netflix.com` or `bsky.app`. So the `netflix.com` entry in the TLD points to the Netflix name servers. The name servers host the zone for a given domain. This means they host the ZoneFile which contains the data for that zone. These zones and ZoneFiles are also authoritative for that domain.#
#### Walking the Tree
The job of DNS is to help you locate and get a query response from the authoritative zone which hosts the DNS records you need.

If you perform a DNS query for `www.netflix.com`, the first thing to be checked will be the local cache and Hosts file. If the local client doesn't know the DNS name you're querying, a DNS resolver is checked next. A Resolver is a type of DNS server often running on a home router or within an IP provider and will make the query on your behalf. The Resolver checks its own local cache and might be able to return an non-authoritative answer.

The Resolver queries the root zone, which contains name server records for the `.com` TLD. The Resolver can now query one of the `.com` name servers. The `.com` name servers can direct the Resolver to the `netflix.com` name servers. The Resolver queries the `netflix.com` name servers, they return a DNS record to the Resolver, which caches the result to improve future performance.

No one name server has all the answers but every query moves you to the next step.
#### Registering a New Domain
A Domain Registrar (Route53 or Hover) has one function, to let you purchase domains. To allow this, they have a relationship with the TLD Registry for many top level domains.

A DNS Hosting Provider operates DNS name servers, which can host DNS zones. They allow you to manage the content of those zones. 

Registrars and hosting providers can be the same or different companies. 

1. customer pays for available `.com` domain
2. registrar either creates a zone or requests a zone be created by a hosting provider
3. registrar supplies name servers info to TLD registry
4. TLD registry adds domain name servers to the `.com` TLD zone

#### DNSSEC
DNSSEC strengthens authentication in DNS using digital signatures based on public key cryptography. With DNSSEC, it's not DNS queries and responses themselves that are cryptographically signed, but rather DNS data itself is signed by the owner of the data.

DNSSEC provides improvements over DNS
- Data origin authentication - this data comes from this zone
- Data integrity protection - this data hasn't been modified in transit
- establishes chain of trust between root zone and records

DNSSEC is additive, it doesn't replace DNS. A DNSSEC-capable device will get back DNS results and DNSSEC results. With DNSSEC, invalid records will be easily identifiable.

Within a Zone, such as `icann.org`, there are resource records (cname, A, AAAA, MX) grouped into resource record sets for easier management. An RRSET is a set of all records with the same name and the same type.

Individual resource records are not validated by DNSSEC, RRSETs are. RRSIG stores a digital signature of an RRSET using public and private pairs of keys. This key pair is known as the Zone Signing Key (ZSK). An external process uses the private part of the ZSK to create a signature which can be stored alongside the plaintext RRSET in the zone using the same name, labelled as RRSIG.

If the RRSET changes, the RRSIG has to be regenerated. Any unauthorized changes result in an invalid signature. The private ZSK is needed for generation.

You need the public part of the ZSK to verify RRSIGs created using the private part. The DNSKEY contains the public key to verify all RRSIGs in the zone. The DNS resolver can take the RRSET, and with the matching RRSIG and the DNSKEY record, can verify that both the RRSIG matches the RRSET and that the signature was generated using the private part of the ZSK. This assumes you trust the DNSKEY, that is that only the zone admin has the private signing key. 

The zone is linked cryptographically to the parent zone. The private Key Signing Key (KSK) creates an RRSIG from the DNSKEY. Changing the ZSK requires regenerating the RRSIG records, updating the DNSKEY RecordSet, and then regenerating the RRSIG of the DNSKEY RecordSet using the private KSK. All of this occurs inside the zone only.

We can validate the KSK because it's referenced from the parent zone. The DNSKEY (KSK) is the point of trust for the zone. The parent zone links to the KSK, so we can change the ZSK as required without impacting the parent zone.

##### DNSSEC Chain of Trust
A parent zone needs to explicitly say it trusts its child zone and this is done using a Delegated Signer Record Set. This stores a hash of the child domain's public Key Signing Key. Since the hash is one way and unique, adding this record shows the parent zone trusts the child zone's KSK. 

There is a matching RRSIG, a matching digital signature of the DS RRSET made using the parent zone's private ZSK. RRSIGs in the parent zone are validated using its DNSKEY (public ZSK and public KSK). This DNSKEY needs a matching RRSIG created by signing the DNSKEY RRSET with the parent zone's public KSK.

The root zone's private Key Signing Key is explicitly trusted, it's sometimes called the trust anchor. In two secure locations (California and Virginia) are what are essentially the keys to the internet. The private DNS Root Key Signing Key. They rarely change, and the trust in them is hard-coded into all DNSSEC clients. They are locked away, protected and never exposed. They use redundant hardware security modules, also redundant across physical locations.

The public part of the DNS Root Key Signing Key is part of the DNSKEY RecordSet within the Root zone, along with the public Zone Signing Key. The Root zone Zone Signing Key is signed with the private Root zone KSK producing as an output the Root zone RRSIG DNSKEY.

The signing ceremony involves a Ceremony Administrator and at least 3 Crypto Officers, using the hardware security modules and a stateless ceremony laptop.

### Cloud Computing
Cloud computing is a model for enabling ubiquitous, convenient, on-demand network access to a shared pool of configurable computing resources (networks, servers, storage, applications, and services) that can be rapidly provisioned and released with minimal management effort or service provider interaction.

NIST (National Institute of Standards and Technology) defines 5 essential characteristics of cloud computing:

1. **On-demand Self Service** - you can provision capabilities as needed **without requiring human interaction**
2. **Broad Network Access** - capabilities are available over the **network** and accessed through **standard mechanisms**
3. **Resource Pooling** - There is a sense of location independence, with no control or knowledge over the exact location of the resources. The resources are pooled to serve multiple consumers using a multi-tenant model.
4. **Rapid Elasticity** - capabilities can be elastically provisioned and released to scale rapidly outwards and inwards with demand. To the consumer, the capabilities available for provisioning often appear to be unlimited.
5. **Measured Service** - resource usage can be monitored, controlled, reported, and billed.

#### Public vs Private vs Multi vs Hybrid Cloud
Public cloud, simply put, is a cloud environment that is available to the general public.

Multi-cloud is using multiple public cloud platforms in a single system, perhaps to achieve high availability.

Private cloud is a solution dedicated to your business and run from your business premises (such as AWS Outputs). It still needs to meet the 5 characteristics of cloud computing.

Hybrid cloud is the use of private cloud in conjunction with public cloud collaborating as a single environment. It is not connecting a public cloud to your legacy on-premises network (this could be called hybrid environment).

#### Cloud Service Models
When you deploy an application anywhere, it uses an infrastructure stack, a collection of things that application needs which build on top of each other.
- application
- data
- runtime
- container
- OS
- virtualization
- servers
- infrastructure
- facilities

There are parts you manage and parts that are managed by a vendor. You consume and pay for a unit of consumption, the part of the system from which point upwards you are responsible for managing. If you procure a virtual server, then your unit of consumption is the virtual machine.

With an on-premises system, your business has to buy and manage all parts of the stack, as well as the staff costs and risks. This is expensive and has some risks but is very flexible.

Data centre hosting (which was more popular before cloud computing) means that you buy infrastructure and use a facility (data centre) owned by someone else.  

Infrastructure as a service (IaaS) means that the provider manages the facilities, storage, networking, physical server and virtualization. You consume the OS and manage it and anything above the OS. You usually pay per second/minute/hour for the virtual machine. EC2 would be an example of IaaS.

Platform as a Service (PaaS) is aimed at developers who have an application they want to run without being concerned about the infrastructure. Your unit of consumption is the runtime environment.

Software as a Service (SaaS) is when you consume the application. You have no exposure to anything else in the stack. For example, Dropbox, Gmail, Office 365.

### Kubernetes
Kubernetes (K8s) is an open-source system for automating deployment, scaling, and management of containerized applications.

- Cluster - a deployment of K8s, for management, orchestration
- Node - resources, pods are placed on nodes to run
- Pod - 1 or more containers, smallest unit in K8s, often 1 container 1 pod
- Service - abstraction, service running on one or more pods
- Job - ad-hoc, creates one or more pods until completion
- Ingress - exposes a way into a service. (ingress -> routing -> service -> 1+ pods)
- Ingress Controller - used to provide ingress (such as nginx or AWS LB controller using ALB/NLB)
- Persistent Storage (PV) - volume whose lifecycle lives beyond any one pod using it

A K8s cluster is a highly-available cluster of compute resources organized to work as one unit. The cluster Control Plane manages the cluster, scheduling applications, scaling and deploying. Cluster Nodes are virtual machines or physical servers which function as workers in a cluster. On each node, containerd, Docker or another container runtime handles container operations. Kubelet is an agent to interact with the cluster control plane. The Kubernetes API is used for communication between the control plane and kubelet agents.

Pods are the smallest units of computing in Kubernetes. They have shared storage and networking. It's common to see a one container - one pod architecture. The pods handle the containers within them. Pods are non-permanent. They are created, do a job, and are disposed of. Pods can be deleted when finished, evicted for lack of resources, or if the node itself fails.

kube-apiserver is the frontend for the Kubernetes control plane. It's what nodes and other cluster elements interact with. It can be horizontally scaled for high availability and performance. etcd provides a highly-available key-value store used as the main backing store within the cluster.

kube-scheduler identifies any pods within the cluster with no assigned nodes and assigns a node based on resource requirements, deadlines, affinity/anti-affinity, data locality, or other constraints. The cloud-controller-manager provides cloud-specific control logic. It allows you to link K8s with a cloud provider's APIs.

kube-controller-manager is a collection of processes. Node controller monitors and responds to node outages. Job controller runs pods to execute one off tasks. Endpoint controller populates endpoints, linking services to pods. Service Account and Token controller handles account and API token creation.

kube-proxy is a network proxy running on each node. It coordinates networking with the control plane. It helps implement services and configure rules allowing communication with pods from inside or outside the cluster.

It's best to architect things in K8s to be stateless from a pod's perspective since pod are temporary.

### Backups and Data Recovery
#### Recovery Point Objective (RPO) and Recovery Time Objective (RTO)
Recovery Point Objective (RPO) and Recovery Time Objective (RTO) are two key metrics used in business continuity and disaster recovery planning. 

RPO refers to the amount of data that can be lost, or the maximum allowable age of the last backup, in the event of a disaster or outage. 

RTO, on the other hand, refers to the maximum tolerable duration of a service outage, or the time it takes to restore services to normal after a disaster or outage.

RPO and RTO are both critical in determining the appropriate backup and recovery strategy for an organization, as well as for ensuring that the organization can quickly recover from disaster or outage and resume normal business operations.

A successful backup is a Recovery Point. The maximum data loss is the maximum time between two successful backups. Lower RPO means more frequent backups, which means more cost. Make sure backups occur as or more frequently that the desired RPO. A failed backup can double data loss.

Recovery time of a system begins at the moment of the failure, and ends when the system is operational and handed back to the business. You can only start to recover when you're made aware that the system has failed. Is there monitoring? Is it reliable? How will staff be notified? 

Other questions include: Is the restoration process documented? Is the necessary person available? What are you restoring on? Do you need a secondary site because not just is the server on fire but the entire server room? 

Before final handover to the customer, business testing and user testing is necessary. Different business and systems will have different RPO and RTOs. RPO and RTO can be reduced via planning, monitoring, notifications, processes, spare hardware, training, and more efficient systems.

### Configuration Formats
#### YAML
YAML (YAML Ain't Markup Language) is a lightweight, human-readable data serialization format that is often used for configuration files and data exchange between applications. YAML supports complex data structures, including lists, dictionaries and nested structures, and can be used with a wide range of programming languages and tools.

A YAML document is an unordered collection of key-value pairs, separated by a colon. YAML supports strings, integers, floats, booleans, null and lists.

```YAML
cats: ["truffles", "penny", "winkie"]
alsocats:
 - "truffles"
 - "penny"
 - winkie
```

String values can be enclosed in quotes or not. Enclosing can be more precise.

Using YAML's support for key-value pairs, lists and dictionaries allows you to build complex data structures in a human-readable way.

#### JSON
JSON (JavaScript Object Notation) is a lightweight data interchange format that is commonly used for data exchange between applications. Indentation isn't important because everything is enclosed in quotes or brackets. JSON supports strings, numbers, objects, arrays, booleans, and null.

Every JSON document is a collection of unordered key-value pairs, where the value is a JSON object.













