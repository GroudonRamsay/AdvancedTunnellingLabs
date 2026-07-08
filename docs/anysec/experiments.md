## Laboratory Experiments

With the laboratory completed, we will now move forward to the experiments related to ANYsec. Our aim for this section will be to analyse ANYsec´s most important components related to its tunnelling capabilities, and do some technical comparisons with MACsec.

## MKA over UDP, Handshake and KeepAlive comparisons with MACsec

For our first experience, we will study the way ANYsec does its authentication. Similar to MACsec, ANYsec uses MKA to perform its authentication process and to share the session keys that will be used to secure the traffic within ANYsec.

The main difference between ANYsec´s MKA and MACsec´s MKA, is that for ANYsec, the MKA packets are sent using UDP, since it is needed to send the packets through entire networks and not only on a link level.

!!! warning "For the majority, if not all of the ANYsec packets we will be analysing, WireShark is still incapable of dissecting them. As such all the figures we will display with dissected packets are done in WireShark with the plugin [ANYsec Dissectors](https://github.com/xavixava/anysec-dissectors) installed. We recommend installing it for the full experience."

We will then need to see the MKA process occuring to see any diferences or similarities. For this we will retrigger a handshake by rebooting routers PE1 and PE2 using the following command at both machines simultaneously:

```srl
admin reboot now
```

Before running the command place WireShark probes at the two ports of both routers that are facing P3 and P4, and then run the command to see the handshake and the MKA process.

The result will be similar to what we have seen in MACsec, a sudden break from the KeepAlive routine, and then the MKA Handshake packets appearing, followed by the routine packets again.

<figure markdown id="figure-1">
  ![Figure 1: MKA Handshake from the Key Server view](../images/ANYMKAKEYSERVER.png)
  <figcaption>Figure 1: MKA Handshake from the Key Server view</figcaption>
</figure>

From this Figure we can identify some interesting aspects. First of all, the MKA process is identical to the one we see in MACsec, and secondly, we can see two handshakes occuring at once, due to the two services using tunnel encryption.

<figure markdown id="figure-2">
  ![Figure 2: MKA Handshake First Key Name](../images/ANYMKAKEYNAME1.png)
  <figcaption>Figure 2: MKA Handshake First Key Name</figcaption>
</figure>

<figure markdown id="figure-3">
  ![Figure 3: MKA Handshake Second Key Name](../images/ANYMKAKEYNAME2.png)
  <figcaption>Figure 3: MKA Handshake Second Key Name</figcaption>
</figure>

We can see in Figures 2 and 3, that the CAK names for both packets are different and match the names we used in the configuration of the two services using tunnel encryption.

<figure markdown id="figure-4">
  ![Figure 4: MKA Handshake Second Key Name](../images/ANYMKAKEYDIST.png)
  <figcaption>Figure 4: MKA Handshake Second Key Name</figcaption>
</figure>

In Figure 4 we can see the final handshake packets, which distribute the two SAKs that will be used by the ANYsec tunnel, specifically the two slices using tunnel encryption. This way each slice has its own SAK, which means its own encryption, detached from the other slices.

Following the full handshake process, we can see that ANYsec uses MKA to the letter, following exactly MACsec´s usage, with the difference that it is transported by UDP so it can traverse across several routers to its destination, and that several handshakes occur at once depending on how many CAs are needed for tunnels or services.

We will now briefly analyse the ANYsec KeepAlive, to see what it uses to maintain its connection.

<figure markdown id="figure-5">
  ![Figure 5: ANYsec KeepAlive packet](../images/ANYKEEPALIVE.png)
  <figcaption>Figure 5: ANYsec KeepAlive packet</figcaption>
</figure>

As we can see in Figure 5, the KeepAlive for ANYsec is identical to the one used by MACsec, sharing the same details in its parameters to prove it is still up and ready to communicate.

## End-to-End encryption vs Hop-by-Hop encryption

As we saw in the MACsec experiments, MACsec operates with Hop-by-Hop encryption, meaning that at every router a MACsec encrypted packet passes through, it must be decrypted and then encrypted again by a new SAK. This type of operation is valuable for added security, but it also adds challenges, mainly around performance.

ANYsec differs from MACsec in this regard, operating with End-to-End encryption, meaning that it encrypts a packet at the starting point of ANYsec, crossing several networks and routers, and only decrypts when it reaches the ending router of ANYsec.

This is possible due to ANYsec capability to operate above Layer 2, namely Layer 2.5 and 3, making it possible to be routed all the way to its destination without having to be decrypted.

With this experiment we will see this behaviour in action, sending packets across ANYsec, capturing them in routers P3 and P4, and comparing them to what we see arriving in routers PE1 and PE2, to confirm if it truly displays End-to-End encryption.

To begin, we will use Host 1 to send the pings, therefore we only need to place WireShark probes at port 1/1/c2/1 of P3 and P4 and at both ports of PE2.

After placing the probes and ensuring everything is running correctly, we will perform the same three pings we did to test the laboratory configuration:

```bash
ping 192.168.51.8
ping 192.168.52.8
ping 192.168.63.8
```

Let each ping run for some time, in order to capture several packets in every step of the way.

After all pings are done, use your WireShark capable of dissecting ANYsec packets to look at the captures.

In theory, the packets related to the service in 192.168.51.8 should have travelled through P3 to reach PE2 in port 1/1/c1/1, the packets related to the service in 192.18.52.8 should have travelled through P4 to reach PE2 in 1/1/c2/1, and the packets related to the service in 192.168.63.8 should have travelled through P4, then to P3, and finally arrived to PE2 in port 1/1/c1/1.

Let´s analyse the packets captured and see if our End-to-End theory is correct.

We will start by looking at the packets sent by pinging 192.168.51.8. These should be related to service 1001, meaning they should have the label 2101.

<figure markdown id="figure-6">
  ![Figure 6: ANYsec Service 1001 P3 Capture](../images/ANY1001P3.png)
  <figcaption>Figure 6: ANYsec Service 1001 P3 Capture</figcaption>
</figure>

As we can see in Figure 6, the packet in P3 does have the expected 2101 label in MPLS, meaning it is one of the pings from the first address. Let´s now compare it to the packet received in PE2, and see if the information within MPLS, 802.1AE Security Tag and Data remains the same.

<figure markdown id="figure-7">
  ![Figure 7: ANYsec Service 1001 PE2 Capture](../images/ANY1001PE2.png)
  <figcaption>Figure 7: ANYsec Service 1001 PE2 Capture</figcaption>
</figure>

We can observe in Figure 7 that the packet reached PE2 shortly after P3. We can confirm it is the same from its 2101 label. Furthermore, we can see absolutely no changes in any piece of information on the headers or in the data, proving that the packet crossed through the network untouched and arrived in PE2 in the same state as when it left PE1.

Now that we have proved our assumption, let´s look at the other two pings to confirm it follows the same pattern.

<figure markdown id="figure-8">
  ![Figure 8: ANYsec Service 1002 P4 Capture](../images/ANY1002P4.png)
  <figcaption>Figure 8: ANYsec Service 1002 P4 Capture</figcaption>
</figure>

<figure markdown id="figure-9">
  ![Figure 9: ANYsec Service 1002 PE2 Capture](../images/ANY1002PE2.png)
  <figcaption>Figure 9: ANYsec Service 1002 PE2 Capture</figcaption>
</figure>

As we can see in Figures 8 and 9, this packet belongs to service 1002, due to its 2201 MPLS label, and all the headers and data related to ANYsec remain the same, further proving the End-to-End encryption of ANYsec.

<figure markdown id="figure-10">
  ![Figure 10: ANYsec Service 1003 P4 Capture](../images/ANY1003P4.png)
  <figcaption>Figure 10: ANYsec Service 1003 P4 Capture</figcaption>
</figure>

<figure markdown id="figure-11">
  ![Figure 11: ANYsec Service 1003 PE2 Capture](../images/ANY1003PE2.png)
  <figcaption>Figure 11: ANYsec Service 1003 PE2 Capture</figcaption>
</figure>

And finally, in Figures 10 and 11, we can see that the service 1003, identified by the MPLS label 2001, also follows the same pattern of the other two services, and the packets fully match.

## Tunnel Encryption and Slicing

For this experiment, we will be exploring one of ANYsec´s ways of encrypting its traffic, through Tunnel Encryption and one of its main features, Tunnel Slicing.

## Service Encryption
