> By providing security though the lowest layers of hardware, with a minimum of dependencies, it is possible to remove the operating system and device driver vendors, platform and peripheral vendors, and service providers and their admins, from the list of required trusted parties, thereby reducing exposure to potential compromise at any point in the system lifecycle.

> With these increased protections for data-in-use, new use cases become more realistic, e.g. multi-party computations in financial and/or regulated industries or machine learning at the edge, where the data being operated needs protecting from the processing environment owner itself.

Confidential Computing provides end-to-end data protection at-rest, in-transit, and in-use. 

Confidential Computing leverages Trusted Execution Environments. A TEE is a secure area within a processorthat runs an isolated environment parrallel to the OS. 

A TEE is defined as assuring:

* **Data confidentiality**: Unauthorized entities cannot view data while it is in use within the TEE.
* **Data integrity**: Unauthorized entities cannot add, remove, or alter data while it is in use within the TEE.
* **Code integrity**: Unauthorized entities cannot add, remove, or alter code executing in the TEE.

TEE may also provide:
* code confidentiality (example an algo that is IP)
* authentication 
* code trusting 


[GCP Confidential VM](https://cloud.google.com/compute/confidential-vm/docs/about-cvm)

[Confidential Computing: An AWS Perspective](https://aws.amazon.com/blogs/security/confidential-computing-an-aws-perspective/)

[AWS Nitro Enclaves](https://aws.amazon.com/ec2/nitro/nitro-enclaves/)

[Confidential Computing Consortium](https://confidentialcomputing.io/)
