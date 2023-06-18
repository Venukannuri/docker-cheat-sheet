# The Docker Handbook
Learn Docker for Beginners

# Table of Contents
* **[Introduction](#introduction)**
    * **[What is Docker?](#what-is-docker)**
    * **[Docker Engine](#docker-engine)**
    * **[Docker Architecture](#docker-architecture)**
    * **[Docker Objects](#docker-objects)**
    * **[The underlying technology](#the-underlying-technology)**

# Introduction

### What is Docker?

Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly.

### Docker Engine

Docker Engine is a *client-server* application with these major components:

* A server which is a type of long-running program called a daemon process (the dockerd command).

* A REST API which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.

* A command line interface (CLI) client (the docker command).

<p align="center">
  <img src="images/engine-components-flow.png">
</p>


### Docker Architecture

As previously mentioned, Docker uses a **client-server** architecture.
* The **Docker client** talks to the **Docker daemon**, which does the heavy lifting of building, running, and distributing your Docker containers.
* The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon.
* The Docker client and daemon communicate using a **REST API**, over UNIX sockets or a network interface.

<p align="center">
  <img src="images/architecture.svg">
</p>

### Docker Objects

When you use Docker, you are creating and using **images**, **containers**, **networks**, **volumes**, **plugins**, and **other objects**. This section is a brief overview of some of those objects.

* **Image:**
    * An _image_ is a lightweight, stand-alone, executable package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and config files.

    * An image is a read-only template with instructions for creating a Docker container. Often, an image is based on another image, with some additional customization.

    * You might create your own images. To do that, you create a **Dockerfile** with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a Dockerfile creates a layer in the image.

* **Container:**
    * A _container_ is a runtime instance of an image.
    * **Isolation:** It **runs completely isolated** from the host environment by default, only accessing host files and ports if configured to do so. By default, a container is relatively well isolated from other containers and its host machine. You can control how isolated a container’s network, storage, or other underlying subsystems are from other containers or from the host machine.
    * Containers **run apps natively** on the host machine’s kernel.
    * They have **better performance** characteristics than virtual machines that only get virtual access to host resources through a hypervisor.
    * Containers can get native access, each one running in a discrete process, taking no more memory than any other executable.

### The underlying technology

Docker is written in **Go** and takes advantage of several features of the Linux kernel to deliver its functionality.

* **Namespaces**

Docker uses a technology called **namespaces** to provide the isolated workspace called the container. When you run a container, Docker creates a set of namespaces for that container.

These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

**Docker Engine** uses namespaces such as the following on Linux:

* **The `pid` namespace:** Process isolation (PID: Process ID).
* **The `net` namespace:** Managing network interfaces (NET: Networking).
* **The `ipc` namespace:** Managing access to IPC resources (IPC: InterProcess Communication).
* **The `mnt` namespace:** Managing filesystem mount points (MNT: Mount).
* **The `uts` namespace:** Isolating kernel and version identifiers. (UTS: Unix Timesharing System).

* **Control Groups**

Docker Engine on Linux also relies on another technology called control groups (**cgroups**). A **cgroup** limits an application to a specific set of resources. Control groups allow Docker Engine to share available hardware resources to containers and optionally enforce limits and constraints. For example, you can limit the memory available to a specific container.

* **Union File Systems**

Union file systems, or **UnionFS**, are file systems that operate by **creating layers**, making them **very lightweight** and **fast**. Docker Engine uses UnionFS to provide the building blocks for containers. Docker Engine can use multiple UnionFS variants, including AUFS, btrfs, vfs, and DeviceMapper.

* **Container Format**

Docker Engine combines the namespaces, control groups, and UnionFS into a wrapper called a **container format**. The default container format is **libcontainer**. In the future, Docker may support other container formats by integrating with technologies such as BSD Jails or Solaris Zones.
