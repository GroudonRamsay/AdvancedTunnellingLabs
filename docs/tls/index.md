## TLS Laboratory

Transport Layer Security (TLS) is one of the most used protocols to secure Internet communications, providing confidentiality, integrity, and authentication for applications such as HTTPS, APIs, email, and enterprise services.

TLS 1.2 served as the industry standard for many years, but its complexity, dependence on numerous cipher suites, and susceptibility to downgrade and mode-specific vulnerabilities motivated the need for a more robust and modern successor.

TLS 1.3 addresses these challenges through a reduced handshake, removal of legacy algorithms, and stricter cryptographic guarantees, offering faster and more secure session establishment.

## TLS Handshake

TLS 1.2 relies on a multi-round-trip handshake that includes several optional messages, multiple cipher negotiation paths, and support for quantum-vulnerable primitives such as RSA key exchange
and CBC-mode ciphers. Although flexible, this design introduced complexity and multiple attack surfaces.

TLS 1.3 significantly simplifies this model by mandating modern AEAD ciphers, enforcing perfect forward secrecy, and reducing the handshake to a usual 1 RTT exchange.

We can observe in Figure 1 the comparison between the handshake of TLS 1.2 and 1.3. We can see some improvements between versions such as, reduced RTT for session establishment, through the front-loading of key exchange material into the Client HELLO, which allows TLS 1.3 to start a session faster and with encrypted messages sooner.

<figure markdown id="figure-1">
  ![Figure 1: TLS 1.2 and 1.3 Handshakes](../images/TLSHAND.png)
  <figcaption>Figure 1: TLS 1.2 and 1.3 Handshakes</figcaption>
</figure>

A major innovation in TLS 1.3 is its support for 0-RTT data during session resumption, enabling clients to send application data immediately after initiating a new handshake. While 0-RTT data is vulnerable to replay and requires careful use, it provides substantial performance gains for repeated connections and is a key enabler for low-latency secure communication.
