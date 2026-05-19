# WireGuard

WireGuard is a modern Layer 3 VPN protocol focused on simplicity, performance, and cryptographic security.

---

## Features

- Minimal configuration
- High performance
- Modern cryptography
- UDP-based transport
- Simple key management
- Cross-platform support

---

## Cryptography

| Component      | Algorithm  |
| -------------- | ---------- |
| Key Exchange   | Curve25519 |
| Encryption     | ChaCha20   |
| Authentication | Poly1305   |
| Hashing        | BLAKE2s    |

---

## Typical Use Cases

=== "Site-to-Site VPN"

    Connect remote networks securely.

=== "Remote Access"

    Secure remote user connectivity.

=== "Lab Topologies"

    Encrypted routing experiments.

=== "Container Networking"

    Secure inter-container communication.

---

## Example Topology

```mermaid
graph LR
    A[WG Node 1] --- B[Internet]
    B --- C[WG Node 2]
```

---

## Topics Covered

- Linux WireGuard setup
- Peer configuration
- OSPF over WireGuard
- Routing integration
- MTU tuning
- Containerlab deployment
- Troubleshooting

---

## Example Configuration

```ini
[Interface]
Address = 10.10.10.1/24
PrivateKey = <private-key>
ListenPort = 51820

[Peer]
PublicKey = <peer-public-key>
AllowedIPs = 10.10.10.2/32
Endpoint = 192.168.1.10:51820
```

!!! warning

    Incorrect MTU values commonly cause fragmentation and tunnel instability.
