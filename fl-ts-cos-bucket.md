---

copyright:
  years: 2020, 2021
lastupdated: "2021-05-25"

keywords: is.flow-log-collector.00003E

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
{:important: .important}
{:note:.note}
{:troubleshoot: data-hd-content-type='troubleshoot'}

# Why can't the Cloud Object Storage bucket be found?
{: #fl-ts-error-cos-bucket}
{: troubleshoot}
{: support}

A flow log collector requires a {{site.data.keyword.cos_full_notm}} bucket to be created and accessible. If you see the error log with message ID `is.flow-log-collector.00003E`, the bucket does not exist, or is no longer accessible. The flow log collector cannot publish data to the Cloud Object Storage bucket.
{: shortdesc}

To avoid lost data, create a Cloud Object Storage bucket within the next 24 hours to correct this problem.
{: important}

The flow log collector lost connection to a Cloud Object Storage bucket:
   `is.flow-log-collector.00003E: Cloud Object Storage bucket <BucketName> was not found`
{: tsSymptoms}

A bucket that is associated with a flow log collector was deleted, or is not accessible by a flow log collector.
{: tsCauses}

Follow these steps to resolve this issue:
{: tsResolve}

1.	Create a Cloud Object Storage bucket with the same `<BucketName>` specified in the error message. To create a Cloud Object Storage bucket, see the [Cloud Object Storage](https://cloud.ibm.com/catalog/services/cloud-object-storage) ordering page.

   The Cloud Object Storage bucket must be a single-region bucket in the same region as the target resource. Additionally, it is recommended that you secure the bucket through IAM access groups and audit logging.
   {: note}
   
1. Check to make sure that you defined an authorization between the flow log collector and the Cloud Object Storage bucket so that the flow log collector can publish data. To define an authorization, use the following steps:

   * In the {{site.data.keyword.cloud_notm}} console, click **Manage** &gt; **Access (IAM)**.
   * Select **Authorizations** from the navigation pane.
   * Click **Create**.
   * For **Source service**:
      * Select **VPC Infrastructure Services**. Then, select **Services based on attributes**.
      * Select **Resource type**. Then, select **Flow Logs for VPC**.
      * Select **Source resource instance** and choose an option.
   * For **Target service**:
      * Select **Cloud Object Storage**. Then, select **Services based on attributes**.
      * Select **Service instance** and choose an option.
   * For **Service access**, select the **Writer** role.
   * Click **Authorize**.

For more information, see [Creating a flow log collector](/docs/vpc?topic=vpc-ordering-flow-log-collector).
{: note}
