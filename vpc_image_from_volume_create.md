---

copyright:
  years: 2021, 2022
lastupdated: "2022-10-19"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an image from a volume
{: #create-ifv}

Use the UI, CLI, or API to create an image from a volume that is attached to an available virtual server instance as the primary boot volume or as a secondary boot volume.
{: shortdesc}

## Scenarios for creating an image from a volume
{: #ifv-scenarios}

You can create an image from a volume in several ways.

* Select an instance and create an image from that instance's boot volume. The new image inherits the boot volume encryption (customer-managed or IBM-managed).

* Select an instance, create an image from that instance's boot volume, and specify different encryption. For example, if the instance's boot volume was encrypted with IBM-managed encryption, you can select customer-managed encryption for the new image.

* Create an image from a boot volume in the list of block storage volumes. The volume must be a boot volume that is attached to a virtual server instance.

Depending on the size of the image that you're creating, the job might take from 5 minutes to 1.5 hours. You can cancel a job that's taking too long to queue. For more information, see [Performance considerations](/docs/vpc?topic=vpc-image-from-volume-vpc-manage#ifv-performance).
{: note}


## Create an image from a volume in the UI
{: #create-image-from-volume-vpc-ui}
{: ui}

Use the UI to create an image from a volume that is attached to an available virtual server instance as the primary boot volume or as a secondary boot volume.

### Choose the source for your custom image
{: #import-custom-image-src-UI}

Use the UI to import your custom image by choosing to create and import an image from a volume.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.

2. Click **Create**. The Import custom image page is displayed.

3. Complete the required fields on the Import custom images page (see Table 1), and then continue with one of the **Source** options.

| Field | Value |
|-------|-------|
| Name  | A name is required for your custom image. |
| Resource group | Select a resource group for the instance. |
| Tags |  You can assign a label to this resource so that you can easily filter resources in your resource list. |
| Region | Select the location, or specific geographic area, where you want your custom image to be available for provisioning.|
| Source | Select the source for the custom image by choosing either [Virtual server instance boot volume](#import-custom-image-vsi) (default) or [Block storage boot volume](#import-custom-image-vol). |
{: caption="Table 1. Import custom image user interface fields" caption-side="bottom"}

### Create an image from a virtual server instance boot volume
{: #import-custom-image-vsi}

When you select **Virtual server instance boot volume** as the source of your custom image, a list of instances displays. To create an image from the instance's boot volume:

1. On the **Import custom image** page, select **Virtual server instance boot volume** (default).

   To create an image of the boot volume associated with an instance, the virtual server instance must be stopped.
   {: note}

1. Optionally, stop a running instance by clicking the overflow menu (ellipsis) and click **Stop**.

1. Select an instance from the list. You're selecting the boot volume of the instance.

1. Select the encryption type, either customer-managed encryption or IBM-managed encryption (see [Table 2](#encrypt-custom-images)).

1. On the side panel, click **Import custom image**.

### Create an image from the list of boot volumes
{: #import-custom-image-vol}

When you select **Block storage boot volume** as the source of your custom image, a list of block storage volumes displays.

To create an image from the volume:

1. On the **Import custom image** page, select **Block storage boot volume**.

   A list of block storage volumes shows attached **Boot** and **Data** volumes. Unattached volumes appear with a dash (-). To create an image from a volume:

   * The volume must be a boot volume
   * The volume must have an `available` status
   * The volume must be attached to a virtual server instance
   * The instance must be stopped

1. Optionally, stop a running instance by clicking the overflow menu (ellipsis) and click **Stop attached instance**. Click refresh and select the volume.

1. Select the volume from the list.

1. Select the encryption type, either customer-managed encryption or IBM-managed encryption (see [Table 2](#encrypt-custom-images)).

1. On the side panel, click **Import custom image**.


### Select the encryption type
{: #encrypt-custom-images}

To complete your image from volume, select either IBM-managed encryption or customer-managed encryption. Table 2 describes these options as they appear in the UI.

| Field | Value |
|-------|-------|
| Encryption | The default selection is **No encryption**. If you have not encrypted your image by using QEMU, use the default value, No encryption. If you are importing an image that you encrypted by using QEMU and your own passphrase, select the key management service where your customer root key (CRK) that protects your passphrase is stored. Select either **Key Protect** or **Hyper Protect Crypto Services**. |
| Encryption service instance | For an encrypted image, select the specific instance of the key management service where your CRK that wraps your encryption passphrase is stored. |
| Key name | Select the customer root key (CRK) that you used to wrap your encryption passphrase. |
| Key ID | Shows the key ID that is associated with the data encryption key that you selected. |
{: caption="Table 2. Encryption types" caption-side="bottom"}

For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs).

### View and use your image from volume
{: #ifv-image-creation-completed}

When the image from a volume is created, it appears in the list of custom images.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.

2. To see the volume from which it was created, click the image name. The image details panel links to the source volume.

To use this image when you create a new instance, select it as operating system type when you create an instanceL

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to the **menu ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

2. Click **Create**. For more information about how to provision a new virtual server instance, see [Creating virtual server instances in the UI](/docs/vpc?topic=vpc-creating-virtual-servers). For the **Operating system**, select the **Custom image** tile.

3. Click **Select custom image** and select the image from volume that you created.


## Create an image from a volume with the CLI
{: #image-from-volume-vpc-cli}
{: cli}

Use the CLI to create an image from a volume that is attached to an available virtual server instance as the primary boot volume or as a secondary boot volume.

### Before you begin
{: #before-creating-ifv-cli}

1. Make sure that you downloaded, installed, and initialized the following CLI plug-ins:
   * {{site.data.keyword.cloud_notm}} CLI
   * The infrastructure-service plug-in

   For more information, see the [CLI Reference](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

   After you install the vpc-infrastructure plug-in, set the target to generation 2 by running the command `ibmcloud is target --gen 2`.
   {: important}


2. Make sure that you [created an {{site.data.keyword.vpc_short}}](/docs/vpc?topic=vpc-getting-started).


### Create an image from a boot volume that is attached to an instance
{: #ifv-create-cli}

1. [Stop the running instance](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#instance-stop) before you create the image from the volume.

   ```sh
   ibmcloud is instance-stop INSTANCE_ID
   ```
   {: pre}

2. Run the [`image-create` command](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#image-create) to create an image of a boot volume. Specify the ID of the source volume.

   ```sh
   ibmcloud is image-create IMAGE_NAME [--source-volume VOLUME_ID]
   ```
   {: pre}

   Example:

   ```sh
   $ ibmcloud is image-create test-ifv-vol1 --source-volume ecc68c2f-96a1-4862-bc86-14f47e5d9ed8

   Creating image test-ifv-vol1 in resource group  under account DEMO as user user1-mycompany.com...
   ID                 5b2fd4ee-c636-44c4-9673-453fca36832e
   Name               test-ifv-vol1
   CRN                crn:v1:staging:public:is:us-south:a/2d1bace7b46e4815a81e52c6ffeba5cf::image:5b2fd4ee-c636-44c4-9673-453fca36832e
   Status             pending
   Status reason      Code                   Message                                                More info
                      image_request_queued   The image request is accepted and waiting for system   -
                                             resources to become available.
   Operating system   Name             Architecture   Vendor   Version                 Dedicated host only
                      centos-8-amd64   amd64          CentOS   8.x - Minimal Install   false
   Source volume      ID                                     Name
                      ecc68c2f-96a1-4862-bc86-14f47e5d9ed8   aa-1-bx-boot-1617035447000
   Created            2021-05-20T09:43:16+08:00
   Visibility         private
   File size(GB)      -
   Encryption         none
   Resource group     f22cf48f-8836-4527-9131-1d7c73ba85e9
   ```
   {: screen}

## Create an image from a volume with the API
{: #image-from-volume-vpc-api}
{: api}

Use the regional API to create an image from volume when you create an instance or from an existing instance.

### Before you begin
{: #ifv-prereqs-api}

Before you can use the API, you must take some preliminary steps. For more information, see [API prerequisites](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).

### Create an image from volume when you create an instance (API)
{: #ifv-vpc-api-create-instance}

Follow these steps to create an image from volume when you create a new instance. The new instance's boot volume is used for the image, which inherits the encryption from the boot volume. In this scenario, the boot volume uses the default IBM-managed encryption, which is inherited by the new image. You can also specify your own root encryption key in the `POST/images` call. For more information, see [Create an image from volume and use your root encryption key](#ifv-use-crk).

This procedure:

1. Creates an instance and uses its boot volume ID as the source volume.
2. Stops the running instance.
3. Creates an image with the ID of the boot volume. In this scenario, the boot volume is encrypted with the default IBM-managed encryption. If the boot volume was encrypted with a [customer root key](/docs/vpc?topic=vpc-vpc-encryption-about#vpc-customer-managed-encryption), that root key is used to encrypt the new image. The API response displays the [CRN of the root key](#ifv-use-crk).

#### Step 1 - Create an instance
{: #ifv-create-instance}

Specify a `POST/instances` request to create an instance and pass the required parameters. See the following example.

```curl
curl -X POST \
"$vpc_api_endpoint/v1/instances?version=2021-05-20 \
$iam_token" -d
 `{
   "name":"test-ifv-instance",
   "profile":{
      "name":"bx2-2x8"
   },
   "vpc":{
      "id":"6054c339-8a17-4c75-958c-4a23358a37ed"
   },
   "image":{
      "id":"524d52aa-b267-4801-a7d2-6d4fe75baf65"
   },
   "zone":{
      "href":"https://us-south.iaas.cloud.ibm.com/v1/regions/us-south/zones/us-south-3"
   },
   "primary_network_interface":{
      "name":"ifv-nic",
      "subnet":{
         "id":"7382-4b33d349-1752-4bf9-ab73-751ec065e193"
      }
   }
}'
```
{: codeblock}

#### Step 2 - Stop the running instance
{: #ifv-stop-instance-api}

Stop the running instance by specifying the `stop` action in a `POST /instances` call:

```curl
curl -X POST \
"$vpc_api_endpoint/v1/instances/{instance-id}/gen/actions?version=2021-05-20"
$iam_token" -d
'{
   "type":"stop"
}'
```
{: codeblock}

#### Step 3 - Create an image and provide the ID of the boot volume image
{: #ifv-create-image}

The source boot volume originates from an image. This boot volume is used to populate the new image's operating system information.

Create an image with a `POST /images` request and pass the boot volume ID of the instance that you created in [Step 1](#ifv-create-instance) as source volume. See the following example:

```curl
curl -X POST \
"$vpc_api_endpoint/v1/images?version=2021-05-20"
$iam_token" -d
{
    "name": "test-ifv-boot-volume",
    "source_volume": {
    	"id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7"
    }
}
```
{: codeblock}

In the example response, `source_volume` indicates the boot volume that is used to create the image. Also notice that the image encryption appears as `none` because the source volume used the default IBM-managed encryption. If you [used you own root key](#ifv-use-crk), the response would show `user-managed` instead.

```json
{
  "created_at": "2021-05-20T00:05:13.873893Z",
  "crn": "crn:[...]",
  "encryption": "none",
  "file": {},
  "href": "https://us-south.iaas.cloud.ibm.com/v1/images/4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
  "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
  "name": "test-ifv-boot-volume",
  "operating_system": {
    "architecture": "amd64",
    "display_name": "CentOS 8.x - Minimal Install (amd64)",
    "family": "CentOS",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/centos-8-amd64",
    "name": "centos-8-amd64",
    "vendor": "CentOS",
    "version": "8.x - Minimal Install"
  },
  "resource_group": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/resource_groups/3fad3f2204eb4998c3964d254ffcd771",
    "id": "3fad3f2204eb4998c3964d254ffcd771",
    "name": "Default"
  },
  "status": "pending",
  "status_reasons": [],
  "visibility": "private"
  "source_volume":  {
    "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
    "crn": "crn:[...]",
    "href": "",
    "name": "ifv-boot-vol"
  }
}
```
{: codeblock}

### Create an image from volume and specify your own encryption key
{: #ifv-use-crk}

In this scenario, you're creating an image from a volume and specifying your root key in the `POST/images` call.

The example request specifies the CRN of the root key in the `encryption_key` parameter:

```cURL
curl -X POST \
"$vpc_api_endpoint/v1/images?version=2021-05-20"
$iam_token" -d
{
    "name": "test-ifv-boot-volume",
    "source_volume": {
        "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7"
    },
    "encryption_key":{
      "crn":"crn:[...key:...]"
    },
}
```
{: codeblock}

The response includes information about the root key:

```json
{
  "created_at": "2021-05-20T00:05:13.873893Z",
  "crn": "crn:[...]",
  "encryption_key": {
    "crn": "crn:[...key:...]"
    },
  "encryption": "user_managed",
  "file": {},
  "href": "https://us-south.iaas.cloud.ibm.com/v1/images/4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
  "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
  "name": "test-ifv-boot-volume",
  "operating_system": {
    "architecture": "amd64",
    "display_name": "CentOS 8.x - Minimal Install (amd64)",
    "family": "CentOS",
    "href": "https://us-south.iaas.cloud.ibm.com/v1/operating_systems/centos-8-amd64",
    "name": "centos-8-amd64",
    "vendor": "CentOS",
    "version": "8.x - Minimal Install"
  },
  "resource_group": {
    "href": "https://us-south.iaas.cloud.ibm.com/v1/resource_groups/3fad3f2204eb4998c3964d254ffcd771",
    "id": "3fad3f2204eb4998c3964d254ffcd771",
    "name": "Default"
  },
  "status": "pending",
  "status_reasons": [],
  "visibility": "private"
  "source_volume":  {
    "id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7",
    "crn": "crn:[...]",
    "href": "",
    "name": "ifv-boot-vol"
  }
}
```
{: codeblock}

### Create an image from a volume of an existing instance
{: #ifv-vpc-api-list-instance}

You can also create an image from a boot volume that is attached to an existing instance. This procedure:

1. Lists all instances and then, gets the ID of a boot volume that is attached to an available, running instance.
2. Stops the running instance.
3. Creates an image by using the ID of the boot volume image.

#### Step 1 - Locate the instance and the boot volume ID
{: #ifv-locate-instance}

Make a `GET /instances` call to list all instances and locate the available, running instance that you need. See the following example:

```cURL
curl -X GET \
"$vpc_api_endpoint/v1/instances/version=2021-05-20&generation=2" \
-H "Authorization: $iam_token"
```
{: codeblock}

Make a `GET /instances/{id}` to view details of a single instance and locate its boot volume ID.

The response shows the ID of the boot volume under `volume_attachments`:

```sh
  "volume_attachments": [
    {
      "device": {
        "id": "a8a15363-a6f7-4f01-af60-715e85b28141"
      },
      "href": "https://us-south.iaas.cloud.ibm.com/v1/instances/e402fa1b-96f6-4aa2-a8d7-703aac843651/volume_attachments/7389-a8a15363-a6f7-4f01-af60-715e85b28141",
      "id": "a8a15363-a6f7-4f01-af60-715e85b28141",
      "name": "my-boot-volume-attachment",
      "volume": {
        "crn": "crn:[...]",
        "href": "https://us-south.iaas.cloud.ibm.com/v1/volumes/49c5d61b-41e7-4c01-9b7a-1a97366c6916",
        "id": "49c5d61b-41e7-4c01-9b7a-1a97366c6916",
        "name": "my-boot-volume"
      }
    },
```
{: codeblock}

You can also tell whether the instance is running by making a `GET /volumes/{id}` call and specifying the boot volume ID. If you see `active = true` in the response, the instance is running.
{: tip}

#### Step 2 - Stop the running instance
{: #ifv-stop-instance}

Stop the running instance by specifying the `stop` action in a `POST /instances` call:

```cURL
curl -X POST \
"$vpc_api_endpoint/v1/instances/{instance-id}/gen/actions?version=2021-05-20"
$iam_token" -d
'{
   "type":"stop"
}'
```
{: codeblock}


#### Step 3 - Create an image and provide the ID of the boot volume image
{: #ifv-create-image-boot-id}

Create an image with a `POST /images` request and pass the boot volume ID of the instance.

```cURL
curl -X POST \
"$vpc_api_endpoint/v1/images?version=2021-05-20"
$iam_token" -d
{
    "name": "test-ifv-boot-volume",
    "source_volume": {
    	"id": "4fec84ef-baf9-405d-bf3b-9d5a60e068f7"
    }
}
```
{: codeblock}

## Next steps
{: #ifv-next-steps-api}

[Use your image from a volume](#ifv-image-creation-completed) when you create an instance.
[Manage your image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc-manage).
