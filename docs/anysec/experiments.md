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

## Tunnel Encryption and Slicing

## Service Encryption
