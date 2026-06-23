---
title: "Blog 1"
date: "2026-06-01"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

# [SECURITY/Web3] Building secure, verifiable blockchain key management on AWS Nitro Enclaves at Turnkey
<span class="meta-info">by Harshvardhan Chunawala and Jack Kearney | on 08 JUN 2026 |

Hello everyone! With private key leaks happening constantly, "Key Management" has become a massive headache for Web3/DeFi developers. I recently read a technical blog post from the AWS Web3 Blog about how Turnkey thoroughly solves this issue by combining cryptography and AWS Nitro Enclaves hardware infrastructure. Here are the core takeaways:

1. **Challenges in current key management architecture**
   The traditional transaction signing process requires a careful trade-off between security and operational performance:
   * Self-hosted infrastructure: Requires significant costs and carries high compliance risks.
   * Third-party custodians: Reduces direct control and lacks operational transparency.
   * Standard software infrastructure: Raw keys are vulnerable to exploitation via memory dumps or log files if the ecosystem is compromised.

2. **Isolation mechanism of AWS Nitro Enclaves**
   Turnkey implements an Enclave-Native Key Management model, moving all sensitive tasks—including key generation, digital signing, and policy execution—into the AWS Nitro Enclaves environment:
   * Hardware Isolation: The enclave environment has no persistent storage, no interactive access (no SSH), and no Internet connection.
   * Secure Communication: Data is transmitted via an internal virtual VSOCK channel. Configuration keys are only decrypted in RAM at the moment of transaction signing and are immediately wiped. Neither Turnkey’s system administrators nor AWS can access the raw keys.

3. **Cryptographic initialization and data storage process**
   The system uses a Hierarchical Deterministic Wallet (HD Wallet) model to manage derivative key structures:
   * Seed Generation: Uses a cryptographically secure random number generator provided by the Nitro Security Module (NSM) hardware.
   * Symmetric Encryption: The root seed string is encrypted via a Quorum Key before being stored in the database.
   * Transaction Signing: The ciphertext is loaded into the enclave, decrypted temporarily in RAM to perform the cryptographic signing, and then immediately purged. Raw keys are never written to disk.

4. **System state separation and data flow architecture**
   For clarity, Turnkey's architecture is divided into two separate parts: the External Management Partition (not absolutely secure) and the Internal Computing Partition (absolutely secure).
   * External (AWS Cloud Infrastructure): When a user sends a request via API Gateway, the EC2 server (Coordinator) receives and processes it. State data and encrypted root keys are stored in an Aurora Database. Other background tasks (Async Queue, Redis, Updater, Heartbeat, and Notifier) handle system synchronization and webhook notifications. This partition has absolutely no knowledge of the raw keys.
   * Internal (AWS Nitro Enclave): The EC2 server passes sensitive commands down to the Enclave via an internal gRPC/VSOCK channel. In this isolated environment, the processing flow happens in 5 closed steps:
     * TLS Fetcher: Establishes a secure network connection from inside.
     * Parser: Decodes the transaction data.
     * Policy Engine: Checks if the transaction violates user-defined rules (limits, blocklists, etc.).
     * Notarizer: Certifies the valid transaction after it passes the policy engine.
     * Signer: Fetches the encrypted key from the database, decrypts it temporarily in RAM to sign the transaction, and then wipes all traces.

5. **Remote Attestation based on mathematics**
   Turnkey shifts the model from trust-based to verifiable based on:
   * Remote Attestation: AWS provides cryptographic attestation documents signed by hardware to verify that the code running in the enclave has not been altered or tampered with.
   * Reproducible Builds: The system operates on QuorumOS—a minimalist operating system. Independent parties can recompile the source code from scratch to verify the integrity of the system.

**Practical Applications**
* Embedded Wallets: Integrating non-custodial wallets into decentralized applications with enterprise-grade security standards.
* AI Agent Transactions: Supporting AI agents in executing automated on-chain transactions according to pre-set policies without exposing configuration keys.

**Summary**
Turnkey’s solution leverages AWS Nitro Enclaves to establish a closed-loop key processing workflow within RAM, with automatic memory clearing after use. The total separation between State storage and the isolated hardware execution environment protects digital assets even if the virtual machine infrastructure is breached. Furthermore, thanks to remote attestation and reproducible build capabilities, the system allows users to verify the integrity and transparency of the entire cryptographic process.

![Turnkey Infrastructure Diagram](/static/images/3-Blogs/Blog-1/blog1.png)

## Conclusion
Turnkey’s solution leverages AWS Nitro Enclaves to establish a closed-loop key processing workflow within RAM, with automatic memory clearing after use. The total separation between State storage and the isolated hardware execution environment protects digital assets even if the virtual machine infrastructure is breached. Furthermore, thanks to remote attestation and reproducible build capabilities, the system allows users to verify the integrity and transparency of the entire cryptographic process.

Link bài viết: [Building secure, verifiable blockchain key management on AWS Nitro Enclaves at Turnkey | AWS Web3 Blog ](https://aws.amazon.com/vi/blogs/web3/building-secure-verifiable-blockchain-key-management-on-aws-nitro-enclaves-at-turnkey/)