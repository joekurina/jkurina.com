+++
title = 'Say Aloha to Aloha-HE'
author = 'Joe'
date = 2024-10-20T21:52:20-07:00
draft = false
+++

## Introduction

In this post we'll be attempting to install, run, and analyze the Aloha-HE implementation on the PYNQ-Z2 development board as instructed on the project README here: https://github.com/flokrieger/Aloha-HE

Full disclosure: I haven't been able to get this working yet, but it's my hope that by documenting my process here, I'll be able to figure it out (or get some help) without polluting the authors' inbox or github issues page with a lot of back-and-forth.

## Project Overview
Let's start off by reading their paper and going over the fundamentals of what homomorphic encryption is and how it's accomplished: [Aloha-HE: A Low-Area Hardware Accelerator for Client-Side Operations in Homomorphic Encryption](https://eprint.iacr.org/2023/1736).

In a nutshell, homomorphic encryption allows you to perform computations on encrypted data without having to decrypt it.
This is a huge win for data privacy! I'll get in to the math behind this a little more in future posts, but the gist is that the plaintext data is first encoded into large-order polynomials in a quotient ring and then multiplied by a public-key along with samples from a noise distribution to create a cyphertext pair. Herein lies the motivation for a hardware accelerator; expanding and encoding the plaintext data into a massive polynomial vector increases the size and performing polynomial multiplication introduces a huge computational overhead.

Enter the Number Theoretic Transform (NTT)! I will get into the nitty-gritty of this operation in a future post, but what it does for us is simplify polynomial multiplication by allowing us to use element-wise multiplication on our transformed vectors. Here is where we can take advantage of hardware accleration! FPGAs really shine in operations like these which can be *parallelized* and *pipelined*. Below is the overall architecture of this accelerator:

![The Aloha-HE Architecture](/aloha-he-architecture.PNG)

So that's the accelerator, lets get it running and then later explore the details of its implementation!

## Prerequisites
Per the github README, this project was built using Vivado 2019.1 for the PYNQ-Z2 and Genesys2 boards.
Here's my environment:
- Ubuntu 24.04.1 LTS
- [Vivado 2019.1](https://www.xilinx.com/member/forms/download/xef-vivado.html?filename=Xilinx_Vivado_SDK_2019.1_0524_1430.tar.gz)
- PYNQ-Z2 \
![The PYNQ-Z2](/PYNQZ2.jpeg)

## Installation Caveats

Right off the bat, we run into some problems. First, installing Vivado on Ubuntu has been a fraught endeavor for years; at a certain point the installer will just hang. Fortunately this is a headache I have endured in the past and I knew that I needed to search google for a post containing the packages required: **libncurses5** and **libtinfo5**. If you're following along with me, <u>*make sure that these packages are installed before you begin!*</u>

With that out of the way, something that the README fails to point out is that the build is going to assume that you're installing your toolchain in: \
/opt/Xilinx/ \
However, the default location suggested by the Vivado installer is: \
/tools/Xilinx/ \
This isn't a big deal, but it's something we need to be aware of if we want to avoid an angry linker trying to find our libraries in folders that don't exist in the near future. Little things like this can be a real time-sink, and my job as a crash-test-dummy is to run into these problems before you do and give a heads up! For the sake of using the Aloha-HE distribution AS-IS, and the fact that I prefer to use existing conventions on my system, I'm going to set the default installation location in: \
/opt/Xilinx/ \
\
![Vivado Install Dir](/vivado_install.png)
