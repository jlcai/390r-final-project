# SPRING 2023 CS390R Final Project - Checkpoint #2

### WannaCry Ransomware Research
As mentioned in our previous checkpoint, we know that WannaCry depends on a vulnerability found in Windows' ***Server Message Block (SMB)*** version 1.0 protocol, and that it does not work on Windows Vista, 8, 10, or any other modern Windows release, so we set up our initial debug environment on a Windows 7 VM.

#### The Server Message Block (SMB) Protocol
The SMB protocol is a response-request communication protocol that is used to share access for resources on a network. It operates over TCP ports 139 and 445.

The SMB protocol allow for things called SMB transactions that give permissions for read and write to be performed by a SMB client and server. 

SMBv1 was developed in the 1980s and can be vulnerable to pre-authentication integrity, encryption, and other exploits if not patched. This got patched on March 14, 2017 in security bulletin MS17-010.
- [Source 1]([What is the Server Message Block (SMB) protocol? How does it work? (techtarget.com)](https://www.techtarget.com/searchnetworking/definition/Server-Message-Block-Protocol))

#### SMBv1 Vulnerabilities

##### SMBv1's MD5 Algorithm
SMBv1 uses the MD5 (message-digest) algorithm that is a cryptographic protocol that is used to authenticate messages, content verification, and digital signatures. It takes some message of arbitrary length as an input and returns a fixed-length digest hash value as an output that is used to authenticate the original input message. Since this hashing algorithm was used to encrypt files, files that were encrypted using this method have some sort of long hash value as an output.

MD5 is vulnerable to attackers because they can take an expected hash value for one file and create an entirely different file that has the exact same hash. 

SMBv1's MD5 algorithm is something that we learned about while learning about SMBv1 and its potential vulnerabilities, but as per the existing documentation of WannaCry, it does not seem that the MD5 algorithm was used for encryption purposes in the ransomware but instead just to keep track of file authenticity.
- ["The Server Message Block (SMB) v1 protocol is not disabled" Security Misconfiguration]([How to disable SMB v1 (Server Message Block) (manageengine.com)](https://www.manageengine.com/vulnerability-management/misconfiguration/legacy-protocols/how-to-disable-smb-v1.html))
- [What is MD5 and how does it work?]([What Is the MD5 Hashing Algorithm & How Does It Work? | Avast](https://www.avast.com/c-md5-hashing-algorithm#:~:text=Because%20a%20hacker%20can%20create,someone%20tampers%20with%20a%20file.))

##### SMB Transaction Buffer Overflow
Buffer overflows allow for attackers to move to certain parts of a program by sending large size payloads and then obtaining remote code execution.

As mentioned earlier, SMB transactions allow for read/write to be performed between a SMB client and server. If the message that is communicated between the two is greater than the SMB maximum buffer size `MaxBufferSize`, then the remaining messages are sent as secondary requests (`Secondary Trans2`), affecting the `srv2.sys` kernel driver. 
- [Source]([SMB Exploited: WannaCry Use of "EternalBlue" | Mandiant](https://www.mandiant.com/resources/blog/smb-exploited-wannacry-use-of-eternalblue))

##### WannaCry's SMBv1 Exploit and EternalBlue
In the malware lecture, we talked about Eternal Blue, which is a computer exploit that was developed by the NSA to exploit the SMB protocol. Later on, this exploit was repackaged into WannaCry. This malware spreads to unpatched pre-MS17-010 Windows systems on a network that has SMBv1 enabled, eventually allowing remote code execution. WannaCry utilizes EternalBlue by creating custom SMB session requests with specialized packets that contained hardcoded local IP values found in the ransomware.

The exploit begins with an initial SMB handshake,
- [WannaCry Malware Profile]([WannaCry Malware Profile | Mandiant](https://www.mandiant.com/resources/blog/wannacry-malware-profile))
- [WannaCry's use of Eternal Blue]([SMB Exploited: WannaCry Use of "EternalBlue" | Mandiant](https://www.mandiant.com/resources/blog/smb-exploited-wannacry-use-of-eternalblue))

---

#### Asymmetric and Symmetric Encryption
We know that the WannaCry ransomware depends on asymmetric and symmetric encryption. Although we outlined this in our initial checkpoint, this checkpoint will delve more into the intricacies of how these types of encryption work in general as well as how WannaCry utilizes it.

The aforementioned hash values that come as a result of the MD5 algorithm portion of the SMBv1 protocol were used to 

