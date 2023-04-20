---

copyright:
  years: 2019, 2023

lastupdated: "2023-04-06"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Importing and validating custom images into VPC
{: #importing-custom-images-vpc}

You can create your own custom image on premises and then import it to {{site.data.keyword.vpc_full}} infrastructure from {{site.data.keyword.cos_full}}. Then, you can use your custom image to create new virtual server instances that runs on the KVM hypervisor. If you plan to use your custom image in a private catalog, you must first import and validate the custom image into {{site.data.keyword.vpc_short}}.
{: shortdesc}

You can also create a custom image of a boot volume that is attached to a server at import time. For more information, see [About creating an image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc).
{: note}

## Prerequisites
{: #pre-req-import-custom-image-cloud-object-storage}

To complete this task, you must have an instance of {{site.data.keyword.cos_full}} available. You must also create an authorization so that the Image Service for VPC can access images in {{site.data.keyword.cos_full_notm}}. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import images](/docs/vpc?topic=vpc-object-storage-prereq).

{{site.data.content.custom-image-requirements-list}}

Keep the following considerations to keep in mind when you import a custom image:

* When you import a custom image, it's private to the account where you import it. Also, the region where you choose to import the image is the region where you can create virtual server from that image.
* You can't create an image from an encrypted boot volume (image from a volume) that is not 100 GB. The operation is blocked.
* For custom images with Red Hat Enterprise Linux&reg; or Windows&reg; operating systems, you must select the appropriate version of the operating system. Depending on how you configured the image, select either bring your own license `byol`, or if you plan to license the OS through {{site.data.keyword.cloud_notm}}, select the version without `byol` appended.

{{site.data.content.custom-image-information-link}}

## Importing a custom image by using the UI
{: #import-custom-image-cloud-object-storage-ui}
{: ui}

When you have an image available in {{site.data.keyword.cos_full_notm}}, you can import it to {{site.data.keyword.vpc_short}} infrastructure by using the {{site.data.keyword.cloud_notm}} console.

The Image lifecycle feature is a beta feature that is available for evaluation and testing purposes.
{: beta}

1. Make sure that your compatible custom image is available in {{site.data.keyword.cos_full_notm}}. To upload an image to {{site.data.keyword.cos_full_notm}}, on the **Objects** page of your bucket, click **Upload**. You can use the Aspera high-speed transfer plug-in to upload images larger than 200 MB. For more information, see [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image), [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image), [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about) and [Uploading data](/docs/cloud-object-storage?topic=cloud-object-storage-upload) to {{site.data.keyword.cos_full_notm}}.
1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images**.
1. On the **Custom images** tab, click **Create**.
1. Complete the fields that are described in Table 1 and click **Create Custom Image**.

| Field | Value |
|-------|-------|
| Name  | A name is required for your custom image. |
| Resource group | Select a resource group for the server. |
| Tags |  You can assign a label to this resource so that you can easily filter resources in your resource list. |
| Region | Select the location, or specific geographic area, where you want your custom image to be available for provisioning.|
| Source | Select **Cloud {{site.data.keyword.cos_short}}** as the source. A list displays from which you select a {{site.data.keyword.cos_full_notm}} service instance where the image that you want to import is stored. \n Do you need to create a custom image from a volume instead? Select the source for the custom image by choosing either **Virtual server instance boot volume** or **Block storage boot volume**. For more information, see [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv). |
| Location | Select the specific geographic region where your image is stored. |
| Bucket | Select the {{site.data.keyword.cos_full_notm}} bucket where your image is stored.|
| Name | Select the image file in the {{site.data.keyword.cos_full_notm}} service instance that you want to import. If you are importing an encrypted image, the image must be encrypted with LUKS encryption by using QEMU and your own passphrase. For more information, see [Encrypting the image](/docs/vpc?topic=vpc-create-encrypted-custom-image#manually-encrypt-image). |
| Operating System | Select the operating system that is included in your image. \n \n For custom images with Red Hat Enterprise Linux or Windows operating systems, you can bring your own license (BYOL) or to license through {{site.data.keyword.cloud_notm}}. \n \n For Red Hat Enterprise Linux or Windows [BYOL custom images](/docs/vpc?topic=vpc-byol-vpc-about), you must select the OS with `-byol` appended to the name. For example, if you have a Windows 2019 BYOL custom image, select *Windows-2019-amd64-byol* for the operating system. Failure to select the `-byol` version of the operating system when you import a BYOL custom image might result in a virtual server that is unable to start. \n \n If you configured your Red Hat Enterprise Linux or Windows custom image to license through {{site.data.keyword.cloud_notm}}, you must select the nonbyol operating system. For example, if you have a Windows 2019 custom image that you plan to license through {{site.data.keyword.cloud_notm}}, select *Windows-2019-amd64* as the operating system when you import the custom image. |
| Manage image lifecycle (optional) | Select to schedule status changes for the image. You can schedule a single status change or schedule the complete lifecycle of the image. The image statuses are:  \n  \n * `available`: The image can be used to create an instance.  \n  \n * `deprecated`: The image is still available to use to provision an instance. Using the `deprecated` status can discourage use of the image before the status changes to `obsolete`.  \n * `obsolete`: The image is not available to use to provision an instance.  \n  \n * Schedule complete lifecycle: You can schedule both the `deprecated` and `obsolete` status changes at the same time.  \n  \n You can move back and forth between the three statuses. Only the statuses you can change to are displayed. You can schedule status changes by using calendar date and time or number of days. The obsolescence date must always be after the deprecation date. |
| Encryption | The default selection is **No encryption**. If you have not encrypted your image by using QEMU, use the default value, **No encryption**. If you are importing an image that you have encrypted by using QEMU and your own passphrase, select the key management service where your customer root key (CRK) that protects your passphrase is stored. Select either **Key Protect** or **Hyper Protect Crypto Services**. VHD format images are not supported for encryption. |
| Encryption service instance | For an encrypted image, select the specific instance of the key management service where your CRK that wraps your encryption passphrase is stored. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs). |
| Key name | Select the customer root key (CRK) that you used to wrap your encryption passphrase. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs). |
| Wrapped data encryption key | For an encrypted image, specify the ciphertext that is associated with the wrapped data encryption key (WDEK). The WDEK is produced by wrapping the passphrase that you used to encrypt your image with your customer root key. For more information, see [Setting up your key management service and keys](/docs/vpc?topic=vpc-create-encrypted-custom-image#kms-prereqs).|
{: caption="Table 1. Import custom image user interface fields" caption-side="bottom"}

## Importing a custom image by using the CLI
{: #import-custom-image-cloud-object-storage-cli}
{: cli}

Make sure that your compatible custom image is available in {{site.data.keyword.cos_full_notm}}. For more information, see [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image), [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image), [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about) and [Uploading data](/docs/cloud-object-storage?topic=cloud-object-storage-upload) to {{site.data.keyword.cos_full_notm}}.

When you have an image available in {{site.data.keyword.cos_full_notm}}, you can import it to {{site.data.keyword.vpc_short}} infrastructure by using the command-line interface (CLI).

To import a custom image by using the CLI, use the **`ibmcloud is image-create`** command. Specify the name of the custom image to be created by using the `IMAGE_NAME` variable. You must also specify the source; for example, specify the `--file` option with the image file location. Specify the `--os-name` option with the name of the operating system for the image.

```sh
ibmcloud is image-create IMAGE_NAME [--file IMAGE_FILE_LOCATION] [--os-name OPERATING_SYSTEM_NAME]
```
{: pre}

The following example creates a custom image with the name of `my-ubuntu-16-amd64`. The image source location is `cos://us-south/custom-image-vpc-bucket/customImage-0.qcow2`. The operating system is `ubuntu-16-amd64`.

```sh
ibmcloud is image-create my-ubuntu-16-amd64 --file cos://us-south/custom-image-vpc-bucket/customImage-0.qcow2 --os-name ubuntu-16-amd64
```
{: pre}

For more information, see [ibmcloud is image-create](/docs/vpc?topic=vpc-vpc-reference#image-create) in the VPC CLI reference page.

### Schedule custom image lifecycle status changes by using the CLI
{: #import-schedule-ilm-status-change-cli}

Custom image lifecycle is a beta feature that is available for evaluation and testing purposes.
{: beta}

When you import a custom image by using the command-line interface (CLI), you can also schedule the lifecycle status changes of an {{site.data.keyword.vpc_short}} custom image at the same time by using options of the **`ibmcloud is image-create`** command.

Specify the name of the custom image to be created by using the `IMAGE_NAME` variable. You must also specify the source; for example, specify the `--file` option with the image file location. Specify the `--os-name` option with the name of the operating system for the image.

To schedule the `deprecated` or `obsolete` status changes at the same time, use the `--deprecate-at` and `--obsolete-at` options that are specified in the `YYYY-MM-DDThh:mm:ss+hh:mm` format. If you define both the deprecate-at and obsolete-at dates, the obsolete-at date must be after the deprecate-at date. Both dates must be in the future.


```sh
ibmcloud is image-create IMAGE_NAME [--file IMAGE_FILE_LOCATION] [--os-name OPERATING_SYSTEM_NAME] [--deprecate-at YYYY-MM-DDThh:mm:ss+hh:mm] [--obsolete-at YYYY-MM-DDThh:mm:ss+hh:mm]
```
{: pre}

The following example creates a custom image with the name of `my-ubuntu-16-amd64`. The image source location is `cos://us-south/custom-image-vpc-bucket/customImage-0.qcow2`. The operating system is `ubuntu-16-amd64`. The image is scheduled to be deprecated on `2023-03-01T06:11:28+05:30`. The image is scheduled to be obsolete on `2023-12-31T06:11:28+05:30`.

```sh
ibmcloud is image-create my-ubuntu-16-amd64 --file cos://us-south/custom-image-vpc-bucket/customImage-0.qcow2 --os-name ubuntu-16-amd64 --deprecate-at 2023-03-01T06:11:28+05:30 --obsolete-at 2023-12-31T06:11:28+05:30
```
{: pre}

For more information, see [ibmcloud is image-create](/docs/vpc?topic=vpc-vpc-reference#image-create)in the VPC CLI reference page.

## Importing a custom image by using the API
{: #import-custom-image-cloud-object-storage-api}
{: api}

Make sure that your compatible custom image is available in {{site.data.keyword.cos_full_notm}}. For more information, see [Creating a Linux custom image](/docs/vpc?topic=vpc-create-linux-custom-image), [Creating a Windows custom image](/docs/vpc?topic=vpc-create-windows-custom-image), [Bring your own license](/docs/vpc?topic=vpc-byol-vpc-about) and [Uploading data](/docs/cloud-object-storage?topic=cloud-object-storage-upload) to {{site.data.keyword.cos_full_notm}}.

When you have an image available in {{site.data.keyword.cos_full_notm}}, you can import it to {{site.data.keyword.vpc_short}} infrastructure by using the application programming interface (API).

To import a custom image by using the API, use [Create an image](/apidocs/vpc/latest#create-image).

Specify the `file.href` subproperty with the location of the image. Specify the `operating_system.name` subproperty with the name of the image operating system.

The following example imports a custom image with the name of `my-image`, source location of `cos://us-south/my-bucket/my-image.qcow2`, and the operating system for the image is `debian-9-amd64`.

```sh
curl -X POST "$vpc_api_endpoint/v1/images?version=2023-02-21&generation=2" -H "Authorization: Bearer $iam_token" -d '{
      "name": "my-image",
      "file": {
        "href": "cos://us-south/my-bucket/my-image.qcow2"
      },
      "operating_system": {
        "name": "debian-9-amd64"
      }
    }'
```
{: pre}

### Schedule custom image lifecycle status changes by using the API
{: #import-schedule-ilm-status-change-API}

Custom image lifecycle is a beta feature that is available for evaluation and testing purposes.
{: beta}

When you import a custom image by using the application programming interface (API), you can also schedule the lifecycle status changes of an {{site.data.keyword.vpc_short}} custom image at the same time by using the [Create an image](/apidocs/vpc/latest#create-image) command.

The `name` can't be used by another image in the region and names that start with `ibm-` are reserved for system-provided images. Specify the `file.href` subproperty with the location of the image. Specify the `operating_system.name` subproperty with the name of the image operating system.

To schedule the `deprecated` or `obsolete` status changes at the same time, use the `deprecated_at` and `obsoleted_at` subproperties that are specified in the `YYYY-MM-DDThh:mm:ss+hh:mm` format. If you define both the deprecated_at and obsoleted_at dates, the obsoleted_at date must be after the deprecated_at date. Both dates must be in the future.

The following example imports a custom image with the name of `my-image`, source location of `cos://us-south/my-bucket/my-image.qcow2`, and the operating system for the image is `debian-9-amd64`. The image is scheduled to be deprecated on `2023-03-01T06:11:28+05:30`. The image is scheduled to be obsolete on `2023-12-31T06:11:28+05:30`.

```sh
curl -X POST "$vpc_api_endpoint/v1/images?version=2023-02-21&generation=2" -H "Authorization: Bearer $iam_token" -d '{
      "name": "my-image",
      "file": {
        "href": "cos://us-south/my-bucket/my-image.qcow2"
      },
      "operating_system": {
        "name": "debian-9-amd64"
      },
      "deprecated_at": "2023-03-01T06:11:28+05:30",
      "obsoleted_at": "2023-12-31T06:11:28+05:30"
    }'
```
{: pre}

## Validating an imported custom image by using the UI
{: #validate-custom-images-cloud-object-storage-ui}
{: ui}

After you import a custom image, you can view the checksum that was generated for the image when it is imported to {{site.data.keyword.vpc_short}}.

If you generate a checksum locally for your image before you import it, you can compare the two checksums to make sure that they are identical. Matching checksums indicate that the image is unaltered.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images**.
2. On the **Custom images** tab, from your list of custom images, click the name of the custom image that you want to validate.
3. In the image details side panel, locate the **Checksum (SHA256)** field. You see content similar to, *6809606da67eb83670e6249e54e94043eb43c0471669fb96ea4050c4c07e2df7*.

   <!-- DO NOT MOVE WAZI AAS TO PUBLISH -->
   *For z/OS Wazi aaS custom image only:* You cannot validate the checksum of the imported custom images that are deployed from Wazi Image Builder.
   {: note}

   <!-- DO NOT MOVE WAZI AAS TO PUBLISH -->

4. Compare the Checksum (SHA256) value to the output that is generated when you calculate a checksum for the image locally.

   * Example Linux command: `sha256sum ubuntu_image.qcow2`
   * Example Mac command: `shasum -a 256 ubuntu_image.qcow2`
   * You receive output similar to: `6809606da67eb83670e6249e54e94043eb43c0471669fb96ea4050c4c07e2df7`

5. [Create a virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers&interface=ui) by using this image.

## Validating an imported custom image by using the CLI
{: #validate-custom-images-cloud-object-storage-cli}
{: cli}

After you import a custom image, you can view the checksum that was generated for the image when it is imported to {{site.data.keyword.vpc_short}}.

If you generate a checksum locally for your image before you import it, you can compare the two checksums to make sure that they are identical. Matching checksums indicate that the image is unaltered.

To validate an imported custom image by using the CLI, use the **`ibmcloud is image`** command.

First, view the details of the imported custom image you want to validate. Specify the name or ID of the custom image to be created by using the `IMAGE` variable.

```sh
ibmcloud is image IMAGE
```
{: pre}

For more information, see [ibmcloud is image](/docs/vpc?topic=vpc-vpc-reference#image)in the VPC CLI reference page.

The following example views the details of a custom image with the id of `r134-1d1e92e9-6550-4d06-8483-d674310045fd`.

```sh
ibmcloud is image r134-1d1e92e9-6550-4d06-8483-d674310045fd
```
{: pre}

```text
ID                             r134-589548bd-9241-4ad7-a610-1df6ba020793
Name                           my-image-from-volume-cli-1
CRN                            crn:v1:staging:public:is:us-south:a/efe5afc483594adaa8325e2b4d1290df::image:r134-589548bd-9241-4ad7-a610-1df6ba020793
Status                         available
Operating system               Name             Architecture   Vendor   Version                 Dedicated host only
                               centos-7-amd64   amd64          CentOS   7.x - Minimal Install   false

Source volume                  ID                                          Name
                               r134-6438d80f-4433-4445-be2f-0cca05afff3e   transpose-clubhouse-putt-repent

Created                        2023-03-16T01:11:03+05:30
Deprecation Date               2023-03-01T06:11:28+05:30
Obsolescence Date              2023-12-31T06:11:28+05:30
Visibility                     private
Owner type                     user
Minimum provisioned size(GB)   250
SHA256 Checksum                774d44ac0d55f2bfe869b995565715e5d9970ec23ce7127b2e2776b2618a9f7c
File size(GB)                  2
Encryption                     none
Resource group                 11caaa983d9c4beb82690daab08717e9
Resource type                  image
Catalog Offering Managed       false
```
{: codeblock}

Compare the returned Checksum (SHA256) value to the output that is generated when you calculate a checksum for the image locally.

   * Example Linux command: `sha256sum ubuntu_image.qcow2`
   * Example Mac command: `shasum -a 256 ubuntu_image.qcow2`
   * You receive output similar to: `6809606da67eb83670e6249e54e94043eb43c0471669fb96ea4050c4c07e2df7`

After you validate the Checksum (SHA256), use the image to create a virtual server. See [Creating a virtual server instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli).

## Validating an imported custom image by using the API
{: #validate-custom-images-cloud-object-storage-api}
{: api}

After you import a custom image, you can view the checksum that was generated for the image when it is imported to {{site.data.keyword.vpc_short}}.

If you generate a checksum locally for your image before you import it, you can compare the two checksums to make sure that they are identical. Matching checksums indicate that the image is unaltered.

To validate as custom image by using the API, use the [List all images](https://test.cloud.ibm.com/apidocs/vpc/latest#list-images).

For the `$image_id`, specify the ID of the custom image you want to validate.

```sh
curl -X GET "$vpc_api_endpoint/v1/images/$image_id/?version=2023-02-21&generation=2" -H "Authorization: Bearer $iam_token"
```
{: pre}

Compare the returned Checksum (SHA256) value to the output that is generated when you calculate a checksum for the image locally.

   * Example Linux command: `sha256sum ubuntu_image.qcow2`
   * Example Mac command: `shasum -a 256 ubuntu_image.qcow2`
   * You receive output similar to: `6809606da67eb83670e6249e54e94043eb43c0471669fb96ea4050c4c07e2df7`

After you validate the Checksum (SHA256), use the image to create a virtual server. See [Creating a virtual server instance by using the API](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api).

## Next steps
{: #next-validate-custom-image}

After you validate the custom images, you can deploy and manage your custom images. For more information, see [Managing custom images](/docs/vpc?topic=vpc-managing-custom-images&interface=ui). If you plan to manage your custom images with a private catalog, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).
