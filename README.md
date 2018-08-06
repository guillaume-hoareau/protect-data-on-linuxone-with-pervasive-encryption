# Protect data on LinuxONE with "Pervasive Encryption"
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

![Image of the Crypto Stack](https://raw.githubusercontent.com/guikarai/ELK-CPACF/master/images/architecture-crypto-icp.png)

1. User to create first a Linux Virtual Machine on LinuxONE.
2. User to configure the Linux cryptostack in order to benefit the hardware cryptographic acceleration.
3. User to deploy and ELK stack on IBM Cloud private.
3. User to use a script in order to collect thanks to Linux crypto APIs.
4. User to create crypto dashboard thanks to ELK.

# Included components

* [IBM Cloud private](https://www.ibm.com/us-en/marketplace/ibm-cloud-private/details)
* [Ubuntu](https://www.ubuntu.com/)
* [LinuxONE Crypto](https://www.ibm.com/it-infrastructure/linuxone/capabilities/secure-cloud)

# Featured technologies

* [Docker](https://www.docker.com/)
* [IBM LinuxONE](https://www.ibm.com/it-infrastructure/linuxone)
* [ELK](https://www.elastic.co/fr/elk-stack)

# Steps

## Step 1 - Enabling Linux to use hardware encryption

    1. Introduction to the pervasive encryption
    2. Introduction to the Linux crypto stack
    2. Enabling Linux to use the Hardware
    3. Enabling OpenSSL and OpenSSH to use the hardware acceleration support
    4. Checking Hardware Crypto functions

## Step 2 - Building and deploying ELK docker images to IBM Cloud private

    1. Build Docker images
    2. Deploying docker images to IBM Cloud private

## Step 3 - Deploying ELK microservice from the IBM Cloud private catalog

    1. Discover the Helm chart from the calalog
    2. Configure and install your ELK microservices
    3. Access your ELK microservice
    4. Feeding your ELK crypto dashboard
    
## Step 4 - Creating a crypto dashboard with ELK microservice

    1. Acceccing to Kibana
    2. Sourcing the ElasticSearch DataSource
    3. Creating your first search with Kibana
    4. Creating your first charts with Kibana
    5. Creating your first dashboard with Kibana
    6. Realizing your first crypto dashboard with Kibana
