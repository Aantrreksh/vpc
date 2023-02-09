---

copyright:
  years: 2019, 2023
lastupdated: "2023-02-09"

keywords: 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Managing custom images 
{: #managing-custom-images}

After you import a custom image to {{site.data.keyword.vpc_short}}, you can view details about the image, use the image to create a virtual server instance, export the image, and even share it with a private catalog. 
{: shortdesc}

To manage an image from volume, see [Managing image from volume](/docs/vpc?topic=vpc-image-from-volume-vpc-manage&interface=ui). To use a custom image in a private catalog, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).
{: note}

For details about the `$vpc_api_endpoint` and `$iam_token` variables in the following examples, see the Authentication and Endpoint URLs sections in [Virtual Private Cloud API Introduction](/apidocs/vpc#about-vpc-api).
{: api}

## Managing custom images using the UI
{: #custom-images-managing-ui}
{: ui}

You can manage an image by using the {{site.data.keyword.cloud_notm}} console.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images**.
2. On the **Custom images** tab,  click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) for a specific image and select from the available options. Encrypted custom images are identified by a lock icon after the image name. You can select from the following actions:

| Action | Description |
|--------|-------------|
| Rename | Rename the custom image. |
| Create virtual server instance | Create a new virtual server from the custom image. |
| Export | Copy the custom image to {{site.data.keyword.cos_full_notm}}. |
| Share to catalog | Share the {{site.data.keyword.vpc_short}} custom image to an existing private catalog. |
| Copy | Copy the UUID of the custom image. |
| View checksum | View the checksum for the custom image. |
| Delete | Delete the custom image.  \n  \n  If you want to delete a custom image in a private catalog, see [Deleting a custom image in a private catalog](/docs/vpc?topic=vpc-planning-custom-images#deleting-private-catalog-custom-image-vpc). |
{: caption="Table 1. Custom image actions" caption-side="bottom"}

## Listing custom images using the CLI
{: #custom-images-list-cli}
{: cli}

You can list all of the {{site.data.keyword.vpc_short}} custom images in your region by using the command-line interface (CLI).

To list all custom images by using the CLI, use the **ibmcloud is images** command. Custom images are private to the account where they are created, so the `--visibility` option is `private`. 

```sh
ibmcloud is images --visibility private
```
{: pre}

For more information, see [ibmcloud is images](/docs/vpc?topic=vpc-vpc-reference#images) in the VPC CLI reference page.

## Listing all images with the API
{: #custom-images-list-api}
{: api}

You can list all of the {{site.data.keyword.vpc_short}} images in your region by using the application programming interface (API).

To list all images by using the API, use [List all images](/apidocs/vpc/latest#list-images). Custom images are private to the account where they are created, so you can set the `visibility` property to `private` to retrieve only custom images. 

```sh
curl -X GET \
”$vpc_api_endpoint/v1/images?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token"
```
{: codeblock}

## Viewing custom image details 
{: #viewing-custom-image-details}
{: ui}

You can view the following details of a custom image:

* Name and ID
* Assigned resource group 
* Size 
* Location 
* Creation date 
* Operating system and version 
* Encryption type

You can edit the name of the custom image, adds tags for searchability, and view and copy the unique Cloud Resource Name (CRN). The SHA256 checksum value for the image is accessible.
{: tip} 

To view details for a custom image, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images**.
2. On the **Custom images** tab, click the name of a custom image to view details about that image. 
3. On the **Image details** page you can edit the name of the image, add tags, and copy the CRN for the image. 
4. From the **Actions** menu of the Image details page, you can create a virtual server instance from the custom image, or delete the image. 

## Viewing custom image details using the CLI
{: #custom-image-details-cli}
{: cli}

You can view the details of a custom image, such as name and ID, operating system and version, encryption status, and assigned resource group, by using the command-line interface (CLI).

To view the details of a custom image by using the CLI, use the **ibmcloud is image** command. Specify the name or ID of the custom image with the `IMAGE` variable.

```sh
ibmcloud is image IMAGE 
```
{: pre}

For more information, see [ibmcloud is image](/docs/vpc?topic=vpc-vpc-reference#image) in the VPC CLI reference page.

## Retrieving custom image details with the API
{: #custom-image-retrieve-api}
{: api}

You can view the details of a custom image, such as name and ID, operating system and version, encryption status, and assigned resource group, by using the application programming interface (API).

To retrieve the details of a specific custom image by using the API, use [Retrieve an image](/apidocs/vpc/latest#get-image). For the`$image_id` variable, specify the ID of the custom image that you want to retrieve. 

```sh
curl -X GET \
”$vpc_api_endpoint/v1/images/$image_id?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token"
```
{: codeblock}

## Exporting a custom image to {{site.data.keyword.cos_full_notm}} (Beta)
{: #custom-image-export-to-cos}

Export custom image is a beta feature that is available to select customers for evaluation and testing purposes. To request to be included in the evaluation of this beta feature, contact IBM Support. 
{: beta}

You can export a custom image to {{site.data.keyword.cos_full_notm}}. You might want to subsequently copy the image to a new region by importing it again, or you might want to extract the image outside of VPC for use in a different location, such as on premises. Be aware that charges might be incurred for storing images in {{site.data.keyword.cos_full_notm}}. For more information, see [Billing](/docs/cloud-object-storage?topic=cloud-object-storage-billing). 

### Prerequisites
{: #custom-image-export-to-cos-prereqs}

To export a custom image, you must meet the following prerequisites: 

* You must have an IAM role that includes the `is.image.image.export` and `is.image.image.operate` actions. For more information, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started). 
* You must have an {{site.data.keyword.cos_full_notm}} service instance created. You must also create an authorization for the Image Service for VPC to access {{site.data.keyword.cos_full_notm}} with the Writer service access role. For more information, see [Granting access to {{site.data.keyword.cos_full_notm}} to import and export images](/docs/vpc?topic=vpc-object-storage-prereq).
* You must have IAM access to write to the bucket where you plan to export an image. For more information, see [Assigning access to an individual bucket](/docs/cloud-object-storage?topic=cloud-object-storage-iam-bucket-permissions). 

### Exporting a custom image using the UI
{: #custom-image-export-to-cos-ui}
{: ui}

To export a custom image to {{site.data.keyword.cos_full_notm}}, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images**.
2. On the **Custom images** tab, click the name of the custom image that you want to export. 
3. On the image details page, click the **Actions** menu and select **Export**.
4. On the **Export custom image panel**, you can optionally specify a name for the export job. The name is used, along with the image name, to construct the name of the exported object in {{site.data.keyword.cos_full_notm}}. If you do not specify a name, a name is automatically generated.
5. Select the file format for exporting. Valid file formats are **QCOW2** and **VHD**. If the custom image is an encrypted image, **QCOW2** format is required.
6. Specify the {{site.data.keyword.cos_full_notm}} service instance and bucket where you want to export the custom image. Alternatively you can specify the CRN for the {{site.data.keyword.cos_full_notm}} bucket where you want to export the image.  
7. Click **Export custom image**. 

### Exporting a custom image using the CLI
{: #custom-image-export-to-cos-cli}
{: cli}

You can export an {{site.data.keyword.vpc_short}} custom image to {{site.data.keyword.cos_full_notm}} by using the command-line interface (CLI).

To export a custom image by using the CLI, use the **ibmcloud is image-export-job-create** command. Specify the name or ID of the custom image to export by using the `IMAGE` variable. You have the option to specify a name for the export job with `NEW_NAME`. You can also specify the file format for exporting the image, either `qcow2` or `vhd`. The `qcow2` format is the default file format and is the required format if the custom image is encrypted. Additionally, specify the {{site.data.keyword.cos_full_notm}} `BUCKET` where you want to export the custom image. 

```sh
ibmcloud is image-export-job-create IMAGE [--name NEW_NAME] [--format qcow2 | vhd] --bucket BUCKET
```
{: pre}

The following example exports a custom image with the ID of `72251a2e-d6c5-42b4-97b0-b5f8e8d1f479`. The export job is named `my-export-job`. The file format for exporting the image is `qcow2`. The {{site.data.keyword.cos_full_notm}} bucket where the image exported is named `my-bucket`. 

```sh
ibmcloud is image-export-job-create 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479 --name my-export-job --format qcow2 --bucket my-bucket
```
{: pre}


### Exporting a custom image with the API
{: #custom-image-export-to-cos-api}
{: api}

You can export an {{site.data.keyword.vpc_short}} custom image to {{site.data.keyword.cos_full_notm}} by using the application programming interface (API). 

To export a custom image by using the API, use [Create an image export job](/apidocs/vpc-beta#create_image_export_job). 

For the`$image_id` variable, specify the ID of the custom image that you want to export. You can optionally specify a meaningful `name` for the image export job. In this example, the export job name is `my-image-export`. For the `storage_bucket` `name` sub-property, specify the name of the {{site.data.keyword.cos_full_notm}} bucket where you want to export the custom image. In this example the bucket name is `bucket-27200-lwx4cfvcue`. Alternatively, you can use the `crn` sub-property to specify the CRN of the bucket. 

```sh
curl -X POST \
"$vpc_api_endpoint/v1/images/$image_id/export_jobs?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token" \
-d '{
  "name": "my-image-export",
  "storage_bucket": {
    "name": "bucket-27200-lwx4cfvcue"
  }
}'
```
{: codeblock}

## Viewing and managing custom image export history (Beta)
{: #custom-image-export-job-history}

You can view the custom image export history to see details such as the export job name, status, target bucket in {{site.data.keyword.cos_full_notm}}, the {{site.data.keyword.cos_full_notm}} object name that is assigned, the date that the export job started, and if the export job has completed, the completion date. 

You can also delete a completed image export job or cancel (and delete) an in-progress image export job if you have an IAM role that includes the `is.image.image.export` action. 

When you delete an image export job, it is removed only from the export history. The exported image file is not removed from the {{site.data.keyword.cos_full_notm}} bucket. You must manage files stored in {{site.data.keyword.cos_full_notm}} by using the {{site.data.keyword.cos_full_notm}} interfaces.
{: important}

Finally, you can rename an image export job. Renaming the image export job does not change the name of the object that is created in {{site.data.keyword.cos_full_notm}}. 

### Limits for image export jobs
{: #custom-image-export-job-limits}

The following limits exist for image export jobs:

* 5 active export job requests per image
* 10 total export jobs per image
* 20 active export jobs per account per region

### Viewing custom image export history using the UI 
{: #custom-image-export-job-history-ui}
{: ui}

To view the export history of a custom image, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images**.
2. On the **Custom images** tab, click the name of a custom image to view details about that image. 
3. On the **Image details** page, click the **Export history** tab. 

#### Deleting or canceling an image export job using the UI 
{: #custom-image-export-job-delete-ui}
{: ui}

To delete a completed image export job or cancel (and delete) an in-progress image export job, complete the following steps.

1. On the image details page, click the **Export history** tab. 
2. For the export job that you want to delete or cancel, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) and select **Delete**.

#### Renaming an image export job using the UI 
{: #custom-image-export-job-rename-ui}
{: ui}

To rename an image export job, complete the following steps.

1. On the image details page, click the **Export history** tab. 
2. For the export job that you want to rename, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) and select **Rename**.


### Viewing custom image export history using the CLI
{: #custom-image-export-job-history-cli}
{: cli}

To view the export history of a specific custom image by using the CLI, use the **ibmcloud is image-export-jobs** command. Specify the name or ID of the custom image that was exported by using the `IMAGE` variable. 

```sh
ibmcloud is image-export-jobs IMAGE
```
{: pre}

The following example shows the export job history for a custom image with the ID of `72251a2e-d6c5-42b4-97b0-b5f8e8d1f479`. 

```sh
ibmcloud is image-export-jobs 72251a2e-d6c5-42b4-97b0-b5f8e8d1f479
```
{: pre}


### Viewing image export job details using the CLI
{: #custom-image-export-job-details-cli}
{: cli}

To view the details of a specific image export job by using the CLI, use the **ibmcloud is image-export-job** command. Specify the name or ID of the custom image that was exported by using the `IMAGE` variable. Specify the name or ID of the image export job by using the `JOB` variable. 

```sh
ibmcloud is image-export-job IMAGE JOB
```
{: pre}

### Deleting or canceling an image export job using the CLI 
{: #custom-image-export-job-delete-cli}
{: cli}

To delete a completed image export job or cancel (and delete) an in-progress image export job, use the **ibmcloud is image-export-job-delete** command. Specify the name or ID of the custom image that was exported by using the `IMAGE` variable. Specify the name or ID of the image export job by using the `JOB` variable. 

```sh
ibmcloud is image-export-job-delete IMAGE JOB
```
{: pre}

### Renaming an image export job using the CLI 
{: #custom-image-export-job-rename-cli}
{: cli}

To rename a custom image export job by using the CLI, use the **ibmcloud is image-export-job-update** command. Specify the name or ID of the custom image that was exported by using the `IMAGE` variable. Specify the name or ID of the image export job by using the `JOB` variable. Specify the new name that you want to assign to the image export job by using the `NAME` variable.  

```sh
ibmcloud is image-export-job-update IMAGE JOB --name NAME
```
{: pre}

### Viewing custom image export history with the API
{: #custom-image-export-job-history-api}
{: api}

To view the export history of a specific custom image by using the API, use [List all image export jobs](/apidocs/vpc-beta#list_image_export_jobs). 

For the`$image_id` variable, specify the ID of the custom image for which you want to display export jobs. 

```sh
curl -X GET \
"$vpc_api_endpoint/v1/images/$image_id/export_jobs?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token" 
```
{: codeblock}

### Viewing image export job details using the API
{: #custom-image-export-job-details-api}
{: api}

To view the details of a specific image export job by using the API, use [Retrieve an image export job](/apidocs/vpc-beta#get-image-export-job). 

For the`$image_id` variable, specify the ID of the custom image that was exported. For the `$export_job_id` variable, specify the ID of the image export job for which you want to retrieve details. 

```sh
curl -X GET \
"$vpc_api_endpoint/v1/images/$image_id/export_jobs/$export_job_id?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token" 
```
{: codeblock}

### Deleting or canceling an image export job with the API
{: #custom-image-export-job-delete-api}
{: api}

To delete a completed image export job or cancel (and delete) an in-progress image export job by using the API, use [Delete an image export job](/apidocs/vpc-beta#delete-image-export-job). 

For the`$image_id` variable, specify the ID of the custom image for which you want to delete an export job. For the `$export_job_id` variable, specify the ID of the image export job that you want to delete. 

```sh
curl -X DELETE \
"$vpc_api_endpoint/v1/images/$image_id/export_jobs/$export_job_id?version=2022-11-21&generation=2" \
-H "Authorization: Bearer $iam_token" 
```
{: codeblock}

### Renaming an image export job with the API
{: #custom-image-export-job-rename-api}
{: api}

To rename a custom image export job by using the API, use [Update an image export job](/apidocs/vpc-beta#update-image-export-job). 

For the`$image_id` variable, specify the ID of the custom image for which you want to rename an export job. For the `$export_job_id` variable, specify the ID of the image export job that you want to rename. The new name to assign to the image export job in the following example is `my-export-job-patched`.

```sh
curl -X PATCH \
"$vpc_api_endpoint/v1/images/$image_id/export_jobs/$export_job_id?version=2022-11-21&generation=2" \
-H 'Content-Type: application/json' \
-H "Authorization: Bearer $iam_token" \
-d '{
      "name": "my-export-job-patched"
    }'
```
{: codeblock}

## Sharing a custom image to a private catalog
{: #sharing-custom-image-private-catalog-ui}
{: ui}

You can share an existing {{site.data.keyword.vpc_short}} custom image to an existing private catalog. If you don't already have a private catalog, see [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui).

The **Share image to private catalog** action is not available for the following images:
* The image is already shared with a private catalog. An image in a private catalog can exist in only one version within one product offering in one private catalog. You must remove the image from the version it is in first before that image can be shared in a different version.
* The image is encrypted.
* The image is not an x86 image.

To share a custom image to a private catalog, complete the following steps.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Images**.
1. On the **Custom images** tab, click the Actions icon ![More Actions icon](../icons/action-menu-icon.svg) and select **Share to catalog**.
1. On the **Share image to private catalog** page, select a **Private catalog** from the list of available private catalogs.
1. Enter a version number in **Software version**.
1. Click **Create catalog offering** to share the image to the private catalog.
1. After the image is created, click **Validate image**.

This process shares only the custom image to the private catalog. The validation process occurs within the private catalog. Clicking **Validate image** takes you to a new page and from this screen, you can start with [Step 3: Validate the virtual server image in Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial&interface=ui&locale=en#catalog-vsivpc-review-images) to complete the validation process.
{: note}
