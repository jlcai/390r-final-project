# SPRING 2023 CS390R Final Project - Checkpoint #1

## Table of Contents
- [General Overview](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md#general-overview)
- [Set-Up Debug Environment](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md#set-up-debug-environment)
- [Map Out Target Code-Base](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md#map-out-target-code-base)
- [Plans for Rest of Project](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md#plans-for-rest-of-project)
- [Resources and Research](https://github.com/jlcai/390r-final-project/blob/main/checkpoint1.md#resources-and-research)

-----

### General Overview
<sub>**Instruction:** Provide a short general overview of your target. What are its intended use-cases,
what type of features does it support, etc.</sup>

We want to be able to understand how malware works via reversing. We plan on doing research via the Practical Malware Analysis textbook as well as looking at a target sample of malware. Our initial target is the well-documented malware [**WannaCry**](https://en.wikipedia.org/wiki/WannaCry_ransomware_attack). WannaCry is a ransomware cryptoworm that targets computers, encrypts their files, and asks for Bitcoin as remittence for decryption. We want to understand how malware is structured by looking at the decompiled binary in Ghidra. Although the source code of WannaCry exists online, we want to be able to understand it through the whole process of reversing and looking at the binary itself.

Since WannaCry has a built-in kill switch, checking the domain name iuqerfsodp9ifjaposdfjhgosurijfaewrwergwea.com, we want to look at harder samples of malware after this target to look at and reverse.

-----

### Set-Up Debug Environment
<sub>**Instruction:** Set up a debug environment for your target. If you are targeting an embedded
device this might include setting up emulation, for windows targets you might need a
windows debugger, and in some cases you may want to play around with compile
flags to set up source code debugging if possible. Do what you can given your
target, describe what you did, and upload required files for this to your team’s github
repository.</sub>

-----

### Map Out Target Code-Base
<sub>**Instruction:** Map out the targets code-base. Your target probably has many source directories
with various different files. In this part we want you to describe the general layout
and purpose of different files, major functions, etc. From this we should geta good
idea of where exactly in the target-code we should look to find specific features,
code-sections, etc.</sub>


-----

### Plans for Rest of Project
<sub>**Instruction:** What are your plans for the rest of the project?</sub>

-----

### Resources & Research
- [Textbook] Practical Malware Analysis: The Hands-On Guide to Dissecting Malicious Software by Michael Sikorski and Andrew Honig
- [FlareVM](https://github.com/mandiant/flare-vm) for setting up a reverse engineering environment on a VM w/ helpful scripts
- [WannaCry binary](https://github.com/ytisf/theZoo/tree/master/malware/Binaries/Ransomware.WannaCry) from theZoo GitHub repository
