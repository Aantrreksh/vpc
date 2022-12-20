---

copyright:
  years: 2022
lastupdated: "2022-12-20"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# FAQs for Backup for VPC
{: #backup-service-vpc-faq}

The following questions pertain to the VPC Backup service. If you have other questions you'd like to see answered here, provide feedback by using the **Open doc issue** or **Edit topic** links after the FAQs.
{: shortdesc}

## What is the VPC backup service?
{: faq}
{: #faq-baas-concepts}

With the VPC backup service, you can create backup policies for your {{site.data.keyword.block_storage_is_short}} volumes. A backup policy contains a backup plan, where you set a scheduled backup of your volumes. You can create up to four backup plans per policy. When a backup is triggered, it creates a snapshot of the volume contents. You can also set a retention period for your backups so that the oldest ones are deleted either by date or total count. For more information, see [Backup service concepts](/docs/vpc?topic=vpc-backup-service-about#backup-service-concepts).

## How do I set up the backup service?
{: faq}
{: #faq-baas-setup}

Before you can create backup policies, you need to grant [service-to-service authorizations](/docs/vpc?topic=vpc-backup-s2s-auth&interface=api), and specify user roles to enable the backup service. Then, you add user tags for new or existing data volumes that you associate with a backup policy. Finally, you create backup policies and plans to schedule automatic backups. For more information, see [Creating a backup policy](/docs/vpc?topic=vpc-backup-policy-create&interface=ui).

## How does the backup service work?
{: faq}
{: #faq-baas-function}

You add [user tags](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-about-tags) to your volumes and specify the same tags in a backup policy. When the tags match, a backup is triggered based on the backup plan schedule. You can [view backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs) to see the progress of the operation. The Snapshot for VPC service is used to create the backup. The entire contents of the volume are copied and retained for the number of days or total number of backups that are specified in the backup plan. When the retention period is reached, the older backups are deleted. 

## What resources are backed up?
{: faq}
{: #faq-baas-resources}

{{site.data.keyword.block_storage_is_short}} data and boot volumes with user tags that match the tags in a [backup policy](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-policies) are backed up.

## How do enable my volumes to be backed up?
{: faq}
{: #faq-baas-enable}

Enabling your backups is two-part process. First, you [specify user tags](/docs/vpc?topic=vpc-backup-use-policies) on the {{site.data.keyword.block_storage_is_short}} volumes that you want to back up. You then [create a backup policy](/docs/vpc?topic=vpc-backup-policy-create) and specify these tags, which identify the volumes that you're backing up. Within a policy, you create a [backup plan](/docs/vpc?topic=vpc-backup-policy-create&interface=ui#backup-plan-ui) to schedule backups of these resources. You can schedule backups to be taken every daily, weekly, or monthly.

## How many backups can I create?
{: faq}
{: #faq-baas-total}

You can create up to 750 backups per volume per account. Consider how your [billing changes](/docs/vpc?topic=vpc-snapshots-vpc-about&interface=api#snapshots_vpc_considerations) when you increase the number of snapshots that you take and retain.

## What are backup policy jobs?
{: faq}
{: #faq-backup-jobs}

Backup policy jobs, or backup jobs for short, are triggered when a scheduled backup snapshot is being created or deleted. If the create or delete action is successful, the backup job contains information about the backup snapshot that was created or deleted. If the job ran unsuccessfully, the job contains the reason for the failure. For more information, see [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

## Can I back up my volume snapshots?
{: faq}
{: #faq-baas-snapshots}

Tags for snapshots are inherited from the source volume. When you restore a volume from a snapshot, and the tags that are applied to the new volume match the tags in a backup policy, the new volume is backed up. But you can't directly back up a snapshot that has tags in a backup policy.

## How long are backups retained?
{: faq}
{: #faq-baas-retention}

You can specify that backups be kept 1 - 30 days (default). The retention period can't be shorter than the backup frequency or it returns an error.

You can also specify the number of backups to retain, up to 750 per volume, after which the oldest backups are deleted.

## Are there limitations on how many backups I can take?
{: faq}
{: #faq-baas-limitations}

Yes. You can create 10 backup policies per account and up to 750 backups of a volume. For other limitations of this release, see [Limitations in this release](/docs/vpc?topic=vpc-backup-service-about&interface=ui#backup-service-limitations).

## How do I create a new volume from a backup?
{: faq}
{: #faq-baas-restore}

Restoring from a backup snapshot creates a fully provisioned boot or data volume. You can restore boot and data volumes during instance creation, when you modify an existing instance, or when you provision a stand-alone volume. For more information, see [Restoring a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore).

## Am I charged for usage?
{: faq}
{: #faq-baas-pricing}

Yes. Cost for backups is calculated based on GB capacity that is stored per month, unless the duration is less than one month. The backup exists on the account until it reaches its retention period, or when you delete it manually, or when you reach the end of a billing cycle, whichever comes first.

Pricing of subsequent backups can also increase or decrease when you [increase source volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes) or [adjust IOPS](/docs/vpc?topic=vpc-adjusting-volume-iops) by specifying a different IOPS profile for the source volume. For example, expanding volume capacity increases costs while changing an IOPS profile from a 5 IOPS/GB tier to a 3 IOPS/GB tier decreases the monthly and hourly rate. Billing for an updated volume is automatically updated to add the prorated difference of the new price to the current billing cycle. The new full amount is then billed in the next billing cycle.

Pricing for backups is also set by region of the source volume. For more information, see [Pricing](https://www.ibm.com/cloud/vpc/pricing).
