# Protect data on LinuxONE With Pervasive Encryption
Protect your data on data LinuxONE using pervasive encryption with nearly no CPU overhead.

In this Code Pattern, you will build and deploy a crypto dashboard with IBM Cloud private running in the LinuxONE Community Cloud.

IBM Cloud Private is a private cloud platform for developing and running workloads locally. It is an integrated environment that enables you to design, develop, deploy and manage on-premises, containerized cloud applications behind a firewall. It includes the container orchestrator Kubernetes, a private image repository, a management console and monitoring frameworks.

When you will complete this Code Pattern, you will understand how to:
* Configure a LinuxONE Linux guest to use the hardware cryptographic acceleration.
* Use the LinuxONE crypto APIS to get monitoring data about hardware cryptographic use.
* Build a Docker image from an existing application.
* Deploy a Docker image to IBM Cloud Private.
* Run the existing application using the IBM Cloud Private catalog.
* Build an ELK Dashboard to monitor hardware cryptographic activity of LinuxONE Linux guest.

# Architecture
In this Code Pattern, the following key architecture components are required:
* A Linux Virtual Machine provisionned in the IBM LinuxONE Community cloud
* A Canonical Ubuntu 16.04.LTS (or later) Linux Virtual Machine

1. User to create first a Linux Virtual Machine on LinuxONE.
2. User to configure the Linux cryptostack in order to benefit the hardware cryptographic acceleration.
3. User to deploy and ELK stack on IBM Cloud private.
3. User to use a script in order to collect thanks to Linux crypto APIs.
4. User to create crypto dashboard thanks to ELK.

#Included components

* IBM Cloud private
* Ubuntu https://www.ubuntu.com/
* LinuxONE Crypto https://www.ibm.com/it-infrastructure/linuxone/capabilities/secure-cloud

#Featured technologies

* Docker
* IBM LinuxOne
* ELK https://www.elastic.co/fr/elk-stack

#Steps

##Step 1 - Enabling Linux to use hardware encryption

    1 - Introduction to the pervasive encryption
    2 - Introduction to the Linux crypto stack
    2 - Enabling Linux to use the Hardware
    3 - Enabling OpenSSL and OpenSSH to use the hardware acceleration support
    4 - Checking Hardware Crypto functions

##Step 2 - Build and deploy a docker image to IBM Cloud private

    Part 1 - Build the Docker image
    Part 2 - Deploy the docker image to IBM Cloud private

##Step 3 - Instantiate the banking microservice from the IBM Cloud private catalog

    Part 1 - Discover the Helm chart from the calalog
    Part 2 - Configure and install your banking microservice
    Part 3 - Access your banking microservice
