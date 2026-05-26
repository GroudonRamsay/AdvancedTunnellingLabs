# Containerlab Setup

Containerlab is a container-based network emulation framework designed for rapid deployment of virtual networking laboratories.

It enables engineers to quickly build realistic network topologies using several router images, the most important for us being Nokia´s SR SIM.

Containerlab uses Docker and Linux networking primitives to interconnect virtual nodes with high performance and minimal resource consumption.

# Main Capabilities

- Fast Topology Deployment: Labs can be deployed in seconds using YAML topology definitions.

- Multi-Vendor Support: Containerlab supports numerous network operating systems and Linux distributions.

- Native Linux Networking: Containerlab uses Linux bridges, namespaces, Docker networking.

- Automation Friendly: Topology files are fully declarative and integrate easily with Git, Ansible and Python automation.

- Packet Captures: Packet captures can be performed directly through the CLI or through WireShark containers through ContainerLab´s VSCode extension.

# Recommended Environment

Containerlab is native to Linux, as such, we recommend using it in a native Linux computer, or running it in an Ubuntu VM, using VMware or VirtualBox.

!!! warning

    Beware that large ContainerLab deployments might consume significant resources. As such, ensure that your device has the necessary resources to run ContainerLab, especially if it is a Virtual Machine.

# Prerequisites

Before installing Containerlab, install:

- Docker
- VSCode

# Installation

The ContainerLab team has prepared a command that should install all necessary components:

```bash
curl -sL https://containerlab.dev/setup | sudo -E bash -s "all"
```

# VSCode Extensions

ContainerLab has an extension that allows it to be fully run through a GUI in VSCode. This is very helpful to start working in ContainerLab and performing the laboratories as simply as possible.
Besides the ContainerLab extension, we recommend others that help making its usage significantly better:

| Extension    | Purpose            |
| ------------ | ------------------ |
| Containerlab | Topology support   |
| YAML         | YAML syntax        |
| Docker       | Docker integration |

# Installing the ContainerLab VSCode Extension

Open VSCode.

Navigate to:

```text
Extensions → Search "Containerlab"
```

Install:

```text
Containerlab
by srl-labs
```

# Features of the VSCode Extension

The extension provides tools to visualize your topology and undeployed labs, deploy, destroy and redeploy your lab, access your devices through Shell and SSH and capture traffic between your devices with an in-built docker container with a WireShark image.

# Using Containerlab with VSCode

Create the folder where you want your lab to be in.

```text
Explorer → New Folder → clab-test
```

Create a topology file:

```text
clab-test.clab.yml
```

A base topology for two Alpine devices connected to each other is as follows:

```yaml
name: clab-test

topology:
  nodes:
    r1:
      kind: linux
      image: alpine

    r2:
      kind: linux
      image: alpine

  links:
    - endpoints: ["r1:eth1", "r2:eth1"]
```

The ContainerLab extension allows topology deployment directly from the editor.

Right-click the topology file and select:

```text
Deploy
```

To close your laboratory select the topology file again and select:

```text
Destroy
```

Another way of using this, is by going directly to the ContainerLab tab in VSCode, choosing the undeployed lab, and deploying it from there.

To configure a router directly through its CLI, you can access it through the extension, selecting the SSH option.

To capture packets in your lab, choose the port where the capture will occur and select the Capture option.

# Best Practices

- Use Git for topology versioning
- Keep configs in separate folders
- Use consistent interface naming
- Capture traffic frequently to troubleshoot your labs
- Validate Linux bridges carefully
- Document addressing schemes
