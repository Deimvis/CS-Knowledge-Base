# Security

## Threats

* Security threat — a potential for violation of security, which exists when there is an entity, circumstance, capability, action, or event that could cause harm
* Types
  * interception
  * modification
  * fabrication
  * interruption

* Attack Examples

  * eavesdropping

  * data tampering

  * man-in-the-middle

  * masquerading, spoofing, phishing

  * replaying

  * denial-of-service

  * malware, spyware


## Secure Systems in General

### Requirements

* Privacy
* Integrity
* Authentification
* Impossibility of downtime
* Authorization
* Availability
* Scalability

### Assumptions

* Publicity of interfaces
* Networks aren't secure
* Algorithms and code are available for attackers
* Attackers have big computing capability

### Methodology

* Threats analysis
* Threats prevention
* Validation
* Audit

### Approaches

* Cryptography
* Access Control (authorization, etc)
* Security Management (key, certificates distribution, etc)

## Encryption

* Encryption — process of encoding information
* Types
  * Symmetric: encryption and decryption keys are the same
  * Assymetric: encryption and decryption keys are different
* Examples
  * Symmetric (block cipher)
    * DES
    * 3DES
    * TEA
    * IDEA
    * Blowfish
    * Twofish
    * AES
  * Symmetric (streaming cipher)
    * RC4
  * Assymetric
    * RSA
    * ElGamal
    * ECDSA

### Symmetrics vs Assymetric Encryption

|                  | Symmetric                                                    | Assymetric                                           |
| ---------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| Key Distribution | Through secure channel                                       | Through any channel (public key)                     |
| Number of keys   | $\frac{N(N-1)}{2}$ in system of $N$ nodes (key for each pair) | $2 \cdot N$ in system of $N$ nodes (2 keys for each) |
| Performance      | Fast                                                         | Slow (10-100 times slower)                           |

## Best Common Practices

* TTL of secrets should be limited
* Number of critically important components should be minimized

