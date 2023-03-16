---

copyright:
  years: 2021, 2023
lastupdated: "2023-03-16"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Restoring a volume from a snapshot
{: #snapshots-vpc-restore}

Restoring data from a snapshot creates a new, fully provisioned volume that you can use to start an instance or attach as auxiliary storage. You can restore boot and data volumes during instance creation or when you modify an existing instance. You can also restore a data volume from a snapshot of a volume that is not attached to an instance. Volumes can be restored from snapshots that were created manually or by a backup policy. You can restore volumes from fast restore clones, too. You can create volumes from snapshots in the UI, from the CLI, or with the API.
{: shortdesc}

## About restoring a volume from a snapshot
{: #snapshots-vpc-restore-concepts}

Restoring a volume from a snapshot creates a boot or data volume, depending on whether the snapshot is bootable or nonbootable. 

* Restoring from a bootable snapshot creates a boot volume that you can use to start a virtual server instance. The boot volume uses a general-purpose profile and is limited to 250 GB.

* A new data volume that was created from nonbootable snapshot inherits its properties from the original volume, such as [profile](/docs/vpc?topic=vpc-block-storage-profiles), capacity, data, and metadata. If the source volume used [customer-managed encryption](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), the volume inherits that encryption with the original customer root key (CRK). However, you can specify a larger volume size, different IOPS profile, and different CRK if you prefer.

You can restore volumes from a manually created snapshot or from a snapshot that was created by a backup policy. This type of snapshot is called a backup. For more information, see [Restoring a volume from a backup snapshot](/docs/vpc?topic=vpc-baas-vpc-restore). 

You can also choose to restore a volume by using a fast restore snapshot clone. For more information about fast restore, see the [FAQs](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-fr).

You can restore volumes at various stages of the VPC lifecycle.

* When you provision a virtual server instance, you can specify a snapshot of a boot or a snapshot of data volume. The restored boot volume is used to start the new instance. Restored data volumes are automatically attached to the instance as auxiliary storage.
* When you want to add a new auxiliary storage to your existing instance, you can restore a data volume from a nonbootable snapshot.
* When you create an unattached (stand-alone) {{site.data.keyword.block_storage_is_short}} volume from a snapshot, you can still attach the volume to an instance later.

## Limitations of restoring a volume from a snapshot
{: #snapshots-vpc-restore-limitations}

The following limitations apply when you restore a volume from a snapshot.

* To restore a volume, the snapshot must be in a _stable_ state.
* You can delete the new volume at any time. However, you can't delete the snapshot from which the volume is restored from unless the hydration is complete or the volume is deleted.
* If snapshot has customer-managed encryption and you don't specify a different root key CRN, the restored volume is encrypted with the snapshot's encryption key and it cannot be changed later.
* When the new volume is created, data restoration begins immediately, but performance is degraded until the volume is fully hydrated.

### Performance impact
{: #snapshots-performance-considerations}

The performance of boot and data volumes is initially degraded when data is restored from a snapshot. Performance degradation occurs during the restoration because your data is copied from {{site.data.keyword.cos_full}} to {{site.data.keyword.block_storage_is_short}} in the background.

At first, the restored volume appears as _pending_ or _degraded_, and the service begins pulling data from the snapshot that is stored in {{site.data.keyword.cos_short}}. While the data is being restored, the volume's health state is monitored by the volume resource. If the volume that was created from snapshot is attached to an instance before its data is fully restored, volume hydration pauses on the service node and continues on the compute node. If the volume is detached while the hydration is in progress, the hydration pauses on compute node and continues on the storage node. You cannot delete a snapshot that is being used to hydrate a volume. After the restoration process is complete, you can realize full IOPS on the new volume.

Volumes that are restored from fast restore clones do not require hydration. The data is available as soon as the volume is created. However, to achieve the best performance and efficiency when you provision multiple instances, boot from an existing image. Custom images that you provide are better than stock images for this purpose. Images can have a maximum size of 250 GB.

## Restoring a volume with the fast restore feature
{: #snapshots-vpc-use-fast-restore}

Restoring a volume by using a [fast restore](/docs/vpc?topic=vpc-snapshots-vpc-faqs&interface=ui#faq-snapshot-fr) snapshot clone creates a fully provisioned volume at creation time. 

With this feature, you create and keep a clone of the snapshot in a zone within your region instead of an {{site.data.keyword.cos_short}} bucket. When you restore data from a fast restore snapshot, the data is pulled from the clone within your region. Because the data is immediately available, no hydration is necessary. Performance levels are not affected. By using fast restore snapshots, you can achieve [recovery time objectives](#x3167918){: term} (RTO) quicker than by restoring from a regular snapshot.

You can also use the fast restore feature with the backup service. For more information, see [creating backup policies and plans](/docs/vpc?topic=vpc-create-backup-policy-and-plan).

When you use the fast restore feature, your existing regional plan is adjusted, including billing. Billing for fast restore snapshot copies is based on instance hours, regardless of snapshot size. For more information, see [Pricing](https://www.ibm.com/cloud/vpc/pricing){: external}.

## Restore a volume from a snapshot in the UI
{: #snapshots-vpc-restore-ui}
{: ui}

You can create volumes from various pages in the {{site.data.keyword.cloud_notm}} console. During the "create and attach" phase of virtual server instance provisioning, you can select a snapshot to restore the volume. Restoring from a bootable snapshot creates a boot volume that you use to provision the virtual server instance. The boot volume uses a general-purpose profile and is limited to 250 GB. Data volumes are created and attached to the instance. You can restore volumes from a snapshot outside of instance provisioning as well, you can create stand-alone volumes and new auxiliary volumes for existing instances.

### Create a volume from the list of snapshots in the UI
{: #snapshots-vpc-restore-snaphot-list-ui}

From the list of {{site.data.keyword.block_storage_is_short}} snapshots, you can create an {{site.data.keyword.block_storage_is_short}} volume and specify whether it's to be attached to a virtual server instance or unattached (stand-alone). If you choose to attach a data volume, you can select an existing virtual server instance or choose to create an instance. The new volumes are added to the [list of {{site.data.keyword.block_storage_is_short}} volumes](/docs/vpc?topic=vpc-viewing-block-storage&interface=ui#viewvols-ui).

1. Go to the list of {{site.data.keyword.block_storage_is_short}} snapshots. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, click the **menu ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage snapshots**.

2. Select a snapshot from the list. It must be in a `stable` state. 

3. From the overflow menu, select **Create volume**.

4. In the side panel, choose whether you want to create an unattached data volume, create and attach a volume to an existing instance, or create a volume and use it to provision an instance.

    * For a **stand-alone data volume**, leave **Attach volume to virtual server** clear.
       Use this option when you're not sure which virtual server instance you want to attach the volume to.
       {: tip}

    * To attach the volume to an existing instance, select **Attach volume to virtual server**. Click **Attach new volume to an existing virtual server**. Then, select the virtual server instance that you want to attach the volume to. You can filter the list of available servers by zone.
    * To restore a volume and use it to provision a virtual server instance, select **Attach volume to virtual server**, and click **Attach new volume to a new virtual server**. Then, click **Configure virtual server**. This action takes you to the [virtual server provisioning page](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui#creating-virtual-servers-ui).
       The new volume is shown in the **Boot volume** or **Data volume** section of the instance provisioning page. The placement depends on whether the volume that you created was from a bootable snapshot or a nonbootable snapshot.
       {: note}

5. Provide the required volume details.

    | Field | Description |
    |-------|-------------|
    | **Volume details** | Define the new volume. |
    | Name | Enter a name for the new volume. |
    | Resource group | Use the defaults or select from the list. |
    | Zone | Inherited from the snapshot. Change it to another zone in your region if you want to. |
    | Size | Enter a volume size allowed by the profile. The default is the minimum provisioning size based on the snapshot. |
    | **Profile** | Defaults to the snapshot's IOPS tier or custom profile. You can change the profile. |
    | IOPS | For IOPS tiers, specify IOPS tier profile. For custom IOPS, select a range. |
    | Size | Enter a volume size that is allowed by the profile. |
    | **Encryption** | Inherited from the snapshot.|
    {: caption="Table 1. Create volume options." caption-side="bottom"}
   
6. When finished, click **Save**. The new volume is created.

### Create a volume from the snapshot details page in the UI
{: #snapshots-vpc-restore-vol-details-ui}

1. Go to the list of {{site.data.keyword.block_storage_is_short}} volumes and select a volume. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, click the **menu ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Storage > Block storage volumes**.
2. On the {{site.data.keyword.block_storage_is_short}} volume details page, select the **Snapshots and Backups** tab. A list of snapshots that were created manually or by backup policies is shown.
3. From the list, click the snapshot name to go to its details page.
4. From the **Actions** menu, click **Create volume**.
5. Define the new volume in the side panel. The information that you need to provide is the same as when you create a volume from the [list of snapshots](#snapshots-vpc-restore-snaphot-list-ui). See Table 1 for the details.
6. When you're finished, click **Save**. The new volume is created.

### Create a volume from a snapshot when you provision a virtual server instance in the UI
{: #snapshots-vpc-restore-vol-ui}

Follow these steps to create a boot and a data volume from snapshots when you provision a new virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, click the **menu ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. Click **Create** and provision your new instance. For more information about the required fields, see the table in [Creating virtual server instances in the UI](/docs/vpc?topic=vpc-creating-virtual-servers).
3. For the operating system, click the tab for **Snapshots**. The most recent bootable snapshot is listed.
   * If you want to use a different snapshot, click **Edit**. From the list of snapshots, select a bootable snapshot for your instance's operating system. Click **Save**. This action populates the snapshot data in the boot volume field on the provisioning page.
   * If you want to change the properties of the boot volume, such as the name, auto-delete feature, encryption, or tags, click the pencil icon. Change the property that you want and click **Save**. The boot volume information is updated on the provisioning page.
4. To create a data volume, under Data Volumes, click **Create**. A side panel is displayed.
   1. Select **Import from snapshot**. Expand the list to select a data snapshot. The most recent data snapshot is selected by default. However, you can select any snapshot from the list. 
   1. The snapshot contains the properties of the original source volume, including the auto-delete status, tags, size, profile, and encryption. You can change all these properties. Keep in mind that you can change the volume name, profile, size, and IOPS later, but you cannot change encryption type or CRK after the volume is created.
   1. Select a unique name, specify the size, and click **Create**. The data volume information is added in the Data volume list.
5. Review your selections for volumes, instance profile, SSH keys, networking, and so on. 
6. If you are satisfied with your choices, click **Create virtual server instance**. The new instance is created with the volumes that you specified. The new instance appears in the list of virtual server instances.
7. To see the instance details, click the instance name. The volume or volumes that you restored from snapshots are listed under **Storage volumes**. The camera icon indicates that the volume was created from a snapshot.

### Create a data volume from a snapshot for an existing virtual server instance with the UI
{: #snapshots-vpc-create-from-vol-ui}

You can also create a data volume from a snapshot for an existing instance. Choose from the list of virtual server instances.

1. In the [{{site.data.keyword.cloud_notm}} console)](/login){: external}, click the **menu ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.
2. From the list, click the name of an instance. The instance must be in a _running_ state.
3. On the Instance details page, scroll to the list of Storage volumes and click **Attach volumes**. A side panel opens for you to define the volume attachment.
4. From the Attach storage volume panel, expand the list of Block Volumes, and select **Create a data volume**.
5. Select **Import from snapshot**. Expand the Snapshot list and select a snapshot.
6. The snapshot contains the properties of the original source volume, including the auto-delete status, tags, profile, and encryption. You can change all these properties. Keep in mind that you can change the volume name, profile, size, and IOPS later, but you cannot change encryption type or CRK after the volume is created.
7. Select a unique name, specify the size, and click **Save**. The data volume information is added in the Data volume list.
8. Click **Save**. The side panel closes and messages indicate that the restored volume is being attached to the instance. The new volume appears in the list of Storage volumes. Hover over the camera icon to see the name of the snapshot from which it was created.

## Restore a volume from a snapshot from the CLI
{: #snapshots-vpc-restore-CLI}
{: cli}

During the "create and attach" phase of instance provisioning, you can select a snapshot to restore the volume. Restoring from a bootable snapshot creates a boot volume that you cab use to provision a virtual server instance. The boot volume uses a general-purpose profile and is limited to 250 GB. Data volumes are created and can be attached to the instance. You can restore volumes from a snapshot outside of instance provisioning as well, you can create stand-alone volumes and new auxiliary volumes for existing instances.

### Before you begin
{: #snapshots-vpc-restore-prereq-CLI}

Before you can use the CLI, you must install the IBM Cloud CLI and the VPC CLI plug-in. For more information, see the [CLI prerequisites](/docs/vpc?topic=vpc-set-up-environment#cli-prerequisites-setup).
{: requirement}

1. Log in to the IBM Cloud.
   ```sh
   ibmcloud login --sso -a cloud.ibm.com
   ```
   {: pre}

   This command returns a URL and prompts for a passcode. Go to that URL in your browser and log in. If successful, you get a one-time passcode. Copy this passcode and paste it as a response on the prompt. After successful authentication, you are prompted to choose your account. If you have access to multiple accounts, select the account that you want to log in as. Respond to any remaining prompts to finish logging in.

2. Select the current generation of VPC. 
   ```sh
   ibmcloud is target --gen 2
   ```
   {: pre}

### Create a boot volume from a snapshot for a new instance from the CLI
{: #snapshots-vpc-restore-boot-CLI}

Run the `ibmcloud is instance-create` command with the `source_snapshot` property in the boot volume JSON. Specify the ID or name of a bootable snapshot. The restored boot volume is used to initialize the instance.

See the following example.

```sh
bmcloud is instance-create my-instance-restore1 ea002578-ff10-41fe-9652-e63f7e0e3cba us-south-1 bx2-2x8 ba11a6f2-6c17-4fee-a4b5-5c016fe64376 --boot-volume
'{
   "name":"boot-from-snapshot1",
   "volume":{
      "name":"boot-from-snapshot1",
      "profile":{
         "name":"general-purpose"
      },
      "source_snapshot":{
         "id":"d857c69f-d795-46ac-85e4-f26ca3001033"
      }
   }
}'
```
{: codeblock}

A successful response looks like the following example.

```zsh
Creating instance my-instance-restore1 in resource group under account VP01 as user rtuser1@mycompany.com...

ID               7101_eded6dcd-4f3c-4e79-a0cb-00f7c72f38cd
Name             my-instance-restore1
CRN              crn:v1:staging:public:is:us-south-1/2d1bace7b46e4815a81e52c6ffeba5cf
                 ::instance:7101_eded6dcd-4f3c-4e79-a0cb-00f7c72f38cd
Status           pending
Profile          bx2-2x8
Architecture     amd64
vCPUs            2
Memory           8
Network(Gbps)    4
Image            ID                                         Name
                 6f153c4d-6a9a-496d-8063-5c39932f6ded       ibm-centos-7-6-minimal-amd64-2
VPC              ID                                         Name
                 ea002578-ff10-41fe-9652-e63f7e0e3cba       my-vpc
Zone             us-south-1
Resource group   ID                                         Name
                 cdc21b72d4f557b195de988b175e3d81           Default

Created          2022-06-14T17:03:30+08:00
Boot volume      ID                                   Name                  Attachment ID                            Attachment name
                 0651dacb-4589-4147-86b3-a77544598f93 boot-from-snapshot1                  7101-abf9dd2b-9d5d-41f1-849d-55a8ab580ddb                            boot-from-snapshot1

```
{: screen}

For more information about available command options, see [`ibmcloud is instance-create`](/docs/vpc?topic=vpc-vpc-reference#instance-create).

### Create a data volume from a snapshot for a new instance from the CLI
{: #snapshots-vpc-restore-data-CLI}

When you create an instance by using the `ibmcloud is instance-create` command, specify the `source_snapshot` parameter and snapshot name or ID in the volume attachment.

See the following example.

```zsh
ibmcloud is instance-create my-instance-restore1 ea002578-ff10-41fe-9652-e63f7e0e3cba us-south-1 bx2-2x8 ba11a6f2-6c17-4fee-a4b5-5c016fe64376 --volume-attach
'{
   "name":"datavol-from-snapshot",
   "volume":{
      "name":"datavol-from-snapshot",
      "profile":{
         "name":"general-purpose"
      },
      "source_snapshot":{
         "id":"6daaaa39-3d81-4d1d-81f8-e1f6a14f97f3"
      }
   }
}'
.
.
.
```
{: codeblock}

For more information about available command options, see [`ibmcloud is instance-create`](/docs/vpc?topic=vpc-vpc-reference#instance-create).

### Create a data volume from a snapshot for an existing instance from the CLI
{: #snapshots-vpc-restore-data-inst-cli}

For an existing instance, specify the `ibmcloud is instance-volume-attachment-add` command with the `source-snapshot` parameter, and the name or ID of the snapshot. To find the ID of the snapshot from the CLI, see [View snapshots from the CLI](/docs/vpc?topic=vpc-snapshots-vpc-view&interface=cli#snapshots-vpc-view-cli).

See the following example.

```zsh
ibmcloud is instance-volume-attachment-add data-vol-1 a67f49de-fccc-4e5c-824e-dcbd06d009af --profile general-purpose --source-snapshot 52de6e85-7068-4247-90fd-d2fa91fd9864
```
{: codeblock}

For more information about available command options, see [`ibmcloud is instance-volume-attachment-add`](/docs/vpc?topic=vpc-vpc-reference#instance-volume-attachment-add).

### Create a stand-alone volume from a snapshot from the CLI
{: #snapshots-restore-create-vol-cli}

You can create a stand-alone {{site.data.keyword.block_storage_is_short}} data volume from a snapshot from the CLI. The volume is not attached to a virtual server instance. When you run the `ibmcloud is volume` command, the response shows the attachment type as `unattached`. You can attach the volume to an instance later.

Use this option when you're not sure which virtual server instance you want to attach the volume to. Or, if you want to restore data from an unattached volume that was detached from an instance.

Run the `ibmcloud is volume-create` command and specify the `snapshot` parameter and name or ID of the snapshot. For more information, see [Create a stand-alone {{site.data.keyword.block_storage_is_short}} volume from a snapshot](/docs/vpc?topic=vpc-creating-block-storage&interface=cli#create-vol-from-snapshot-cli).

For more information about available command options, see [`ibmcloud is volume-create`](/docs/vpc?topic=vpc-vpc-reference#volume-create).

## Restore a volume from a snapshot with the API
{: #snapshots-vpc-restore-API}
{: api}

Use the VPC API to restore a volume from a snapshot. During the "create and attach" phase of instance provisioning, you can select a snapshot to restore the volume. Restoring from a bootable snapshot with the API is not supported.

### Create a boot volume when you provision an instance with the API
{: #snapshots-vpc-restore-boot-api}

To restore a boot volume from a bootable snapshot when you create an instance, make a `POST /instances` request and specify the `boot_volume_attachment` property and the bootable snapshot ID in the `source_snapshot` subproperty.

See the following example.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2023-03-07&generation=2" \
-H "Authorization: $iam_token" \
-H "Content-Type: application/json" \
-d '{
      "name": "my-server-name",
      "zone": {
         "name": "us-south-1"
      },
      "vpc": {
         "id": "4d27c489-8ad7-3c18-cbf4-2103d9f8da93"
      },
      "profile": {
         "name": "cx2-2x4"
      },
      "primary_network_interface": {
        "name": "region1example-net1",
        "subnet": {
           "id": ""
        }
      },
      "boot_volume_attachment": {
        "delete_volume_on_instance_delete": true,
        "volume": {
            "profile": {
                "name": "general-purpose"
            },
            "source_snapshot": {
                "id": "eb373975-4171-4d91-81d2-c49efb033753"
            }
        }
     },
    "resource_group": {
        "id": "2fab2c7f-c09d-4c64-baf7-1453b7461493"
    }
}'

```
{: codeblock}

### Create a data volume when you provision an instance with the API
{: #snapshots-vpc-restore-data-api}

To restore a data volume from a snapshot and attach it at boot time, make a `POST /instances` request and specify the data volume attachment and snapshot ID.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2022-06-14&generation=2" \
-H "Authorization: $iam_token" \
-H "Content-Type: application/json" \
-d '{
      "name": "my-server-name",
      "zone": {
         "name": "us-south-1"
      },
      "vpc": {
        "id": "4d27c489-8ad7-3c18-cbf4-2103d9f8da93"
      },
      "profile": {
         "name": "cx2-2x4"
      },
      "primary_network_interface": {
        "name": "region1example-net1",
        "subnet": {
           "id": ""
        }
      },
     "volume_attachments": [
        {
            "name": "restore-data-vol1",
            "delete_volume_on_instance_delete": true,
            "volume": {
                "profile": {
                    "name": "general-purpose"
                },
                "source_snapshot": {
                    "id": "bdcdc984-ba4e-4aef-84fb-e8448c3116b1"
                }
            }
        }
     ]
    "resource_group": {
        "id": "2fab2c7f-c09d-4c64-baf7-1453b7461493"
    }
}'
```
{: codeblock}

### Create a stand-alone data volume from a snapshot with the API
{: #snapshots-vpc-restore-unattached-api}

You can use the API to create a stand-alone volume from a snapshot. Use this option when you're not sure which virtual server instance you want to attach the volume to. Or, if you want to restore data from an unattached volume that was detached from an instance.

To restore a stand-alone data volume from a snapshot, make a `POST /volumes` request and specify the ID, CRN, or URL of the snapshot in the `source_snapshot` property. The restored volume capacity (in GBs) must be at least the snapshot's minimum_capacity.

The following example request creates a 100-GB volume that is based on a 5 IOPS/GB profile. It specifies a different root key than the original snapshot. The source snapshot is specified by ID.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/volumes/?version=2022-06-14&generation=2" \
-H "Authorization: $iam_token" \
-H "Content-Type: application/json" \
-d '{
     "name": "volume-from-snapshot-1",
     "capacity": 100,
     "profile": {
        "name": "5iops-tier"
     },
     "zone": {
        "name": "us-south-1"
     },
     "encryption_key":{
        "crn":"crn:[...]"
     },
     "source_snapshot:" {
        "id": "bdcdc984-ba4e-4aef-84fb-e8448c3116b1"
     }
   }`
```
{: codeblock}

## Next Steps
{: #snapshots_vpc_restore_next_steps}

[Create](/docs/vpc?topic=vpc-snapshots-vpc-create) more snapshots or [manage](/docs/vpc?topic=vpc-snapshots-vpc-manage) existing snapshots.
