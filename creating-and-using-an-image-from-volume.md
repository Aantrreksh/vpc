---

copyright:
  years: 2021, 2023
lastupdated: "2023-01-23"

keywords: image, virtual private cloud, boot volume, virtual server instance, instance, instances, virtual servers, creating virtual servers, virtual server instances, virtual machines, Virtual Servers for VPC, compute, vsi, vpc, creating, UI, console

subcollection: vpc

content-type: tutorial
account-plan: paid
completion-time: 15m

---

{{site.data.keyword.attribute-definition-list}}

# Creating a custom image from volume and provisioning an instance in the UI
{: #creating-and-using-an-image-from-volume}
{: toc-content-type="tutorial"}
{: toc-completion-time="15m"}

This tutorial demonstrates how to create a custom image from the boot volume of a virtual server instance, and then use that image to create a new instance.
{: shortdesc}

## Before you begin
{: #before-you-begin-create-image}

This tutorial requires the following prerequisites.

- An IBM Cloud billable account.
- An IBM Cloud [VPC](/docs/vpc?topic=vpc-getting-started).
- An IBM Cloud [SSH key](/docs/vpc?topic=vpc-ssh-keys).
- An IBM Cloud [virtual server instance](/docs/vpc?topic=vpc-creating-virtual-servers).

## Objectives
{: #objectives}

- Create a resource group, *Image resources*.
- Create a custom image from an instances' boot volume.
- Create a new virtual server instance by using the custom image.

## Creating a resource group
{: #creating-a-resource-group}
{: step}

You can use [resource groups](/docs/vpc?topic=vpc-iam-getting-started#resources-and-resource-groups) to organize images.

1. Log in to the [{{site.data.keyword.Bluemix_notm}} console](/login){: external}.
2. Click **Manage** > **Account**.
3. From the *Account* page, click **Resource groups** > **Create**.
4. Give the resource group a unique name such as *Image resources*.
5. Click **Add**.

## Creating an image from a volume in the UI
{: #image-from-volume-vpc-ui}
{: step}

You can use the UI to create an image from a volume that is attached to an available virtual server instance as the primary boot volume.

### Choose the source for your custom image
{: #import-custom-image-source-UI}

Complete the following actions to import your custom image in the UI.

1. In [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Custom Images**.
2. Click **Create**. The Import custom image page is displayed.
3. Complete the following required fields on the Import custom images page.

| Field | Value |
|-------|-------|
| Name | Custom OS |
| Resource group | Image resources |
| Tags | Custom image |
| Region | Dallas 2 |
| Source | [Virtual server instance boot volume](#import-custom-image-vsi) (default) |
{: caption="Table 1. Import custom image user interface fields" caption-side="bottom"}

### Create an image from a virtual server instance boot volume
{: #import-custom-boot-image-vsi}

When you select **Virtual server instance boot volume** as the source of your custom image, a list of instances is displayed. Complete the following actions to create an image from the instance's boot volume.

1. On the **Import custom image** page, select **Virtual server instance boot volume** (default).
2. Select an instance from the list. You're selecting the boot volume of the instance.
3. Make sure that the instance is not running. You can stop a running instance by clicking the overflow menu (ellipsis) and click **Stop**.
4. Select IBM-managed encryption as the encryption type.
5. On the side panel, click **Import custom image**.

## Creating virtual server instances in the UI
{: #creating-virtual-servers-ifv}
{: step}

You can create virtual server instances in your {{site.data.keyword.vpc_short}} in the {{site.data.keyword.cloud_notm}} console. Complete the following actions to create a virtual server instance.

1. In the [{{site.data.keyword.cloud_notm}} console](/login){: external}, go to **menu icon ![menu icon](../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Virtual server instances**.

2. Click **Create** and enter the required information.

   | Field | Value |
   |-------|-------|
   | Name  | Server Instance |
   | Resource group | Image resources |
   | Tags | OS |
   | Location | Dallas 2 |
   | Type of virtual server | Public |
   | Operating system | Select **Custom image** > (custom image). |
   | SSH key | An existing SSH key \n For more information, see [SSH keys](/docs/vpc?topic=vpc-ssh-keys). |
   | Data volumes | You can add one or more secondary data volumes to be included when you provision the instance. To add volumes, click **Create** in the Data volumes section of the page. \n For more information about provisioning the volume, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage#create-from-vsi). |
   | Networking | Select your pre-existing VPC |
   {: caption="Table 2. Instance provisioning selections" caption-side="bottom"}

3. Click **Create virtual server instance** when you are ready to provision.

## Related content
{: #related-content}

* [About creating an image from a volume](/docs/vpc?topic=vpc-image-from-volume-vpc)
* [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv)
* [View and use your image from a volume](/docs/vpc?topic=vpc-create-ifv#ifv-image-creation-completed)
* [resource group](/docs/account?topic=account-account_setup)
* [Best practices for organizing resources and assigning access](/docs/account?topic=account-account_setup)
