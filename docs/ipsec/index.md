## IPsec Laboratory

IPsec is a widely deployed suite of protocols that provides confidentiality, integrity, authentication, and
replay protection for IP communications, enabling secure tunnels over untrusted networks without modifying upper-layer applications.

Its position at the network layer makes it suitable for a variety of scenarios, including enterprise site-to-site VPNs, remote access solutions, and backbone protection mechanisms.

The focus of this laboratory shall be in understanding the configuration of IPsec, and observe it functioning, with both IKEv1 and IKEv2, seeing the differences and evolution between them. We will also analyse its functioning using the Authentication Header and the Encapsulating Security Payload.

The experiments will focus on understanding the handshake of both IKEv1 and IKEv2, their KeepAlive, their operating difference, seeing the practical difference between AH and ESP, and seeing how this protocol can operate with Digital Signatures, instead of Pre Shared Keys.

## IKEv1 and IKEv2

IKEv1 was a major step toward automated key management for IPsec, although its design introduced significant complexity and security trade-offs. The protocol is fragmented into Main Mode, Aggressive
Mode, and Quick Mode, each serving different purposes in the establishment of the IKE SA and IPsec SAs.

Main Mode provides identity protection but requires six messages, which we can see in Figure 1, marked as Phase 1. Aggressive Mode reduces this to three messages at the cost of identity exposure and weaker security guarantees, and Quick Mode is used to negotiate IPsec SAs under an already established IKE SA.

IKEv2 addresses these shortcomings by consolidating the protocol into a single, well-defined four-message initial exchange composed of two phases: IKE_SA_INIT and IKE_AUTH.

During IKE_SA_INIT, present in the second half of Figure 1, peers negotiate cryptographic algorithms, exchange nonces, and perform a single Diffie–Hellman key exchange to derive shared secrets. The subsequent IKE_AUTH exchange authenticates both parties and establishes the first IPsec Child SA in a single step, eliminating the need for a separate Quick Mode.

<figure markdown id="figure-1">
  ![Figure 1: IKEv1 vs IKEv2](../images/IKEv2 diagram.drawio.png)
  <figcaption>Figure 1: IKEv1 vs IKEv2</figcaption>
</figure>

## AH and ESP

IPsec consists primarily of two protection mechanisms: the Authentication Header (AH), which is used to authenticate and guarantee the integrity of the messages that the IPsec tunnel protects, although it does not encrypt the messages, thus not providing confidentiality, and the Encapsulating Security Payload (ESP), which is an evolution of the AH, providing both integrity and confidentiality of the messages IPsec is protecting, not only ensuring these messages are authentic but also cryptographically protected.

## Laboratory Topology

The topology of this laboratory consists in:

- Two host devices, labeled as PC1 and PC2, which use Alpine Linux.
- Two Cisco 3725 routers, labeled as R3 and R4, used as gateways for the PCs into the network.
- Two Cisco IOSv, labeled as R1 and R2, that are the two points where the IPsec tunnel forms, to protect traffic crossing the "Internet".
- One Cisco 7200, labeled as RA, which is used for routing, to simulate a tunnel across the Internet and as a Certificate Authority for one of our experiments.

The topology is visible in Figure 2:

<figure markdown id="figure-2">
  ![Figure 2: IPsec Topology](../images/IPSECTOPO.png)
  <figcaption>Figure 2: IPsec Topology</figcaption>
</figure>

## Router Configuration

We will begin the configuration with the two main configurations, and the focus of our laboratory, the routers hosting the IPsec tunnel.

The first device we will configure is R1, and we will begin by starting it up, opening its console, and waiting for it to be ready to configure. When it is ready, first use the command:

```bash
conf t
```

This will set the router into configuration mode, ready for whatever we have to configure.

The first set of configuration focus on setting up the interfaces we will be using:

```bash
hostname R1

interface g0/0
 ip address 200.1.1.1 255.255.255.0
 ip ospf 1 area 0
 no shutdown

interface g0/1
 ip address 192.168.2.1 255.255.255.0
 ip ospf 2 area 0
 no shutdown

interface l0
 ip address 1.1.1.1 255.255.255.255
 ip ospf 1 area 0
```

Following this configuration, we reach the IPsec section. In this first part, we will set the policy that ISAKMP, the framework that IKE uses to operate, will use:

```bash
crypto isakmp policy 10
encryption aes 256
hash sha
authentication pre-share
group 5
lifetime 86400
```

We can see from this configuration, that we will use AES with 256 bits, SHA for the hash, a pre-shared key as authentication, the Diffie-Hellman group used, number 5 and its lifetime at 86400 seconds.

After that, we will now configure the PSK, the peer address, the Transform set, the IPsec profile that will be used and the Tunnel interface which will be one of the tunnel endpoints:

```bash
crypto isakmp key ipsec address 2.2.2.2

crypto ipsec transform-set myTSet esp-aes esp-sha-hmac

crypto ipsec profile myIPSecProfile
 set transform-set myTSet

interface Tunnel0
 ip unnumbered g0/1
 tunnel source l0
 tunnel destination 2.2.2.2
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile myIPSecProfile
 ip ospf 2 area 0
```

From the configuration we can observe that the key is "ipsec" and that the peer address is 2.2.2.2. We can see that the Transform set, named myTSet, will use ESP, with AES for encryption and SHA for integrity. We can also see the the profile we created, named myIPSecProfile will use this Transform set. Finally, we create the tunnel interface, Tunnel0, which will use g0/1 IP address, as its source, the interface l0, as its peer 2.2.2.2, using IPsec with IPv4 and our profile, myIPSecProfile as its security policy, which in turn will use the Transform set we configured.

For last, we add the final routing configurations needed for the network to function properly:

```bash
router ospf 1
 router-id 1.1.1.1

router ospf 2
 router-id 11.11.11.11
```

With R1 complete, we move to R2. Follow the same initial steps to be ready to configure, and then use the following configuration:

```bash
hostname R2

interface g0/0
 ip address 200.2.2.2 255.255.255.0
 ip ospf 1 area 0
 no shutdown

interface g0/1
 ip address 192.168.3.2 255.255.255.0
 ip ospf 2 area 0
 no shutdown

interface l0
 ip address 2.2.2.2 255.255.255.255
 ip ospf 1 area 0

crypto isakmp policy 10
 encryption aes 256
 hash sha
 authentication pre-share
 group 5
 lifetime 86400

crypto isakmp key ipsec address 1.1.1.1

crypto ipsec transform-set myTSet esp-aes esp-sha-hmac

crypto ipsec profile myIPSecProfile
 set transform-set myTSet

interface Tunnel0
 ip unnumbered g0/1
 tunnel source l0
 tunnel destination 1.1.1.1
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile myIPSecProfile
 ip ospf 2 area 0

router ospf 1
 router-id 2.2.2.2

router ospf 2
 router-id 22.22.22.22
```

We can see that both configurations are similar, with changes only to its addresses and its peer addresses, as expected.

Now we will configure the remaining routers, only used for routing.

RA:

```bash
hostname RA

interface f0/0
 ip address 200.1.1.10 255.255.255.0
 ip ospf 1 area 0
 no shutdown

interface f0/1
 ip address 200.2.2.10 255.255.255.0
 ip ospf 1 area 0
 no shutdown

router ospf 1
 router-id 10.10.10.10
```

R3:

```bash
hostname R3

no ip domain lookup

interface f0/0
 ip address 192.168.2.3 255.255.255.0
 no shutdown

interface f0/1
 ip address 192.168.1.3 255.255.255.0
 no shutdown

ip routing

router ospf 2
 network 192.168.1.0 0.0.0.255 area 0
 network 192.168.2.0 0.0.0.255 area 0
```

R4:

```bash
hostname R4

no ip domain lookup

interface f0/0
 ip address 192.168.3.4 255.255.255.0
 no shutdown

interface f0/1
 ip address 192.168.4.4 255.255.255.0
 no shutdown

ip routing

router ospf 2
 network 192.168.3.0 0.0.0.255 area 0
 network 192.168.4.0 0.0.0.255 area 0
```

For routers RA, R3 and R4, we recommend leaving configuration mode with the command "exit", and then writing the command "wr". This will save the current configuration of this routers as their startup configuration, removing the need to configure them everytime they are turned off.

This is not recommended for R1 and R2, since we will be using several different configurations for our experiments.

And finally, the configuration for the two PCs.

PC1:

```bash
ip addr add 192.168.1.100/24 dev eth0
ip link set eth0 up
ip route add default via 192.168.1.3
```

PC2:

```bash
ip addr add 192.168.4.100/24 dev eth0
ip link set eth0 up
ip route add default via 192.168.4.4
```

With all this done, we can easily test if this setup is working by opening a WireShark probe between R1 and RA, performing a ping from PC1 to PC2 using:

```bash
ping 192.168.4.100
```

And observing if the packets are protected, and instead of being their usual ICMP, they are ESP, as we can see in Figure 3:

<figure markdown id="figure-3">
  ![Figure 3: IPsec Test](../images/IPSECTEST.png)
  <figcaption>Figure 3: IPsec Test</figcaption>
</figure>

With all the configurations ready and understood, we can move forward to the [Experiments](experiments.md) section, to learn more about IPsec, IKE, digital signatures and certificates.
