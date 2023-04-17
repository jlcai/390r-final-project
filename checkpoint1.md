# SPRING 2023 CS390R Final Project - Checkpoint #1

## Table of Contents
- [General Overview](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md#general-overview)
- [Set-Up Debug Environment](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md#set-up-debug-environment)
- [Map Out Target Code-Base](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md#map-out-target-code-base)
- [Plans for Rest of Project](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md#plans-for-rest-of-project)
- [Resources and Research](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md#resources-and-research)

-----

### General Overview
_**Instruction:** Provide a short general overview of your target. What are its intended use-cases,
what type of features does it support, etc._

We want to understand how malware works by reversing it. Our plan involves using the Practical Malware Analysis textbook and examining a sample of malware. Our initial target is [**WannaCry**](https://en.wikipedia.org/wiki/WannaCry_ransomware_attack), a well-documented ransomware cryptoworm that targets computers, encrypts their files, and asks for Bitcoin as remittence for decryption. We aim to understand the structure of malware by analyzing the decompiled binary in Ghidra. Although the source code of WannaCry is available online, we want to gain a comprehensive understanding of it by going through the entire process of reversing and examining the binary.

We know that WannaCry is based off asymmetric and symmetric encryption. The symmetric encryption employs a single key for encryption and decryption (AES encryption system). The asymmetric encryption portion employs two keys: one public, one private. The public key encrypts while the private key decrypts (RSA public-key encryption system). This is how our initial target works:
1. makes symmetric and asymmetric keys (attacker side)
2. encrypts files with asymmetric (attacker side)
3. encrypts asymmetric key with symmetric key (victim side)
4. send private key to attacker then delete (victim side)
5. delete private key (victim side)

Since WannaCry has a built-in kill switch that is (supposedly) easy to switch and requires the Internet to check for the domain name iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com, we want to look at harder samples of malware after this target to look at and reverse.

[Back to Top](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md)

-----

### Set-Up Debug Environment
_**Instruction:** Set up a debug environment for your target. If you are targeting an embedded
device this might include setting up emulation, for windows targets you might need a
windows debugger, and in some cases you may want to play around with compile
flags to set up source code debugging if possible. Do what you can given your
target, describe what you did, and upload required files for this to your team’s github
repository._

We plan on setting up a virtual machine for everyone to work off of. Since WannaCry depends on a vulnerability found in Windows' [Server Message Block (SMB) protocol](https://www.cisecurity.org/insights/blog/commonly-exploited-protocols-server-message-block-smb), we would need to set up a Windows environment to debug. WannaCry also does not work on Windows Vista, 8, 10, or any other modern Windows release, so we found a [Windows 7 iso](https://www.softlay.com/downloads/windows-7-ultimate) to work off of. 
We also found [FlareVM](https://github.com/mandiant/flare-vm) as a potential tool for setting up the reversing environment on a VM. 

After getting the VM set up with Windows 7, we first had to install Firefox since Internet Explorer was too outdated to function. We got Ghidra to statically analyze the virus, then downloaded the WannaCry binary from the Github repository linked later on. After downloading, I disconnected the network connection between the VM and the host while also diabling the auto-connect setting VMware has enabled by default. 

![Windows 7 VM](https://github.com/jlcai/390r-final-project/blob/main/screenshots/win7_vm_ss.png?raw=true)

[Back to Top](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md)

-----

### Map Out Target Code-Base
_**Instruction:** Map out the targets code-base. Your target probably has many source directories
with various different files. In this part we want you to describe the general layout
and purpose of different files, major functions, etc. From this we should geta good
idea of where exactly in the target-code we should look to find specific features,
code-sections, etc._

We have a sample of the WannaCry binary [here](https://github.com/ytisf/theZoo/tree/master/malware/Binaries/Ransomware.WannaCry) from theZoo Github repository. There are many samples of this live malware on the Internet as well as source code. We are not going to explicitly look at the source code so not to be spoiled by it itself, so we are going to be doing manual reversing on this sample. The password for the zip file is `infected`, as given by the `.pass` file in the directory.

[Back to Top](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md)

-----

### Plans for Rest of Project
_**Instruction:** What are your plans for the rest of the project?_

As mentioned previously in the general overview section, we plan to use WannaCry as an initial stepping stone to understand how to reverse malware. We plan on finding another sample later to reverse engineer -- with the sample not being as well-documented as WannaCry is already.

[Back to Top](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md)

-----

### Resources and Research
- [Textbook] Practical Malware Analysis: The Hands-On Guide to Dissecting Malicious Software by Michael Sikorski and Andrew Honig
- [FlareVM](https://github.com/mandiant/flare-vm) for setting up a reverse engineering environment on a VM w/ helpful scripts
- [WannaCry binary](https://github.com/ytisf/theZoo/tree/master/malware/Binaries/Ransomware.WannaCry) from theZoo GitHub repository
- [Windows 7 iso](https://www.softlay.com/downloads/windows-7-ultimate)

[Back to Top](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md)
