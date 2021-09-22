---

copyright:
  years: 2021
lastupdated: "2021-04-19"

keywords: dedicated-host.00001

subcollection: vpc

---

{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:support: data-reuse='support'}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.note}
{:important: .important}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Why did a dedicated host fail to create?
{: troubleshoot}
{: support}

If you see an entry in the error log with message ID dedicated-host.00001, then the specified dedicated host has failed to create due to insufficient capacity in the requested region. 
{: shortdesc}

The dedicated host with the specified ID in the error message failed to create:
   `dedicated-host.00001: Failed to create dedicated host <Dedicated Host ID> due to insufficient capacity in zone.`
{: tsSymptoms}

The specified dedicated host could not be created due to a lack of capacity in the requested region.
{: tsCauses}


At the time of the error message, there was not enough capacity to create the specified dedicated host in the requested region. Try creating another dedicated host with different hardware requirements or in another region.
{: tsResolve}
 
