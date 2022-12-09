---

copyright:
  years: 2019, 2022
lastupdated: "2022-12-09"

keywords:

subcollection: vpc

---
{{site.data.keyword.attribute-definition-list}}

# FAQs for {{site.data.keyword.block_storage_is_short}}
{: #block-storage-vpc-faq}

The following questions often arise about the {{site.data.keyword.block_storage_is_short}} service including performance and security. If you have other questions you'd like to see answered here, provide feedback by using the **Open doc issue** or **Edit topic** links.
{: shortdesc}

## Offering questions
{: #block-storage-vpc-offering-questions}

### How does {{site.data.keyword.block_storage_is_short}} prevent a single point of failure? What mechanism assures data durability?
{: faq}
{: #faq-block-storage-durability}

{{site.data.keyword.block_storage_is_short}} volume data is stored redundantly across multiple physical disks in an Availability Zone to prevent data loss due to failure of any single component.

### How are volumes created and attached to an instance?
{: faq}
{: #faq-block-storage-1}
{: support}

When you create a virtual server instance, you can [create a {{site.data.keyword.block_storage_is_short}} volume](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi) that is attached to that instance. You can also [create stand-alone volumes](/docs/vpc?topic=vpc-creating-block-storage#create-standalone-vol) and later attach them to your instances.

### How many instances can share a provisioned {{site.data.keyword.block_storage_is_short}} volume?
{: faq}
{: #faq-block-storage-2}

A {{site.data.keyword.block_storage_is_short}} volume can be attached to only one instance at a time. Instances cannot share a volume.

### How many {{site.data.keyword.block_storage_is_short}} secondary data volumes can be attached to an instance?
{: faq}
{: #faq-block-storage-3}

You can create 12 {{site.data.keyword.block_storage_is_short}} data volumes per instance, plus the boot volume.

### How am I charged for usage?
{: faq}
{: #faq-block-storage-6}
{: support}

Cost for {{site.data.keyword.block_storage_is_short}} is calculated based on GB capacity that is stored per month, unless the duration is less than one month. The volume exists on the account until you delete the volume or you reach the end of a billing cycle, whichever comes first.

Pricing is also affected when you [expand volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) or [adjust IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) by specifying a different IOPS profile. For example, expanding volume capacity increases costs while changing an IOPS profile from a 5 IOPS/GB tier to a 3 IOPS/GB tier decreases the monthly and hourly rate.  Billing for an updated volume is automatically updated to add the prorated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

Pricing for {{site.data.keyword.block_storage_is_short}} volumes is also set by region. For more information, see [Pricing](https://www.ibm.com/cloud/vpc/pricing){: external}.

### Are there limits on the number of volumes I can create?
{: faq}
{: #faq-block-storage-7}

You can create up to 300 total {{site.data.keyword.block_storage_is_short}} volumes (data and boot) per account in a region. To increase this [quota](/docs/vpc?topic=vpc-quotas#block-storage-quotas), open a [support case](/docs/vpc?topic=vpc-getting-help) and specify the zone where you need more volumes.

### After a data volume is created with a specific capacity, can the capacity later be increased?
{: faq}
{: #faq-block-storage-8}
{: support}

You can increase the capacity of data volumes that are attached to a virtual server instance. You can indicate capacity in GB increments up to 16,000 GB capacity, depending on your volume profile. For more information, see [Increasing {{site.data.keyword.block_storage_is_short}} volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

### Can I increase capacity of a boot volume?
{: faq}
{: #faq-block-storge-rbv}

Boot volume capacity can be increased during instance provisioning or later, by directly modifying the boot volume. This feature applies to instances that are created from stock or custom images. You can also specify a larger boot volume capacity when you create an instance template. The boot volume can't be unattached from an instance (that is, stored as stand-alone data volume). For more information, see [Increasing boot volume volume capacity](/docs/vpc?topic=vpc-resize-boot-volumes).

### Can I change boot volume capacity for an existing instance?
{: faq}
{: #faq-block-storge-rbv-instance}

Yes, boot volume capacity can be increased for an existing instance. However, you increase capacity on the storage side. That is, by modifying the boot volume details. For example, in the UI, you'd select a boot volume from the list of {{site.data.keyword.block_storage_is_short}} volumes and then resize the volume from the volume details page. For more information, see [Increase boot volume capacity from the list of {{site.data.keyword.block_storage_is_short}} volumes in the UI](/docs/vpc?topic=vpc-resize-boot-volumes&interface=ui#resize-boot-vol-list-ui). You can also use the [CLI](/docs/vpc?topic=vpc-resize-boot-volumes&interface=cli#expand-existing-boot-vol-cli) or the [API](/docs/vpc?topic=vpc-resize-boot-volumes&interface=api#expand-existing-boot-vol-api).

### How many volumes can I provision on my account?
{: faq}
{: #faq-block-storage-12}

You can provision up to 300 {{site.data.keyword.block_storage_is_short}} volumes per account in a region. You can request your quota to be increased by opening a [support case](/unifiedsupport/cases/form){: external} and specifying the region where you need more volumes. For more information about preparing a support case when you're ordering {{site.data.keyword.block_storage_is_short}} volumes or requesting an increase to your volume or capacity limits, see [Managing volume count and capacity limits](/docs/vpc?topic=vpc-manage-storage-limit).

### Can I set up shared storage in a multizone cluster?
{: faq}
{: #faq-block-storage-18}
{: support}

In the {{site.data.keyword.cloud}}, storage options are limited to an availability zone. Do not try to manage shared storage across multiple zones.

Instead, use an {{site.data.keyword.cloud}} classic service option outside a VPC such as {{site.data.keyword.cos_full}} or {{site.data.keyword.cloudantfull}} if you must share your data across multiple zones and regions.

### I have volumes on the Classic infrastructure. Can I port them to the VPC?
{: faq}
{: #faq-block-storage-19}
{: support}

No. The VPC provides access to new availability zones in multi-zone regions. Compute, network, and storage resources are designed to function in the VPC.

### What is _image from volume_ and how does it relate to {{site.data.keyword.block_storage_is_short}} volumes?
{: faq}
{: #faq-block-storage-ifv}

With the image from volume feature, you can create a custom image directly from a {{site.data.keyword.block_storage_is_short}} boot volume. Then, you can use the custom image to provision other virtual server instances. For more information, see [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc).

## Volume management questions
{: #block-storage-vpc-volume-questions}

### How is the boot disk created for an instance and how does it relate to the virtual machine image?
{: faq}
{: #faq-block-storage-14}

The boot disk, also called a boot volume, is created when you provision a virtual server instance. The boot disk of an instance is a cloned image of the virtual machine image. For stock images, the boot volume capacity is 100 GB. If you are importing a custom image, the boot volume capacity can be 10 GB to 250 GB, depending on what the image requires. Images smaller than 10 GB are rounded up to 10 GB. The boot volume is deleted when you delete the instance to which it is attached.

### When can I delete a {{site.data.keyword.block_storage_is_short}} data volume?
{: faq}
{: #faq-block-storage-15}

You can delete a {{site.data.keyword.block_storage_is_short}} data volume only when it isn't attached to a virtual server instance. [Detach the volume](/docs/vpc?topic=vpc-managing-block-storage#detach) before you delete it. Boot volumes are detached and deleted when the instance is deleted.

### What happens to my data when I delete a {{site.data.keyword.block_storage_is_short}} data volume?
{: faq}
{: #faq-block-storage-16}

When you delete a {{site.data.keyword.block_storage_is_short}} volume, your data immediately becomes inaccessible. All pointers to the data on that volume are removed. The inaccessible data is eventually overwritten as new data is written to the data block. IBM guarantees that data deleted cannot be accessed and that deleted data is eventually overwritten. For more information, see [{{site.data.keyword.block_storage_is_short}} data eradication](/docs/vpc?topic=vpc-managing-block-storage#block-storage-data-eradication).

### I have compliance requirements. What can I do to ensure that my data is inaccessible?
{: faq}
{: #faq-block-storage-nist}

IBM guarantees that your data is completely inaccessible on the physical disk and is eventually [eradicated](/docs/vpc?topic=vpc-managing-block-storage#block-storage-data-eradication). If you have extra compliance requirements such as NIST 800-88 Guidelines for Media Sanitization, you must perform data sanitation procedures before you delete your volumes. For more information, see the [NIST 800-88 Guidelines for Media Sanitation](https://csrc.nist.gov/publications/detail/sp/800-88/rev-1/final){: external}.

### What rules apply to volume names and can I rename a volume later on?
{: faq}
{: #faq-block-storage-9}

Valid volume names can include a combination of lowercase alphanumeric characters (a-z, 0-9) and the hyphen (-), up to 63 characters. Volume names must begin with a lowercase letter and be unique across the entire VPC infrastructure.

You can change the name of an existing volume in the UI. For more information, see [Managing {{site.data.keyword.block_storage_is_short}}](/docs/vpc?topic=vpc-managing-block-storage#rename).

### Does the volume need to be pre-warmed to achieve expected throughput?
{: faq}
{: #faq-block-storage-10}

You do not have to pre-warm a volume. You can see the specified throughput immediately upon provisioning the volume when you create the volume from an image.
You can experience degraded performance when you provision the volume by restoring a snapshot. 

### What is a {{site.data.keyword.block_storage_is_short}} snapshot?
{: faq}
{: #faq-block-storage-snapshot}

Snapshots are a point-in-time copy of your {{site.data.keyword.block_storage_is_short}} boot or data volume that you manually create. The first snapshot is a full backup of the volume. Subsequent snapshots of the same volume record only the changes since the last snapshot. For more information, see [About {{site.data.keyword.block_storage_is_short}} Snapshots for VPC](/docs/vpc?topic=vpc-snapshots-vpc-about).

### What is a backup snapshot?
{: faq}
{: #faq-block-storage-backup-snapshot}

Backup snapshots, simply called "backups", are snapshots that are automatically created by the Backup for VPC service. For more information, see [About Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

### What is restoring a volume from a snapshot?
{: faq}
{: #faq-block-storage-restore-vol}

Restoring from a snapshot creates a new, fully-provisioned boot or data volume. You can restore boot and data volumes during instance creation, when modifying an existing instance, or when provisioning a new stand-alone volume. For data volumes, you can also use the `volumes` API to create a data volume from a snapshot. For more information, see [Restoring a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore).

### Can I add tags to a volume?
{: faq}
{: #faq-tags}

Yes, you can add user tags and access management tags to your volumes. User tags are used by the backup service to automatically create backup snapshots of the volume. Access management tags help organize access to your {{site.data.keyword.block_storage_is_short}} volumes. For more information, see [Tags for {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-block-storage-about&interface=ui#storage-about-tags).

## Performance questions
{: #block-storage-vpc-performance-questions}

### What is IOPS and how do it relate to my {{site.data.keyword.block_storage_is_short}} volume performance?
{: faq}
{: #faq-block-storage-4a}

Input/output operations per second (IOPS) is used to measure the performance of your {{site.data.keyword.block_storage_is_short}} volumes. A number of variables impact IOPS values, such as the balance of read/write operations, queue depth, and data block sizes. In general, the higher the IOPS of your {{site.data.keyword.block_storage_is_short}} volumes, the better the performance. For more information on expected IOPS for {{site.data.keyword.block_storage_is_short}} profiles, see [Profiles](/docs/vpc?topic=vpc-block-storage-profiles). For more information about how block size affects performance, see [Block storage capacity and performance](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).

### Is the allocated IOPS enforced by instance or by volume?
{: faq}
{: #faq-block-storage-4}

IOPS is enforced at the volume level.

### What are IOPS profiles and how do they affect volume performance?
{: faq}
{: #faq-block-storage-5}
{: support}

IOPS profiles define IOPS/GB performance for volumes of various capacities. There are three predefined [IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers) that you can select that offer reliable IOPS performance for your workload requirements. You can also define [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles#custom) and specify a range of IOPS for a volume size that you choose. Custom IOPS is a good option when you have well-defined performance requirements that do not fall within a predefined IOPS tier. If you choose a custom IOPS profile, also define a minimum and maximum range for the volume size.

Maximum IOPS for data volumes varies based on volume size and the type of profile you select.

IOPS is measured based on a load profile of 16-KB blocks with random 50% read and 50% writes. Workloads that differ from this profile might experience reduced performance. If you use a smaller block size, maximum IOPS can be obtained but throughput is less. For more information, see
[How block size affects performance](/docs/vpc?topic=vpc-capacity-performance#how-block-size-affects-performance).

### What typical network performance might I expect between my compute instances and the {{site.data.keyword.block_storage_is_short}} service?
{: faq}
{: #faq-block-storage-17}

{{site.data.keyword.block_storage_is_short}} is connected to compute instances on a shared network, so the exact performance latency depends on the network traffic within a specific timeframe. Target latency for a 16-KB block size volume is under 1 millisecond for random reads and under 2 milliseconds for writes.

### What mechanisms are used to avoid data storage contention?
{: faq}
{: #faq-block-storage-17a}

Data storage contention is a common issue when multiple instances compete for access to the same {{site.data.keyword.block_storage_is_short}} volume. {{site.data.keyword.block_storage_is_short}} uses rate limiting at the hypervisor for optimal bandwidth between the hypervisor and {{site.data.keyword.block_storage_is_short}} service. As a result, latency is guaranteed to be less than 1 millisecond for random reads and under 2 milliseconds for writes for a typical 16-KB block size. Latency outside these metrics might indicate a problem on the client side.

### What happens when a volume is in a degraded health state?
{: faq}
{: #faq-block-storage-healthstate}

Volume health state defines whether a volume is performing as expected, given its status. Volume health can be OK, degraded, inapplicable, or faulted, depending on what is happening. For example, a degraded status is displayed when a volume is being restored from a snapshot and the volume is not yet fully restored. For more information about volume health states, see [{{site.data.keyword.block_storage_is_short}} volume health states](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#block-storage-vpc-health-states).

## Data security and encryption questions
{: #block-storage-vpc-security-questions}

### How secure is data in a {{site.data.keyword.block_storage_is_short}} volume?
{: faq}
{: #faq-block-storage-20}
{: support}

All {{site.data.keyword.block_storage_is_short}} volumes are encrypted at rest with IBM-managed encryption. IBM-managed keys are generated and securely stored in a {{site.data.keyword.block_storage_is_short}} vault that is backed by Consul and maintained by {{site.data.keyword.cloud}} operations.

For more security, you can protect your data by using your own customer root keys (CRKs). You import your root keys to, or create them in, a supported key management service (KMS). Your root keys are safely managed by the supported KMS, either {{site.data.keyword.keymanagementserviceshort}} (FIPS 140-2 Level 3 compliance) or {{site.data.keyword.hscrypto}}, which offer the highest level of security (FIPS 140-2 Level 4 compliance). Your key material is protected in transit and at rest.

For more information, see [Supported key management services for customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok). To learn how to set up customer-managed encryption, see
[Creating {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

You control access to your root keys stored in KMS instances within {{site.data.keyword.cloud}} by using IBM {{site.data.keyword.iamshort}} (IAM). You grant access to the IBM {{site.data.keyword.block_storage_is_short}} Service to use your keys. With the API, you can link a primary account containing a root key to a secondary account, then use that key to encrypt new volumes in the secondary account. For more information, see [Cross-account encryption for multitenant storage resources](/docs/vpc?topic=vpc-getting-started).

You can also revoke access at any time, for example, if you suspect your keys might be compromised. You can also disable or delete a root key, or temporarily revoke access to the key's associated data on the cloud. For more information, see [Managing root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-manage-root-keys).

### What are the advantages of using customer-managed encryption over provider-managed encryption?
{: faq}
{: #faq-block-storage-21}

Customer-managed encryption encrypts your {{site.data.keyword.block_storage_is_short}} volumes by using your own root keys. You have complete control over your data security and grant IBM access to use your root keys. For more information, see [Advantages of customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#byok-advantages).

### What encryption technology is used for customer-managed encryption?
{: faq}
{: #faq-block-storage-22}

Virtual disk images for VPC use QEMU Copy On Write Version 2 (QCOW2) file format. LUKS encryption format secures the QCOW2 format files. IBM currently uses the AES-256 cipher suite and XTS cipher mode options with LUKS. This combination provides you a much greater level of security than AES-CBC, along with better management of passphrases for key rotation, and provides key replacement options in case your keys are compromised.

### What are master encryption keys and how are they assigned to my {{site.data.keyword.block_storage_is_short}} volumes?
{: faq}
{: #faq-block-storage-23}

Each volume is assigned a unique master encryption key, called a data encryption key or DEK, that is generated by the instance's host hypervisor. The master key for each {{site.data.keyword.block_storage_is_short}} volume is encrypted with a unique KMS-generated LUKS passphrase, which is then encrypted by your customer root key (CRK) and stored in the KMS. Passphrases are AES-256 cipher keys, which means that they are 32 bytes long and not limited to printable characters. You can view the cloud resource name (CRN) for the CRK that is used to encrypt a volume. However, the CRK, LUKS passphrase, and the volume's master encryption key are never exposed. For more information about all the keys IBM VPC uses to secure your data, see [IBM's encryption technology - How your data is secured](/docs/vpc?topic=vpc-vpc-encryption-about#byok-technologies).

### I use customer-managed encryption for my volumes. What happens when I disable or delete my root key?
{: faq}
{: #faq-block-storage-24}

These actions are two separate actions. Disabling a root key in your KMS suspends its encryption and decryption operations, placing the key in a suspended state. Workloads remain running in virtual server instances and boot volumes remain encrypted. Data volumes remain attached. However, if you power down the VM and then power it back on, any instances with encrypted boot volumes do not start. You can enable a root key in a suspended state and resume normal operations. For more information, see [Disabling root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-disable-root-keys).

Deleting a root key has greater consequences. Deleting a root key purges usage of the key for all resources in the VPC. By default, the KMS prevents you from deleting a root key that's actively protecting a resource. However, you can still force the deletion of a root key. You have limited time to [restore a deleted root key](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-restore-root-key) that you imported to the KMS. For more information, see [Deleting root keys](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-delete-root-keys).

### Can I remove IAM authorization from Cloud {{site.data.keyword.block_storage_is_short}} to the KMS and still delete my {{site.data.keyword.block_storage_is_short}} volumes with customer-managed encryption?
{: faq}
{: #faq_block_storage_remove_iam}

If you remove IAM authorization before you delete your BYOK volume (or image), the delete operation completes but the root keys that are protecting these resources are not deregistered in the KMS instance. This means the root key remains registered for a resource that doesn't exist. Always delete a BYOK resource before you remove IAM authorization. For more information about safely removing service authorization, see [Removing service authorization to a root key](/docs/vpc?topic=vpc-vpc-encryption-managing#instance-byok-inaccessible-data).

### What should I do if my root key is compromised?
{: faq}
{: #faq-block-storage-25}

Independently back up your data. Then, delete the compromised root key and power down the instance with volumes that are encrypted with that key.

Also, consider setting up a key rotation policy that automatically rotates your keys based on a schedule. For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

### What is key rotation?
{: faq}
{: #faq-block-storage-25a}

For {{site.data.keyword.vpc_short}} resources such as {{site.data.keyword.block_storage_is_short}} volumes that are protected by your customer root key (CRK), you can rotate the root keys for additional security. When you rotate a root key by a schedule or on demand, the original key material is replaced. The old key remains active to decrypt existing volumes but can't be used to encrypt new volumes. For more information, see [Key rotation for VPC resources](/docs/vpc?topic=vpc-vpc-key-rotation).

### How does key rotation work?
{: faq}
{: #faq-block-storage-25b}

Customer-managed encrypted resources such as {{site.data.keyword.block_storage_is_short}} volumes use your root key (CRK) as the root-of-trust key that encrypts a LUKS passphrase that encrypts a master key that's protecting the volume. You can import your CRK to a key management service (KMS) instance or instruct the KMS to generate one for you. Root keys are rotated in your KMS instance.

When you rotate a root key, a new version of the key is created by generating or importing new cryptographic key material. The old root key is retired, which means its key material remains available for decrypting existing volumes, but not available for encrypting new ones. New resources are protected by the latest key. For more information, see [How key rotation works](/docs/vpc?topic=vpc-vpc-key-rotation#vpc-key-rotation-function).

### Am I charged for using customer-managed encryption?
{: faq}
{: #faq-block-storage-27}

You are not charged extra for creating volumes with customer-managed encryption. However, there is a charge for setting up a {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instance to import, create, and manage your root keys. Contact your IBM customer service representative for details.

### What's the difference between using Key Protect as my KMS compared to HPCS?  When would I use one over the other?
{: faq}
{: #faq-block-storage-28}

Both key management systems provide you complete control over your data, managed by your root keys. {{site.data.keyword.keymanagementserviceshort}} is a multi-tenant KMS where you can import or create your root keys and securely manage them. {{site.data.keyword.hscrypto}} is a single-tenant KMS and hardware security module (HSM) that is controlled by you, which offers the highest level of security. For more information and links to documentation about these key management services, see [Supported key management services for customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#kms-for-byok).

### Can I convert my volume from provider-managed encryption to customer-managed encryption?
{: faq}
{: #faq-block-storage-29}

No, after you provision a volume and specify the encryption type, you can't change it.
