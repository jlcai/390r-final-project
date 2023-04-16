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

We want to be able to understand how malware works via reversing. We plan on doing research via the Practical Malware Analysis textbook as well as looking at a target sample of malware. Our initial target is the well-documented malware [**WannaCry**](https://en.wikipedia.org/wiki/WannaCry_ransomware_attack). WannaCry is a ransomware cryptoworm that targets computers, encrypts their files, and asks for Bitcoin as remittence for decryption. We want to understand how malware is structured by looking at the decompiled binary in Ghidra. Although the source code of WannaCry exists online, we want to be able to understand it through the whole process of reversing and looking at the binary itself.

We know that WannaCry is based off asymmetric and symmetric encryption. In the symmetric portion, there is one key that is used for encryption and decryption and uses the AES encryption system. For the asymmetric encryption portion, there are two keys: one public, one private. The public key encrypts while the private key decrypts (RSA public-key encryption system). This is how our initial target works:
1. makes symmetric and asymmetric keys (attacker side)
2. encrypts files with asymmetric (attacker side)
3. encrypts asymmetric key with symmetric key (victim side)
4. send private key to attacker then delete (victim side)
5. delete private key (victim side)

Since WannaCry has a built-in kill switch that is (supposedly) easy to switch and requires the Internet to check for the domain name iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com, we want to look at harder samples of malware after this target to look at and reverse.

-----

### Set-Up Debug Environment
_**Instruction:** Set up a debug environment for your target. If you are targeting an embedded
device this might include setting up emulation, for windows targets you might need a
windows debugger, and in some cases you may want to play around with compile
flags to set up source code debugging if possible. Do what you can given your
target, describe what you did, and upload required files for this to your team’s github
repository._

We plan on setting up a virtual machine for everyone to work off of. Since WannaCry depends on a vulnerability found in Windows' [Server Message Block (SMB) protocol](https://www.cisecurity.org/insights/blog/commonly-exploited-protocols-server-message-block-smb), we would need to set up a Windows environment to debug. WannaCry also does not work on Windows Vista, 8, 10, or any other modern Windows release, so we found a Windows 7 iso to work off of. 
We also found [FlareVM](https://github.com/mandiant/flare-vm) as a potential tool for setting up the reversing environment on a VM. 

-----

### Map Out Target Code-Base
_**Instruction:** Map out the targets code-base. Your target probably has many source directories
with various different files. In this part we want you to describe the general layout
and purpose of different files, major functions, etc. From this we should geta good
idea of where exactly in the target-code we should look to find specific features,
code-sections, etc._

We have a sample of the binary [here](https://github.com/ytisf/theZoo/tree/master/malware/Binaries/Ransomware.WannaCry) from theZoo Github repository. There are many samples of this live malware on the Internet as well as source code. We are not going to explicitly look at the source code so not to be spoiled by it itself, so we are going to be doing manual reversing on this sample.

-----

### Plans for Rest of Project
_**Instruction:** What are your plans for the rest of the project?_

-----

### Resources and Research
- [Textbook] Practical Malware Analysis: The Hands-On Guide to Dissecting Malicious Software by Michael Sikorski and Andrew Honig
- [FlareVM](https://github.com/mandiant/flare-vm) for setting up a reverse engineering environment on a VM w/ helpful scripts
- [WannaCry binary](https://github.com/ytisf/theZoo/tree/master/malware/Binaries/Ransomware.WannaCry) from theZoo GitHub repository
- [Windows 7 iso](https://www.softlay.com/downloads/windows-7-ultimate)
