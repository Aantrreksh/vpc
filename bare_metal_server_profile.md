---

copyright:
  years: 2021, 2022
lastupdated: "2022-04-18"

keywords: 

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Bare metal server profiles
{: #bare-metal-servers-profile}

When you create a bare metal server, you can select from a profile family that best fits your needs. A profile provides a different combination of hardware configurations that include number of vCPU, amount of RAM, and size of local storage. The attributes define the size and capabilities of the bare metal server that is provisioned. 
{: shortdesc}

## About profile families
{: #profile-familiy}

Profiles are grouped by the _vCPUs:Memory_ ratio across all the VPC compute offerings. You can choose from three profile families:

| Families | vCPU:Memory ratio | Description |
|-----|-----|-----|
| Balanced | 1:4 | Best for midsize databases and common cloud applications with moderate traffic. |
| Compute | 1:2 | Best for CPU-intensive demands with moderate to high web traffic, such as production batch processing and front-end web servers. |
| Memory | 1:8 or 1:6 | Best for memory intensive workloads, such as large caching workloads, large database applications, or in-memory analytics workloads. |
{: caption="Table 1. Profile families" caption-side="bottom"}

## Profiles configurations
{: #bare-metal-servers-profile-list}

See the following table for the configurations of each profile.

| Name | vCPU | Memory (GB) | Local storage | Total network bandwidth (Gbps) | 
|---------|---------|---------|---------|---------|
| bx2-metal-96x384 | 96 | 384 | 960 GB | 100 |
| bx2d-metal-96x384  | 96 | 384  | 960 GB  \n 25.6 TB (secondary)| 100 |
| bx2-metal-192x768 | 192 | 768 | 960 GB | 100 |
| bx2d-metal-192x768 | 192 | 768 | 960 GB  \n 51.2 TB (secondary) | 100 |
{: caption="Table 2. Profile families" caption-side='top"}
{: #bare-metal-balanced-profiles}
{: tab-title="Balanced profile"}
{: tab-group="profile-configurations"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GB) | Local storage | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| cx2-metal-96x192 | 96 | 192 | 960 GB | 100 |
| cx2d-metal-96x192 | 96 | 192 | 960 GB  \n 25.6 TB (secondary) | 100 |
{: caption="Table 2. Profile families" caption-side='top"}
{: #bare-metal-compute-profiles}
{: tab-title="Compute profile"}
{: tab-group="profile-configurations"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Name | vCPU | Memory (GB) | Local storage | Total network bandwidth (Gbps) |
|---------|---------|---------|---------|---------|
| mx2-metal-96x768 | 96 | 768 | 960 GB | 100 |
| mx2d-metal-96x768 | 96 | 768 | 960 GB  \n 25.6 TB (secondary) | 100 |
{: caption="Table 2. Profile families" caption-side='top"}
{: #bare-metal-memory-profiles}
{: tab-title="Memory profile"}
{: tab-group="profile-configurations"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

### Understanding the naming rule of the profiles
{: #profile-naming-rule}

The following information describes the naming rule of the profiles.

* *b* represents a *Balanced* family profile.
 * *x* represents the *x86_64* CPU architecture .
* *2* represents this profile has the current generation of processors (Cascade Lake).
* *d* represents *NVMe U.2* SSDs.
* The field between the two dashes is "metal" for bare metal servers.
* The field after the second dash contains information on the number of vCPU and the size of memory (GB), for example, "192x768" means that this profile has 192 vCPU and a memory of 768 GiB.

Using “bx2d-metal-192x768” as an example, you can know that it's a *Balanced* bare metal profile with *192 vCPU and 768 GiB memory*. This profile has the Cascade Lake processors and NVMe U.2 SSDs for storage.

Bare metal profiles are dedicated servers that provide physical cores. vCPU measurements are used in profile naming only. vCPU to physical cores are a 2:1 ratio (e.g 192 vCPU = 96 physical cores).

## Profile availability by region
{: #bare-metal-profile-availability-by-region}

See the following table to see what bare metal profiles are available by region. 

| Profile |  us-south-1 | us-south-2 | us-south-3 | us-east-2 |
| ------- | ----------- | ---------- | ---------- | --------- |
| cx2-metal-96x192   | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| cx2d-metal-96x192  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| bx2-metal-96x384   | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| bx2d-metal-96x384  | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| mx2-metal-96x768   | ![Checkmark icon](../icons/checkmark-icon.svg) |  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |
| mx2d-metal-96x768  | ![Checkmark icon](../icons/checkmark-icon.svg) |  |  ![Checkmark icon](../icons/checkmark-icon.svg)|  |
| bx2-metal-192x768  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2d-metal-192x768 | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Table 3. Bare metal profiles availability by region" caption-side='top"}
{: #bare-metal-profiles-americas}
{: tab-title="Americas"}
{: tab-group="profile-regions"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

| Profile | eu-de-1 | eu-de-2 |
| ------- | ----------- | ---------- | 
| cx2-metal-96x192    | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| cx2d-metal-96x192   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2-metal-96x384    | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2d-metal-96x384   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx2-metal-96x768    | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| mx2d-metal-96x768   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2-metal-192x768   | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
| bx2d-metal-192x768  | ![Checkmark icon](../icons/checkmark-icon.svg) | ![Checkmark icon](../icons/checkmark-icon.svg) |
{: caption="Table 3. Bare metal profiles availability by region" caption-side='top"}
{: #bare-metal-profiles-europe}
{: tab-title="Europe"}
{: tab-group="profile-regions"}
{: class="simple-tab-table"}
{: summary="Use the buttons before the table to change the context of the table. The column headers identify the hardware class."}

## Viewing profile configurations
{: #view-bare-metal-servers-profile}

You can view available profile configurations by using the UI, [CLI](#view-bare-metal-servers-profile-cli), or the [API](#view-bare-metal-servers-profile-api).

## Using the UI to view profiles
{: #view-bare-metal-servers-profile-ui}
{: ui}

Use the following steps to view available bare metal profiles by using the UI.

1. In the [{{site.data.keyword.cloud_notm}} console ![External link icon](../icons/launch-glyph.svg "External link icon")](https://{DomainName}), go to **Menu icon ![Menu icon](../../icons/icon_hamburger.svg) > VPC Infrastructure > Compute > Bare metal servers**.
2. From the **Bare metal servers for VPC** page, click **Create**.
3. On the **New bare metal server for VPC** page, you can view and select profiles under **Profile**.

## Using the CLI to view profiles
{: #view-bare-metal-servers-profile-cli}
{: cli}

Use the following command to list all the bare metal server profiles that are available in a region:

```text
ibmcloud is bare-metal-server-profiles [--output JSON] [-q, --quiet]
```
{: pre}

## Using the API to view profiles
{: #view-bare-metal-servers-profile-api}
{: api}

List all bare metal server profiles available in a region by running the following API request:

```text
curl -X GET \
"$vpc_api_endpoint/v1/bare_metal_server/profiles?version=2021-03-09&generation=2" \
-H "Authorization: $iam_token"
```
{: pre}

## Next Steps
{: #bare-metal-servers-profile-next-step}

After you choose a profile, you can [create bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers).
