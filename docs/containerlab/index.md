## Containerlab Setup

Containerlab is a container-based network emulation framework designed for rapid deployment of virtual networking laboratories.

It enables users to quickly build realistic network topologies using several router images, the most important for us being Nokia´s SR SIM.

Containerlab uses Docker and Linux networking primitives to interconnect virtual nodes with high performance and minimal resource consumption.

## Main Capabilities

- Fast Topology Deployment: Labs can be deployed in seconds using YAML topology definitions.

- Multi-Vendor Support: Containerlab supports numerous network operating systems and Linux distributions.

- Native Linux Networking: Containerlab uses Linux bridges, namespaces, Docker networking.

- Automation Friendly: Topology files are fully declarative and integrate easily with Git, Ansible and Python automation.

- Packet Captures: Packet captures can be performed directly through the CLI or through WireShark containers through ContainerLab´s VSCode extension.

## Recommended Environment

Containerlab is native to Linux, as such, we recommend using it in a native Linux computer, or running it in an Ubuntu VM, using VMware or VirtualBox.

!!! warning

    Beware that large ContainerLab deployments might consume significant resources. As such, ensure that your device has the necessary resources to run ContainerLab, especially if it is a Virtual Machine.

## Prerequisites

Before installing Containerlab, install:

- Docker
- VSCode

## Installation

The ContainerLab team has an updated website on how to install their emulator here: [https://containerlab.dev/install/](https://containerlab.dev/install/)

## VSCode Extensions

ContainerLab has an extension that allows it to be fully run through a GUI in VSCode. This is very helpful to start working in ContainerLab and performing the laboratories as simply and as fast as possible. A guide for it is available in: [https://containerlab.dev/manual/gui/vsc-extension/](https://containerlab.dev/manual/gui/vsc-extension/)
