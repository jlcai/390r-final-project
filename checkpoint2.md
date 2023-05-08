# SPRING 2023 CS390R Final Project - Checkpoint #2

### WannaCry Ransomware Research

#### Server Message Block (SMB) Vulnerability
As mentioned in our previous checkpoint, we know that WannaCry depends on a vulnerability found in Windows' Server Message Block (SMB) version 1.0 protocol, and that it does not work on Windows Vista, 8, 10, or any other modern Windows release, so we set up our initial debug environment on a Windows 7 VM.

The SMB protocol is a communication protocol that is used to share access for resources on a network. SMBv.1 was developed in the 1980s and can be vulnerable to pre-authentication integrity, encryption, and other exploits if not patched (this later got patched on March 14, 2017 in security bulletin MS17-010). SMBv1 uses the MD5 (message-digest) algorithm that is a cryptographic protocol that is used to authenticate messages, content verification, and digital signatures. It takes some message of arbitrary length as an input and returns a fixed-length digest hash value as an output that is used to authenticate the original input message. This hash value that the MD5 algorithm outputs becomes important in the encryption process of WannaCry.

In the malware lecture, we talked about Eternal Blue, which is a computer exploit that was developed by the NSA to exploit the SMB protocol. Later on, this exploit was repackaged into WannaCry. 


#### Asymmetric and Symmetric Encryption
We know that the WannaCry ransomware depends on asymmetric and symmetric encryption. Although we outlined this in our initial checkpoint, this checkpoint will delve more into the intricacies of how these types of encryption work in general as well as how WannaCry utilizes it.

The aforementioned hash values that come as a result of the MD5 algorithm portion of the SMBv1 protocol were used to 
