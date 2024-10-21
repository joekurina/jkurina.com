+++
title = 'Say Aloha to Aloha-HE'
author = 'Joe'
date = 2024-10-20T21:52:20-07:00
draft = true
+++

## Introduction

In this post we'll be attempting to install, run, and analyze the Aloha-HE implementation on the PYNQ-Z2 development board as instructed on the project README here: https://github.com/flokrieger/Aloha-HE

Full disclosure: I haven't been able to get this working yet, but it's my hope that by documenting my process here, I'll be able to figure it out (or get some help) without polluting their inbox or github issues page with a lot of back-and-forth.

## Project Overview


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
