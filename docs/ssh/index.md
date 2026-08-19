## SSH Laboratory

Secure Shell (SSH) is a protocol for secure remote access, encrypted command execution and file transfer. Standardised by IETF in RFCs 4251 through 4254, SSH provides confidentiality, integrity, and peer authentication through a flexible layered architecture.

Operating at the application layer and running over TCP, SSH offers user-centric access control, key-based authentication, and session multiplexing. These properties make it well suited for administrative access, secure management of remote hosts and port forwarding.

SSH is commonly used to create local, remote, or dynamic tunnels that protect otherwise insecure application flows. Typical use cases include secure system administration, tunnelling database
connections across untrusted networks and enabling secure file transfer through SFTP.

## SSH Architecture and Handshake

SSH is divided into three logical layers: the transport layer, the authentication layer, and the connection layer.

The transport layer, described in RFC 4253, establishes a secure channel, with its handshake present in Figure 1, providing confidentiality and integrity for subsequent protocol layers, negotiating encryption algorithms, for example AES-GCM or ChaCha20-Poly1305, MACs, compression, and key exchange algorithms such as Curve25519 or ECDH.

The authentication layer supports mechanisms such as public-key authentication, with RSA, Ed25519 or ECDSA and password, while the connection layer multiplexes multiple logical channels, like, shell, exec, subsystem and portforwarding, over a single transport connection as defined by RFC 4254.

<figure markdown id="figure-1">
  ![Figure 1: SSH Handshake](../images/SSHHAND.png)
  <figcaption>Figure 1: SSH Handshake</figcaption>
</figure>

## Laboratory Topology

## Device Configuration
