---

copyright:
  years: 2019, 2021
lastupdated: "2021-08-02"

keywords: VPN, network, encryption, authentication, algorithm, IKE, IPsec, policies, gateway, access endpoint

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:important: .important}
{:tip: .tip}
{:note: .note}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}
{:external: target="_blank" .external}

# Accessing service endpoints through VPN
{: #build-se-connectivity-using-vpn}

An IBM Cloud VPN Gateway for VPC allows you to get access to {{site.data.keyword.cloud_notm}} service endpoints from your on-premises network.
{: shortdesc}

To set up access to a service endpoint, follow these steps:

1. Get the IP of the service endpoint. IBM Cloud VPN Gateway for VPC supports two types of service endpoints: Infrastructure as a Service (IaaS) endpoints and {{site.data.keyword.cloud_notm}} service endpoints. The IaaS endpoints are hosted in the IP address ranges `161.26.0.0/16`; {{site.data.keyword.cloud_notm}} service endpoints are hosted in the IP address ranges `166.8.0.0/14`. For more information about endpoints, see [IaaS endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc#infrastructure-as-a-service-iaas-endpoints) and [Using service endpoints](/docs/account?topic=account-vrf-service-endpoint#use-service-endpoint).
1. Do one of the following:

   * **For policy-based VPN gateways** - For the VPN connection, make sure that the local subnets include the range `161.26.0.0/16` for IaaS endpoints and `166.8.0.0/14` for {{site.data.keyword.cloud_notm}} service endpoints.
   
   * **For route-based VPN gateways** - Create a connection to connect your on-premises private network, and add the following routes on your "on-premises gateway" to make sure that the traffic is going through the tunnel. No custom VPC routes are needed in IBM VPC custom routing table.
   
      * Destination - `166.8.0.0/14` for {{site.data.keyword.cloud_notm}} service endpoints, next hop: VPN tunnel interface
      * Destination - `161.26.0.0/16` for IaaS endpoints, next hop: VPN tunnel interface
   
You can narrow the range of the destination CIDR instead of using `166.8.0.0/14` or `161.26.0.0/16`. For example, if you need to access only IBM DNS IP `161.26.0.10` and `161.26.0.11`, you could choose `161.26.0.10/30` as the destination instead of using `161.26.0.0/16`.
{: tip}

For some on-premises VPN gateways, the next hop must be an IP address instead of a tunnel interface name. You should assign an IP address from a CIDR with a 30-bit mask to a tunnel interface on the on-premises VPN gateway, and use the another IP address in the same CIDR as the route's next hop. For example, you could assign `169.254.0.1/30` as the tunnel interface IP address on the on-premises VPN gateway, and use `169.254.0.2/30` as the route's next hop.
{: note}
