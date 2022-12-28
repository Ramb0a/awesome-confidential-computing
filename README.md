# Confidential Computing [![Awesome](https://cdn.rawgit.com/sindresorhus/awesome/d7305f38d29fed78fa85652e3a63e154dd8e8829/media/badge.svg)](https://github.com/sindresorhus/awesome)

There is a secret operating system [watching you](https://www.zdnet.com/article/minix-intels-hidden-in-chip-operating-system/). 

## Content 

- [What is Confidential Compute](#what-is-confidential-compute) :grey_question:
- [Trust](#trust)
- [Trusted Execution Environments](#trusted-execution-environments)
- [Commercial Confidential Computing](#commercial-confidential-computing)
- [Open Confidential Compute](#open-confidentiality-compute)
- [References](#references)

## What is Confidential Compute

> By providing security though the lowest layers of hardware, with a minimum of dependencies, it is possible to remove the operating system and device driver vendors, platform and peripheral vendors, and service providers and their admins, from the list of required trusted parties, thereby reducing exposure to potential compromise at any point in the system lifecycle. [Confidential Computing Consortium](https://confidentialcomputing.io/)

> With these increased protections for data-in-use, new use cases become more realistic, e.g. multi-party computations in financial and/or regulated industries or machine learning at the edge, where the data being operated needs protecting from the processing environment owner itself. [Confidential Computing Consortium](https://confidentialcomputing.io/)

Confidential Computing provides end-to-end data protection at-rest, in-transit, and in-use. Data and algorithm are protected in a hardware enclave during processing. 


## Trust

> At the lowest level, in hardware, trust is gained by having reliable root-of-trust flows that guarantee that systems are booted into known and valid states, from trusted read-only images (possibly signed by a known and trusted provider).  Moreover, for isolates, the hardware, in tandem with firmware, must provide services that enable the measurement and cryptographic authentication of their initial contents and state. (2021, ARM)

> In the case of software, trust is enabled by source code which is: i) available and auditable, ii) built into binaries with bit-exact reproducibility, iii) signed by a trusted party, iv) formally verified, v) equipped with verifiable certificates (e.g. in the form of a proof witness). (2021, ARM)

**Static trust** is measured only once before deployment

**Dynamic trust** is constantly measured throughout the lifecycle, based on the present state of the running system. 


## Trusted Execution Environments

> Trusted Execution Environment (TEE) is a tamper resistant processing environment that runs on a separation kernel. It guarantees the authenticity of the executed code, the integrity of the runtime states (e.g. CPU registers, memory and sensitive I/O), and the confidentiality of its code, data and runtime states stored on a persistent memory. In addition, it shall be able to provide remote attestation that proves its trustworthiness for third-parties...Attacks performed by exploiting backdoor security flaws are not possible.

Confidential Computing leverages Trusted Execution Environments. A TEE is a secure area within a processor that runs an isolated environment parrallel to the OS. 

A TEE is defined as assuring:
* **Data confidentiality**: Unauthorized entities cannot view data while it is in use within the TEE.
* **Data integrity**: Unauthorized entities cannot add, remove, or alter data while it is in use within the TEE.
* **Code integrity**: Unauthorized entities cannot add, remove, or alter code executing in the TEE.

TEE may also provide:
* code confidentiality (example an algo that is IP)
* authentication 
* code trusting 


# Commercial Confidential Computing

## ARM

>  Once the capability of delegating computations safely to untrusted third-parties is developed, it no longer really matters, from a privacy point-of-view, where compute happens. (2021, ARM)

> Establishing trust in the hardware and software that underlies strong isolation technology is a foundational aspect in delivering on the promises of Confidential Compute. (2021, ARM)

[ARM Confidential Compute Architecture](https://www.arm.com/architecture/security-features/arm-confidential-compute-architecture) (ARM CCA)

[Realm Management Extenstion](https://developer.arm.com/documentation/den0126/0100/Overview) -  A Realm is an isolate protected from privileged—and other non-privileged, but unrelated—software surrounding it, including the operating system, hypervisor, and TrustZone firmware. An untrusted operating system manages the memory and CPU resources of a Realm but cannot access nor interfere with its content or state.

## Intel SGX

[SGX Step](https://github.com/jovanbulck/sgx-step)

[Intel TDE](https://www.intel.com/content/www/us/en/developer/articles/technical/intel-trust-domain-extensions.html) - Trust Domain Extention 

## AMD Sev

## [AWS Nitro Enclaves](https://docs.aws.amazon.com/enclaves/latest/user/nitro-enclave.html#nitro-enclave-reqs)

[Confidential Computing: An AWS Perspective](https://aws.amazon.com/blogs/security/confidential-computing-an-aws-perspective/)

* Uses "Enclave Image File" (.EIF) format
* Provide only secure local socket connectivity with their parent instance - vsock socket 
* No persistent storage
* No interactive access 
* No external networking 
* Attestation feature, which allows you to verify an enclave's identity and ensure that only authorized code is running inside it
* Integrated with the AWS Key Management Service
* No additional charges for using Nitro Enclaves. Billed standard charges for the Amazon EC2 instance
* Uses [Nitro Enclave CLI](https://docs.aws.amazon.com/enclaves/latest/user/nitro-enclave-cli.html)
* Enclave application has two components 
  * application that runs on parent instance
  * application that runs inside the enclave 
* AWS Enclave Developer [AMI](https://aws.amazon.com/marketplace/pp/prodview-37z6ersmwouq2)

> Using the Nitro Enclaves SDK, an enclave can request a signed attestation document from the Nitro Hypervisor that includes its unique measurements. This document can be attached to requests from the enclave to an external service. The external service can validate the measurements included in the attestation document against the values in the access policy to determine whether to grant the enclave access to the requested operation.

## [GCP Confidential Computing](https://cloud.google.com/confidential-computing)

[GCP Confidential VM](https://cloud.google.com/compute/confidential-vm/docs/about-cvm)

* Confidential VM is a type of Compute Engine VM
* Confidential VM runs on hosts with AMD EPYC processors which feature - [AMD Secure Encrypted Virtualization (SEV)](https://developer.amd.com/sev/)
  * How does AMD Platform Security Processor [AMD Secure Technology(p. 157)](https://www.amd.com/system/files/TechDocs/52740_16h_Models_30h-3Fh_BKDG.pdf) effect real security?   
* Use [Virtual Trusted Platform Module](https://trustedcomputinggroup.org/resource/trusted-platform-module-tpm-summary/) Attestation. 

## GPU Trusted Execution Environments 

[NVIDIA H100](https://resources.nvidia.com/en-us-tensor-core)

* **Secure boot** - set of hardware/software ensuring GPU started from 
known secure state permitting only authenticated firmware and microcode authored by NVIDIA to run while GPU booting.
* **Measured boot** - process for collecting, securely storing, and reporting characteristics of boot process determining GPU’s secure state. 
* **Attestation and verification** - are the means of comparing measurements to reference values to ensure that the device is an expected secure state.

Full VM/GPU TEE created by: 

* **On-Die Root of Trust (RoT)** - before OS -> GPU communication, GPU uses ROT to confirm firmware authenticity 
* **Device Attestation** - authenticate NVIDIA GPU with CC enabled & trusted firmware/hardware configuration
* **ES-GCM 256** - CPU <-> GPU data transfers encrypted using hardware AWS256-GCM FIPS 140-3 level 2.

No CUDA application code changes are required to use the NVIDIA confidential computing technology

# Open Confidential Compute

> Existing commercial implementations of enclaves like Intel’s SGX lack transparency on formal correctness guarantees and higher level security properties such as confidentiality and integrity. - (2022, [Verifying RISC-V Physical Memory Protection](https://arxiv.org/pdf/2211.02179.pdf))

If you cannot see the code, can you trust the execution? Can closed source really be confidential with the controversial [Intel Minix OS](https://itsfoss.com/fact-intel-minix-case/) as just one example of the obfuscation engaged in by chip makers.  

## WebAssembly Confidential Compute 

ARM has collaborated with the Veracruz project which is based in Web Assembly. 

[VeraCruz](https://veracruz-project.com/) | [Code](https://github.com/veracruz-project/veracruz) 

* "Veracruz’s platform abstraction also means it is an effective way of deploying computations across a host of different isolation technologies, and makes Veracruz a central pillar in realizing our vision: providing both a means for a group of mutually mistrusting individuals to collaborate, and a substrate through which computations can be safely moved around: write once, isolate anywhere." 
* Veracruz aims to support similar use-cases to Advanced Cryptographic techniques, like homomorphic encryption, functional encryption, and secure multi-party computations, but uses a mixture of strong containerization technology, remote attestation, and transport layer security protocols — instead of pure cryptography — to affect these computations.
* Currently creates a Docker dev image on AWS Nitro or Linux with no trusted execution environment. 
 
[Enarx - Confidential Computing in WebAssembly](https://enarx.dev/)

> Enarx aims to minimize the trust relationships required when executing applications, meaning that the only components which need to be trusted are: the CPU and associated firmware, the workload itself, and the Enarx middleware, which is fully open source and auditable. 

**the problem with "trusting the CPU + firmware" are - what if you can't trust the CPU and firmware - ala the secret operating system to which you have no access**

* provides a WebAssembly runtime, based on [wasmtime](https://github.com/bytecodealliance/wasmtime)
* designed to work across silicon architectures transparently to the user

## Risc V Confidential Compute 

- [Keystone TEE Open Framework for RISC V](https://keystone-enclave.org/)
- [Alibaba Risc V Dev Chip Platform w/ GlobalPlatform Standard (TEE) security certification](https://www.computerweekly.com/news/252524139/Alibaba-Cloud-unveils-RISC-V-chip-development-platform)
- [RISC Zero](https://www.risczero.com/) | :keyboard: [code](https://github.com/risc0/risc0) - The RISC Zero zeroKnowledgeVirtualMachine (zkVM) emulates a small RISC-V computer, allowing it to run arbitrary code in any language, so long as a compiler toolchain exists that targets RISC-V. One party (the prover) to convince another party (the verifier) that something is true without revealing all the details.
- [GoTee](https://github.com/usbarmory/GoTEE) - Extend Go language to allow execution of a goroutine within an enclave, to use low-overhead channels to communicate between the trusted and untrusted environments, and to rely on a compiler to automatically extract the secure code and data. Achieves a 5.2x throughput, and a 2.3x latency improvement over the Intel SGX SDK

## Open Source Links

- [Confidential Computing Consortium](https://confidentialcomputing.io/)
- [Open Titan](https://opentitan.org/) - open source reference design and integration guidelines for silicon root of trust 
- [Free and Open Source Silicon Foundation](https://www.fossi-foundation.org/)
- [lowRisc](https://lowrisc.org/) - Non-profit steward of Open Titan 
- [GlobalPlatform TEE Committee](https://globalplatform.org/technical-committees/trusted-execution-environment-tee-committee/)
- [Keylime](https://keylime.dev/) | :keyboard: [github](https://github.com/keylime/keylime) | :crab: [rust official keylime agent](https://github.com/keylime/rust-keylime) - Open Source remote boot attestation and runtime integrity measurement

## Confidential Compute Use Cases

Secure Multiparty Computation 

Privacy-preserving machine learning 

Cryptographic authenticity of images, video and other content - "An isolate could be used at the point of capture to execute the image signal processing computations and append signature and attestation material to the resulting image" - hashing at the point of capture
* use case begun  already for "mis/disinformation" by Coalition for Content Provenance and Authenticity (sigh)  


# References 

(2022) Verifying RISC-V Physical Memory Protection.

(2021) Towards a Trusted Execution Environment via Reconfigurable FPGA. 

(2021) Confidential Computing—a brave new world. ARM.
