---

copyright:
  years: 2019, 2022
lastupdated: "2022-11-22"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}
<!--- comment: linked help topic -->

# About {{site.data.keyword.block_storage_is_short}}
{: #block-storage-about}

{{site.data.keyword.block_storage_is_full}} provides hypervisor-mounted, high-performance data storage for your virtual server instances that you can provision within an {{site.data.keyword.vpc_full}} (VPC). The [VPC](/docs/vpc?topic=vpc-about-vpc) infrastructure provides rapid scaling across zones and extra performance and security.
{: shortdesc}

## Overview
{: #block-storage-overview}

{{site.data.keyword.block_storage_is_short}} supports all [virtual server profiles](/docs/vpc?topic=vpc-block-storage-profiles#vsi-profiles-relate-to-storage).

{{site.data.keyword.block_storage_is_short}} volume data is stored redundantly across multiple physical disks in an Availability Zone to prevent data loss due to failure of any single component.

{{site.data.keyword.block_storage_is_short}} provides primary boot volumes and secondary data volumes. Boot volumes are automatically created and attached during instance provisioning. Data volumes can be created and attached during instance provisioning as well, or as stand-alone volumes that you can later attach to an instance. To protect your data, you can use your own encryption keys or choose IBM-managed encryption.

You can choose an [IOPS tier profile](/docs/vpc?topic=vpc-block-storage-profiles#tiers) to specify a pre-defined level of performance for your volumes. Or, you can choose a [custom IOPS profile](/docs/vpc?topic=vpc-block-storage-profiles#custom) and define your own volume capacity and IOPS level. All profiles are backed by solid-state drives (SSDs).

For data volumes attached to a virtual server instance, you can [increase volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) in GB increments up to 16,000 GB capacity, depending on your volume profile. You can also [increase or decrease IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) for a volume that is attached to an instance by specifying a different IOPS tier or custom IOPS band.

You can also apply user tags and access management tags to your boot and data volumes. Add user tags when you create a volume or update an existing volume with the UI, CLI, or API. Create access management tags with the IAM UI or Global Search and Tagging API, then add them to your volumes. For more information, see [Tags for block storage volumes](#vpc-storage-about-tags).

When you create, view, or update a block storage volume, or [restore a volume from a snapshot](/docs/vpc?topic=vpc-snapshots-vpc-restore), the volume health state is reported in the UI, CLI, or API. For more information, see [Block storage volume health states](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#block-storage-vpc-health-states).

## Block storage for VPC volumes
{: #block-storage-vpc-volumes}

{{site.data.keyword.block_storage_is_short}} offers block-level volumes that are attached to an instance as a boot volume when the instance is created or attached as secondary data volumes. You can configure up to 300 block storage volumes per account in a region. You can request to increase this quota by [opening support case](/docs/vpc?topic=vpc-manage-storage-limit) and specifying in which zone you need more volumes.

You can attach only one block storage volume to a virtual server instance at a time, but you can attach up to 12 block storage data volumes to a single instance. For other limitations, see [Volume attachment limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits).

### Boot volumes
{: #block-storage-vpc-boot-volumes}

When you create an instance from a stock image, a 100 GB, 3,000 IOPS general-purpose boot volume is created and attached to the instance by default. When you create an instance from a custom image, you can specify a boot volume capacity 10 GB to 250 GB, depending what the image requires. This capacity can be any size between the minimum size supported for the selected image and the maximum supported image size. If the custom image is smaller than 10 GB, the boot volume capacity is rounded up to 10 GB. After the boot volume is created, you can expand the boot volume size to the maximum supported size, which is 250 GB.

You cannot create an image from an encrypted boot volume (Image from a volume feature) that is not 100GB. The operation will be blocked.
{: note}

By default, boot volumes are encrypted by IBM-managed encryption. Optionally, you can use your own root keys (CRKs) by choosing customer-managed encryption during instance creation (see [Customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption)).

By default, boot volumes are always deleted when you delete the instance. 

### Data volumes
{: #secondary-data-volumes}

Block storage data volumes are secondary volumes with total capacity range of 10 GB to 16,000 GB. Maximum IOPS for data volumes varies based on volume size and the IOPS tier profile that you selected. For more information, see
[IOPS tiers](/docs/vpc?topic=vpc-block-storage-profiles#tiers).

You can create data volumes as stand-alone volumes or when you provision an instance. Stand-alone volumes exist in an unattached state until you attach the volume to an instance. When you create a data volume as part of instance provisioning, the volume is automatically attached to the instance.

When you create an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance, the data volume that is attached to the instance during instance creation is automatically encrypted with the seed or passphrase that you provide.
{: note}

Block storage data volumes can be attached to any available instance in your region, based on your customer account and permissions, and within [certain limits](/docs/vpc?topic=vpc-attaching-block-storage#vol-attach-limits). These volumes are detached by default when the instance is deleted. Detaching by default allows your data to persist beyond the virtual server instance lifecycle. It only removes the volume's association with the instance. You can delete data volumes manually after they are detached. Also, when you create data volumes, you can specify that they be [automatically deleted](/docs/vpc?topic=vpc-managing-block-storage#auto-delete) when the instance is deleted.

Detached volumes can be attached to an available, running instance without reprovisioning the volume or the instance.

When you create and attach a data volume to a virtual server instance, you can later increase the size of that volume. You indicate capacity in GB increments up to 16,000 GB capacity, depending on your volume profile. For more information, see [Expanding block storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

Data volumes are encrypted by default with IBM-managed encryption. You can also encrypt data volumes by using your own root keys.

## Block Storage encryption
{: #vpc-storage-encryption}

{{site.data.keyword.cloud_notm}} takes the need for security seriously and understands the importance of being able to encrypt data to keep it safe. When you create a stand-alone data volume or create a data volume as part of instance creation, you can choose to protect your data by using your own root keys, or use the default IBM-managed encryption. Boot volumes created when you provision an instance are encrypted with IBM-managed encryption. You can also edit the boot volume to use your root keys. After you set up encryption for a boot or data volume, you can't change it.

For more information about data encryption, see [About data encryption for VPC](/docs/vpc?topic=vpc-vpc-encryption-about).

## Tags for block storage volumes
{: #storage-about-tags}

{{site.data.keyword.block_storage_is_short}} is enabled for Global Searching and Tagging (GhoST). You can create and apply [user tags](#storage-about-user-tags) and [access management tags](#storage-about-mgt-tags) to volumes to better control and organize your block storage resources across the VPC.

### User tags for block storage volumes
{: #storage-about-user-tags}

User tags are uniquely identified by a Cloud Resource Name (CRN) identifier. When you create a user tag, you provide a unique name within your billing account. You can define user tags in label or key-value format.

You can [add user tags](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#add-user-tags-volumes-ui) when you create a block storage volume or when you update an existing volume. You can also add tags to a boot or data volume during [instance creation](/docs/vpc?topic=vpc-creating-block-storage&interface=ui#create-from-vsi). For boot volumes, you edit the boot volume to add tags. For data volumes, you can add tags when you create and attach them to the instance, or add them to existing data volumes. Volume user tags are also used by [backup policies](/docs/vpc?topic=vpc-backup-service-about) to create automatic backup snapshots of the volume. Create, view, and manage user tags from the UI, CLI, or API, and remove then at any time.


### Access management tags for block storage volumes
{: #storage-about-mgt-tags}

[Access management tags](/docs/vpc?topic=vpc-iam-getting-started&interface=ui) help organize access control by creating flexible resource groupings, enabling your block storage resources to grow without requiring updates to IAM policies.

You can create access management tags and then apply them to new or existing volumes. Use the IAM UI or the Global Search and Tagging API to create the access management tag. Then, from the VPC UI or API, add access management when you create a new volume or modify an existing volume. You can't add these tags to volumes that are created during instance provisioning. After you add the access management tags, you can manage access to them by using the IAM policies. For more information, see [Apply access management tags to a block storage volume](/docs/vpc?topic=vpc-managing-block-storage&interface=ui#storage-add-access-mgt-tags).

For more information about managing tags for your account, see [Working with tags](/docs/account?topic=account-tag&interface=ui).

## Cancelling your block storage
{: #vpc-cancel-storage}

If you no longer need a volume, you can cancel it at any time. IBM [wipes all data](/docs/vpc?topic=vpc-managing-block-storage#block-storage-data-eradication) before the storage is reused. If you have extra compliance requirements such as NIST 800-88 Guidelines for Media Sanitization, you must perform data sanitation procedures before you delete your volumes. For more information, see [Sanitizing your data before deleting a volume](/docs/vpc?topic=vpc-managing-block-storage#block-storage-sanitization).

## Next Steps
{: #block-storage-about-next-steps}

Start creating {{site.data.keyword.block_storage_is_short}} volumes.

* For more information about creating a volume during instance provisioning, see [Create and attach a block storage volume when you create an instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi).
* For more information about creating a block storage encrypted by your own encryption keys, see [Creating block storage volumes with customer-managed encryption](/docs/vpc?topic=vpc-block-storage-vpc-encryption).

## Related information
{: #related-info-storage}

For more information about creating and managing instances in the VPC, see [About virtual server instances for VPC](/docs/vpc?topic=vpc-about-advanced-virtual-servers).

{{site.data.keyword.block_storage_is_short}} provides features unique to the VPC and is not compatible with the classic infrastructure storage. If you're interested in {{site.data.keyword.blockstoragefull}} on the classic infrastructure, see [{{site.data.keyword.blockstoragefull}}](/docs/BlockStorage?topic=BlockStorage-getting-started).
{: note}
