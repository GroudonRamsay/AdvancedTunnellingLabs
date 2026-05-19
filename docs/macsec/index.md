# MACsec

MACsec (Media Access Control Security) is a Layer 2 encryption protocol defined by IEEE 802.1AE. It provides hop-by-hop confidentiality, integrity, and replay protection for Ethernet traffic.

---

## Features

- Layer 2 encryption
- AES-GCM confidentiality
- Integrity protection
- Replay attack prevention
- Secure VLAN transport
- Hardware acceleration support

---

## Typical Use Cases

=== "Datacenter"

    Secure leaf-spine links inside datacenters.

=== "Service Provider"

    Protect metro Ethernet and backbone links.

=== "Enterprise"

    Secure switch-to-switch uplinks.

=== "Lab Environments"

    Validate encrypted VLAN transport.

---

## MACsec Architecture

```mermaid
graph LR
    A[Host A] --> B[Switch 1]
    B --> C[Encrypted Ethernet Link]
    C --> D[Switch 2]
    D --> E[Host B]
```

---

## Topics Covered

- SR Linux MACsec configuration
- MKA operation
- CAK/CKN usage
- VLAN encryption modes
- WAN vs LAN MACsec
- Containerlab integration
- Troubleshooting workflows

---

## Verification Commands

```bash
show system security macsec connectivity-association
show system security macsec interfaces
```

!!! note

    MACsec encrypts Ethernet frames directly and operates independently of IP routing.
