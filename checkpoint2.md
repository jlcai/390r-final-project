# SPRING 2023 CS390R Final Project - Checkpoint #2

### WannaCry Ransomware Research
Everything that is described in this research that was conducted on existing documentation of the ransomware WannaCry and the various components that allow for the ransomware to work. Our goal is to be able to search for the sections in the ransomware binary that correspond to these different functionalities.

WannaCry depends on two different things: spreading itself through the target's network and the encryption of files on the target's machine. 

As mentioned in our previous checkpoint, we know that WannaCry depends on a vulnerability found in Windows' ***Server Message Block (SMB)*** version 1.0 protocol for the spread of itself in the network.

#### The Server Message Block (SMB) Protocol
The SMB protocol is a response-request communication protocol that is used to share access for resources on a network. It operates over TCP ports 139 and 445. The SMB protocol allow for things called SMB transactions that give permissions for read and write to be performed by a SMB client and server. These transactions become topical when it comes to exploiting the vulnerability in SMB. SMBv1 was developed in the 1980s and can be vulnerable to pre-authentication integrity, encryption, and other exploits if not patched. This got patched on March 14, 2017 in security bulletin MS17-010.
- [What is the Server Message Block (SMB) protocol? How does it work? (techtarget.com)](https://www.techtarget.com/searchnetworking/definition/Server-Message-Block-Protocol)

#### SMBv1 Vulnerabilities

##### SMBv1's MD5 Algorithm
SMBv1 uses the MD5 (message-digest) algorithm that is a cryptographic protocol that is used to authenticate messages, content verification, and digital signatures. It takes some message of arbitrary length as an input and returns a fixed-length digest hash value as an output that is used to authenticate the original input message. Since this hashing algorithm was used to encrypt files, files that were encrypted using this method have some sort of long hash value as an output.

MD5 is vulnerable to attackers because they can take an expected hash value for one file and create an entirely different file that has the exact same hash. 

SMBv1's MD5 algorithm is something that we learned about while learning about SMBv1 and its potential vulnerabilities, but as per the existing documentation of WannaCry, it does not seem that the MD5 algorithm was used for encryption purposes in the ransomware but instead just to keep track of file authenticity.
- [How to disable SMB v1 (Server Message Block) (manageengine.com)](https://www.manageengine.com/vulnerability-management/misconfiguration/legacy-protocols/how-to-disable-smb-v1.html)
- [What Is the MD5 Hashing Algorithm & How Does It Work? | Avast](https://www.avast.com/c-md5-hashing-algorithm#:~:text=Because%20a%20hacker%20can%20create,someone%20tampers%20with%20a%20file.)

##### SMB Transaction Buffer Overflow
Buffer overflows allow for attackers to move to certain parts of a program by sending large size payloads and then obtaining remote code execution.

As mentioned earlier, SMB transactions allow for read/write to be performed between a SMB client and server. If the message that is communicated between the two is greater than the SMB maximum buffer size `MaxBufferSize`, then the remaining messages are sent as secondary requests (`Secondary Trans2`), affecting the `srv2.sys` kernel driver. 
- [SMB Exploited: WannaCry Use of "EternalBlue" | Mandiant](https://www.mandiant.com/resources/blog/smb-exploited-wannacry-use-of-eternalblue)

##### WannaCry's SMBv1 Exploit and EternalBlue
In the malware lecture, we talked about Eternal Blue, which is a computer exploit that was developed by the NSA to exploit the SMB protocol. Later on, this exploit was repackaged into WannaCry. This malware spreads to unpatched pre-MS17-010 Windows systems on a network that has SMBv1 enabled, eventually allowing remote code execution. WannaCry utilizes EternalBlue by creating custom SMB session requests with specialized packets that contained hardcoded local IP values found in the ransomware.

The exploit first checks to see if the kill switch domain is available (iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com). If it is not available, then it does the following:

The ransomware scans (port scan) the target network to see if traffic is allowed on port 455 (to determine if SMB can be ran).

Afterwards, an initial SMB handshake (request/response) is done. Then, WannaCry connects to the IPC$ share -- a null session connection where Windows allows anonymous users to perform certain activities -- on the machine. 

An `NT Trans` request gets sent out after the handshaking is done, which is the transaction buffer overflow that was mentioned earlier. This buffer overflow that contains a sequence of `NOP`s moves the SMB server machine to a state where another payload can be sent for exploitation. Specifically, after the request, multiple `Secondary Trans2 Requests` attempt to accommodate the large payload size. These `Secondary Trans2` requests are malformed and act as triggers for the vulnerability where the request data portion can contain shellcode. This shellcode launches the ransomware.
- [WannaCry Malware Profile | Mandiant](https://www.mandiant.com/resources/blog/wannacry-malware-profile)
- [SMB Exploited: WannaCry Use of "EternalBlue" | Mandiant](https://www.mandiant.com/resources/blog/smb-exploited-wannacry-use-of-eternalblue)
- [How does the WannaCry malware work (tutorialspoint.com)](https://www.tutorialspoint.com/how-does-the-wannacry-malware-work)
#### WannaCry's Encryption System
We know that the WannaCry ransomware depends on asymmetric and symmetric encryption for encrypting files. Although we outlined this in our initial checkpoint, this checkpoint will delve more into the intricacies of how these types of encryption work in general as well as how WannaCry utilizes it.

##### Before Encryption: Identifying Specific Files
Here are the file extensions that are targeted, with a focus on productivity and database applications like Microsoft Excel, etc.

![img](https://content.secureworks.com/-/media/Images/Insights/Resources/Threat%20Analysis/075%20WCry%20Ransomware%20Analysis/WCry_Ransomware_Analysis_6.ashx?la=en&modified=20170517222423&hash=BF3CE71530E02C3D49691EC8537B991C)

##### Symmetric Encryption (AES)
The symmetric encryption that WannaCry uses is AES-128. This system takes data, divides it into 10 fixed-size blocks (10 since it's 128bit), processes each block using mathematical operations called rounds, and ultimately transforms all of it into ciphertext via some secret key. In WannaCry, there is only one key that is used for both encryption and decryption in the AES cryptosystem.

##### Asymmetric Encryption (RSA public-key)
The asymmetric encryption that WannaCry uses is a private RSA-2048 key pair specific to each individual infection. RSA numbers are sets of large numbers with exactly two prime factors. RSA-2048 has 2048 bits, or 617 decimal digits, and is the largest of the RSA numbers. The cryptosystem involves key generation, key distribution, encryption, and decryption. 

The following modular exponentiation for all integers *m* where $0 \leq m < n$ is:  $\displaystyle (m^{e})^{d}\equiv m{\pmod {n}}$

where the modulus *n* and public/encryption exponent *e* correspond to the public key, the modulus *n* and private/decryption exponent *d* for private key, and *m* for the message. 

Encryption works with the following modular exponentiation:

$\displaystyle c\equiv m^{e}{\pmod {n}}$

where $c$ is the ciphertext and decryption works with the following:

$\displaystyle c^{d}\equiv (m^{e})^{d}\equiv m{\pmod {n}}$.

##### WannaCry's Encryption Process
As detailed in the last checkpoint, this is the order of events that WannaCry takes as the ransomware runs:
1. makes symmetric and asymmetric keys -- *ATTACKER SIDE*
2. encrypts files with asymmetric key -- *ATTACKER SIDE*
3. encrypts asymmetric key with symmetric key -- *VICTIM SIDE*
4. send private key to attacker then delete -- *VICTIM SIDE*
5. delete private key -- *VICTIM SIDE*

#### The Spread of WannaCry
Once a machine is compromised with the ransomware, the ransomware automatically spreads itself via the target's network to other devices that are susceptible to this vulnerability.

After encryption of one machine, it installs **DoublePulsar**, a backdoor tool that runs in kernel mode allowing attackers control over a target system, as a payload that spreads copies of WannaCry onto more systems of vulnerable TCP port 455 machines.
- [What is the WannaCry Ransomware Attack? | UpGuard](https://www.upguard.com/blog/wannacry)


#### Automated Analysis
We used [PeStudio](https://www.winitor.com/) to analyze WannaCry. PeStudio is used to analyze Windows executables for malware and has a host of different features. Running it on the WannaCry executable we are told that its entropy, a measure of a malware's obfuscation, is 7.995 on a range of 0-8. Which means the malware is heavily obfuscated and packed. 