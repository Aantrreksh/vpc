---

copyright:
  years: 2022, 2023
lastupdated: "2023-01-30"

keywords: image, stock image, linuxone image, hpcr, container runtime, virtual private cloud, virtual server, generation 2, gen 2, wazi aas


subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# s390x virtual server images
{: #vsabout-images}

When you provision {{site.data.keyword.vsi_is_full}} on IBM Z (s390x processor architecture) in IBM Cloud, you can select from the supported stock images. Now, *IBM Hyper Protect* is also supported as an operating system and the associated *IBM Hyper Protect Container Runtime* image can be provisioned for your {{site.data.keyword.cloud_notm}} {{site.data.keyword.hpvs}} for {{site.data.keyword.vpc_full}} instance.
{: shortdesc}

The IBM Hyper Protect Container Runtime image has container runtime support and the image is not SSH enabled (is a locked down image). Even if you select and pass in an SSH key, it will not be used and the key cannot be used to connect to the instance. Currently only one container is supported. Container details are provided at instance creation through the contract, specified in the **User Data** field on the order form. Once the containers start, you can interact with the workload that is brought up on the containers. For more information, see [Contract](/docs/vpc?topic=vpc-about-contract_se).
{: note}

## Stock images
{: #vsstock-images}

The following operating systems are available as stock images when you create a virtual server.

### IBM Hyper Protect Container Runtime image
{: #hyper-protect-runtime}

You can now choose IBM Hyper Protect as the operating system for the virtual server instance. On the create virtual server page, under **Confidential computing**, click the **Run your workload with an OS and a profile protected by Secure Execution** toggle, to activate support for secure execution images. Then, in the Operating system field, "IBM Hyper Protect" operating system and the "hyper-protect-container-runtime" image must be selected to create a {{site.data.keyword.hpvs}} for VPC instance. For more information, see [Confidential computing with LinuxONE](/docs/vpc?topic=vpc-about-se).

You can choose a profile based on your requirements. You can choose from balanced, compute, and memory secure execution enabled profiles. For more information, see [s390x instance profiles](/docs/vpc?topic=vpc-vs-profiles). Ensure that you select a secure execution enabled profile (for example, bz2e-1x4) when you enable the **Run your workload with an OS and a profile protected by Secure Execution** toggle. Selecting any profile that is not secure execution enabled will cause the provisioning of the virtual instance to fail.


### Supported IBM Z or LinuxONE stock image operating systems
{: #vs-s390x-supported-os}

| Image | Architectures |
|---------|---------|
|  Ubuntu 18.04.x | s390x |
|  SUSE Linux Enterprise server (SLES) 15 SP2 | s390x |
|  IBM z/OS ({{site.data.keyword.waziaas_full_notm}}) | s390x |
{: caption="Table 1. Supported s390x stock image operating systems" caption-side="bottom"}

The {{site.data.keyword.waziaas_short}} z/OS dev and test stock image is available in all regions supported for the s390x architecture, including Japan (Tokyo), Brazil (São Paulo), Canada (Toronto), United Kingdom (London), and US East (Washington DC). For more information, see [{{site.data.keyword.waziaas_full_notm}} documentation](https://www.ibm.com/docs/en/wazi-aas/1.0.0){: external}.
{: note}

For information about images for x86 processor architecture, see [x86 virtual server images](/docs/vpc?topic=vpc-about-images).

With a cloud-init enabled image, you can provide user data. In the **User Data** field on the order form, you can enter optional cloud-init user data for the server. For more information about user data and automation, see [User data](/docs/vpc?topic=vpc-user-data).

When using the IBM Hyper Protect Container Runtime image, container details are provided at instance creation through the contract, specified in the **User Data** field on the order form. Once the containers start, you can interact with the workload that is brought up on the containers. For more information, see [Contract](/docs/vpc?topic=vpc-about-contract_se).
{: note}


You can access details about each operating system, such as the url for the operating system, by using the API call, [List all operating systems](https://cloud.ibm.com/apidocs/vpc#list-operating-systems){: external}.  
{: tip}


### Stock image naming conventions
{: #vs-image-naming-conventions}

All IBM-provided stock, public images are named by using the following convention:

```sh
ibm-<family>-<version>-<type>-<architecture>-<build>
```

The following example shows the image naming convention.

```sh
ibm-hyper-protect-container-runtime-1-0-s390x-8
```

The following list explains the variables that make up the components of the image name:
* The leading prefix of `ibm-` is used for IBM-provided images. Custom images cannot be named with this prefix.
* The `family` component provides the operating system family, such as *redhat*, *debian* or in this example *hyper-protect*.
* The `version` component provides the operating system version, such as *18-04* for Ubuntu 18.04, or *1.0* for *hyper-protect*.
* The `type` component provides the minimization level of the operating system image, such as *minimal* or *full*.  
* The `architecture` component provides the vCPU architecture that is supported by the operating system image, such as *amd64* or *s390x*.
* The `build` component is a small, non-negative integer that is incremented each time a new build of the operating system is created. For image names that are otherwise identical, the image with the highest build value is the most recent image for that operating system.

You can obtain the current list of images, including stock images, by running the following command in the command-line interface: [ibmcloud is images](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference#images).

The image naming convention is subject to change. The list of image names is not intended to be programmatically parsed or interpreted. You can use the [GET /images](/apidocs/vpc#get-image) API to obtain metadata in a structured format.
{: important}

## Custom images
{: #vs_custom-images}

You can import an image from {{site.data.keyword.cos_full_notm}} to use for creating a new virtual server instance.

To create secure execution based custom images by using the Cloud Object Storage option, see [Preparing the workload](https://www.ibm.com/docs/en/linux-on-systems?topic=tasks-prepare-workload). For information about creating secure execution based images, see [IBM Secure Execution for Linux](https://www.ibm.com/docs/en/linux-on-systems?topic=overview-introducing-secure-execution-linux).

The {{site.data.keyword.waziaas_full_notm}} (Wazi aaS) custom image can be created only by using IBM Wazi Image Builder, which is a separately orderable product from [IBM Passport Advantage](https://www.ibm.com/software/passportadvantage/){: external}. There are additional requirements to order and use Wazi Image Builder. The image cost is the premium applied to cover the cost of technologies that allows for z/OS dev and test images to run on IBM Z hardware in IBM’s cloud infrastructure as a service layer. The z/OS Wazi aaS custom image must meet the following requirements:

* Is in qcow2 format
* The operating system is z/OS 2.4 or z/OS 2.5
* The zHYPaaS host control program creates a virtual machine (VM) runtime environment for each guest operating system (OS) and can host many guests at the same time. For required program temporary fixes (PTFs) on z/OS and other IBM software products, see [Hardware and software requirements](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=builder-hardware-software-requirements){: external}.

For more information, see [Bringing your own image with Wazi Image Builder](https://www.ibm.com/docs/en/wazi-aas/1.0.0?topic=bringing-your-own-image-wazi-image-builder){: external}.

### Requirements
{: #vs_custom-image-reqs}

{{site.data.content.custom-image-requirements-list}}

{{site.data.content.custom-image-information-link}}

<!--### Storage costs
{: #custom-image-storage}

Storage costs are incurred for storing custom images. This charge is separate from charges for storing images in {{site.data.keyword.cos_full_notm}}.-->

## Next steps
{: #vs-next-steps-images}

After you choose a profile, it's time to plan for and create an instance.
* [Planning for instances](/docs/vpc?topic=vpc-vsi_best_practices)
* [Creating an instance by using the UI](/docs/vpc?topic=vpc-creating-virtual-servers)
* [Creating an instance by using the CLI](/docs/vpc?topic=vpc-creating-virtual-servers-cli)
