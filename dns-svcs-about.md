---

copyright:
  years: 2020
lastupdated: "2020-12-02"

keywords:

subcollection: vpc

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:generic: data-hd-programlang="generic”}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Using {{site.data.keyword.dns_short}} with VPC
{: #about-dns-srvcs}

{{site.data.keyword.dns_full}} provides private DNS to Virtual Private Cloud (VPC) users. Private DNS zones are resolvable only on {{site.data.keyword.cloud_notm}}, and only from explicitly [permitted networks](/docs/dns-svcs?topic=dns-svcs-dns-concepts#permitted-networks) in an account. To get started, create a {{site.data.keyword.dns_short}} instance using the {{site.data.keyword.cloud_notm}} console.
{:shortdesc}

## {{site.data.keyword.dns_short}} overview
{: #overview-dns-srvcs}

{{site.data.keyword.dns_short}} allow you to:
  * Create private DNS zones that are collections for holding domain names.
  * Create DNS resource records under these DNS zones.
  * Specify access controls used for the DNS resolution of resource records on a zone-wide level.

{{site.data.keyword.dns_short}} also maintains its own worldwide set of DNS resolvers. Instances that are provisioned under {{site.data.keyword.cloud_notm}} on an {{site.data.keyword.cloud_notm}} network can use resource records that are configured through {{site.data.keyword.dns_full_notm}} by querying {{site.data.keyword.dns_short}} resolvers.

Resource records and zones that are configured through {{site.data.keyword.dns_short}} are:
  * Separated from the wider, public DNS and their publicly accessible records.
  * Hidden from machines outside of and not part of the {{site.data.keyword.cloud_notm}} private network.
  * Accessible only from machines that you authorize on the {{site.data.keyword.cloud_notm}} private network.
  * Resolvable only via the resolvers provided by the service.

## Before you begin
{: #dns-svcs-before-begin}

To use {{site.data.keyword.dns_short}}, you must have at least one virtual server instance in a VPC in the {{site.data.keyword.cloud_notm}}. If you do not have one, learn how to [get started with Virtual Private Cloud](/docs/vpc?topic=vpc-getting-started).


While the private DNS resolvers are required to resolve private DNS names, they also resolve public DNS names if the request is for a name that is not defined to be in a private DNS zone.
{:note}

For more information, see [Getting started with {{site.data.keyword.dns_short}}](/docs/dns-svcs?topic=dns-svcs-getting-started).
