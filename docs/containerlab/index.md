## Containerlab Setup

Containerlab is a container-based network emulation framework designed for the rapid deployment of virtual networking laboratories.

It enables users to quickly build realistic network topologies using several router images, the most important for our purposes being Nokia's SR SIM.

Containerlab uses Docker and Linux networking primitives to interconnect virtual nodes with high performance and minimal resource consumption.

## Main Capabilities

- Fast Topology Deployment: Laboratories can be deployed in seconds using YAML topology definitions.

- Multi-Vendor Support: Containerlab supports numerous network operating systems and Linux distributions.

- Native Linux Networking: Containerlab uses Linux bridges, namespaces, and Docker networking.

- Automation-Friendly: Topology files are fully declarative and integrate easily with Git, Ansible, and Python automation.

- Packet Captures: Packet captures can be performed directly through the CLI or through Wireshark containers using Containerlab's VSCode extension.

## Recommended Environment

Containerlab is native to Linux. As such, we recommend using it on a native Linux system or running it in an Ubuntu VM using VMware or VirtualBox.

!!! warning

    Be aware that large Containerlab deployments might consume significant resources. As such, ensure that your system has the necessary resources to run Containerlab, especially if it is running inside a virtual machine.

## Prerequisites

Before installing Containerlab, install the following:

- Docker
- VSCode

## Installation

The Containerlab team provides an up-to-date installation guide on their website: [https://containerlab.dev/install/](https://containerlab.dev/install/)

## VSCode Extension

Containerlab has an extension that allows it to be fully managed through a GUI in VSCode. This is very helpful for getting started with Containerlab and performing the laboratories as simply and efficiently as possible.

A guide for the extension is available here: [https://containerlab.dev/manual/gui/vsc-extension/](https://containerlab.dev/manual/gui/vsc-extension/)
