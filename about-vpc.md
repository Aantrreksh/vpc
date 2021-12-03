---
copyright:
  years: 2017, 2020, 2021
lastupdated: "2021-08-06"

keywords: vpc, virtual private cloud, private cloud network, quick provisioning, logical isolation, security, cloud-native, workloads, BYOIP, high availability, ACL, Access control list, block storage volumes, generation 2, gen 2

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:new_window: target="_blank"}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:download: .download}
{:table: .aria-labeledby="caption"}
{:DomainName: data-hd-keyref="DomainName"}

# Overview
{: #about-vpc}

Use {{site.data.keyword.vpc_full}} to create your own space in {{site.data.keyword.cloud}}. A virtual private cloud (VPC) is a secure, isolated virtual network that combines the security of a private cloud with the availability and scalability of IBM's public cloud.
{: shortdesc}

## Logical isolation
{: #about-vpc-logical-isolation}

VPC gives your applications logical isolation from other networks, while providing scalability and security. To make this logical isolation possible, the VPC is divided into subnets that use a range of private IP addresses. You can create subnets in suggested prefix ranges, or bring your own public IP address range (BYOIP) to your IBM Cloud account. By default, all resources within the same VPC can communicate with each other over the private network, regardless of their subnet.

## Quick instance provisioning with high network performance
{: #about-vpc-quick-instance-provisioning}

You can quickly provision scalable generation 2 compute resources in your VPC by creating *virtual server instances* with the core and RAM configuration that's best for your workload. You can select from the supported stock images or custom images that were imported from {{site.data.keyword.cos_full_notm}}. All images are cloud-init enabled. You can connect to your instance without using a password by adding *SSH keys*.

You can create instances with up to 80 Gbps network bandwidth per instance. Each instance can be multi-homed, that is, you can create multiple network interfaces per instance.

##  Multi-architecture images
{: #about-vpc-multi-architecture-images}

You can choose to create *virtual server instances* with different operating systems on x86_64 or LinuxONE (s390x processor architecture). For more information, see [Images](/docs/vpc?topic=vpc-about-images).


## Storage capabilities
{: #about-vpc-storage-capabilities}

When you create an instance, a 100 GB block storage volume is automatically attached as a primary boot volume. To add secondary data volumes to your instance, create *block storage* volumes.

## External connectivity
{: #about-vpc-external-connectivity}

Several options are available for enabling your instances to communicate with the public internet:
* To enable all instances in a subnet to send outgoing traffic, attach a *public gateway* to the subnet.
* To enable communication to and from a particular instance, independent of whether the subnet is attached to a public gateway, associate the instance with a *floating IP*.
* To enable secure connectivity, use the Virtual Private Network (VPN) service.

## Security
{: #about-vpc-security}

For instance-level protection, use *security groups* that act as virtual firewalls to restrict traffic for one or more instances. For subnet-level protection, use *access control lists* (ACLs)  to limit a subnet's inbound and outbound traffic.

## High availability
{: #about-vpc-high-availability}

A *region* is the geographical location where you deploy the VPC's services, resources, and applications. Each region contains *zones*, which are logically isolated data centers with independent infrastructures. You can deploy resources in multiple zones to achieve fault tolerance and high availability.  

Use load balancers to distribute your network traffic across a set of virtual server instances to improve performance and availability. You can set up a load balancer to distribute incoming application traffic across instances in a single zone or across multiple zones within a region.

## Classic access
{: #about-classic-access}

You can set up access from a VPC to your {{site.data.keyword.cloud_notm}} classic infrastructure, including Direct Link connectivity. One VPC per region can communicate with classic resources. For more information, see [Setting up access to classic infrastructure](/docs/vpc?topic=vpc-setting-up-access-to-classic-infrastructure).

## Next steps
{: #about-vpc-next-steps}

To get started using the API and CLI, [set up your environment](/docs/vpc?topic=vpc-set-up-environment).
To learn how to create VPC resources, see these tutorials:

* [Using the {{site.data.keyword.cloud_notm}} console to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console)
* [Using the CLI to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-cli)
* [Using the REST APIs to create VPC resources](/docs/vpc?topic=vpc-creating-a-vpc-using-the-rest-apis)

For a list of features not yet supported in VPC, see [Limitations](/docs/vpc?topic=vpc-limitations).
