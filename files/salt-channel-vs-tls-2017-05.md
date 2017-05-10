Frans Lundberg, ASSA ABLOY AB, 2017-05-10


salt-channel-vs-tls-2017-05.md
==============================

Example comparing Salt Channel with TLS. Wireshark is used 
to analyse the network traffic. 
See code in saltchannel.dev.Tls/RunClient/RunServer for 
implementation details.

The application protocol: 

    Client sends 6 bytes (0x010505050505), the server echoes the
    same six bytes back to the client. 12 bytes sent, one round-trip.

Results:

    SALT CHANNEL VS TLS
    
    Salt Channel: v2, no resume.
    
    TLS: v1.2, suite TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256, 
    256-bit keys, curve P-256, client and server certificates used (minimal, self-signed),
    no resume.
    
    RESULTS                    TLS    Salt Channel    Comment
    
    Total sent bytes:         2299             404    TLS: 6 times more data.
    Round-trips:                 4               2    
    Forward secrecy:           yes             yes    For this cipher suite.
    Client authenticated:      yes             yes    For this scenario.
    Client ID hidden:           no             yes    Perhaps in TLS 1.3.
    EC curve:                P-256      Curve25519
    ECC key size (bits)        256             256
    Symmetric key size (bits)  128             128
    

TLS has a much larger handshake overhead, about 6 times more data
is sent and 4 round-trips are used while Salt Channel only needs 
2 round-trips.

Comments:

* TLS uses about 6 times more data.

* TLS uses 4 round-trip while Salt Channel needs only 2.

* 3 round-trips were expected for the TLS case. However, 
    no investigation has been made to explain this 
    unexpected behavior.
    
* ed25519 signature is included in the current TLS 1.3 draft.

* The current TLS 1.3 draft [TLS1.3] says that
  a TLS-compliant application SHOULD support key exchange 
  with X25519.

* Using RSA 4096-bit keys would increase the handshake overhead
    significantly.
    
* Using real certificate chains would increase the
    handshake data significantly. In this example, both the client 
    certificate and the server certificate are self-signed. The certificate
    chains on both sides contain only one certificate.


References
==========

[TLS1.3] TLS version 1.3, draft-ietf-tls-tls13-20, April 2017. 
See https://tools.ietf.org/html/draft-ietf-tls-tls13-20.