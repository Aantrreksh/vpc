---

copyright:
  years: 2021, 2023
lastupdated: "2023-04-03"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.filestorage_vpc_short}} profiles
{: #file-storage-profiles}

When you provision {{site.data.keyword.filestorage_vpc_short}} file shares by using the {{site.data.keyword.cloud_notm}} console, CLI, or API, you specify a file share profile. The profile defines the performance level that you require based on the file share size.
{: shortdesc}

{{site.data.keyword.filestorage_vpc_full}} is available for customers with special approval to preview this service in the Frankfurt, London, Dallas, Toronto, Washington, Sao Paulo, Sydney, Osaka, and Tokyo regions. Contact your IBM Sales representative if you are interested in getting access.
{: preview}

## File storage profiles overview
{: #file-storage-profile-overview}

When you [create a file share](/docs/vpc?topic=vpc-file-storage-create) in your availability zone, you select share size and IOPS performance based on a file storage profile. New file shares are created based on the high performance, [dp2](#dp2-profile) profile. 

Earlier file shares that were created by using either the [IOPS tier](#fs-tiers) profiles or [custom IOPS](#fs-custom) profile continue to operate based on these profiles. You can also update these file shares to use the dp2 profile or switch back to a previous profile. However, you cannot use the previous profiles when you create a new file share.

Table 1 shows the dp2 profile performance levels compared to the earlier profiles. [Table 2](#dp2-profile) presents the IOPS and capacity ranges for the dp2 profile. For more information about the earlier IOPS tiers and custom profiles, see [previous version file storage profiles](#fs-v2-profiles).

| Profile | IOPS | IOPS per share | Max throughput (MB/s)| Share size (GB) | Block size (KB) |
|---------|------|-----------------|---------------------|-----------------|-----------------|
| dp2 | 1-100 IOPS/GB | 100-96000 | 1024 | 10-32000 | 256 |
| General purpose | 3 IOPS/GB | 3000-48000 | 670 | 10-16000 GB |  16 |
| 5-iops | 5 IOPS/GB | 3000-48000 | 768 | 10-9600 | 16 |
| 10-iops | 10 IOPS/GB | 3000-48000 | 1024 | 10-4800 | 256 |
| Custom | 1-100 IOPS/GB | 3000-48000 | 1024 | 10-16800 GB | 256 |
{: caption="Table 1. Comparison of file share profiles and performance levels" caption-side="top"}

## dp2 file storage profile
{: #dp2-profile}

By using the dp2 profile, you can specify the total IOPS for the file share within the range for a specific file share size, 10 GB (default minimum) to 32,000 GB. You can provision shares with IOPS performance from 100 IOPS (default minumum) to 96,000 IOPS, based on share size. The dp2 profile is based on a block size of 256 KB. Maximum throuput is 1024 MB/s. This profile is backed by solid-state drives (SSDs).

Table 2 shows the available IOPS ranges, based on share size.

| Share size (GB) | IOPS range (IOPS) |
|------------------|-----------|
| 10-39   | 100-1000 |
| 40-79 | 100-2000 |
| 80-99 | 100-4000 |
| 100-499 | 100-6000 |
| 500-999 | 100-10000 |
| 1000-1999 | 100-20000 | 
| 2000-3999  | 200-40000 |
| 4000-7999 | 300-40000 |
| 8000-15999  | 500-64000 | 
| 16000-32000 | 2000-96000&sup1; |
{: caption="Table 2. dp2 file share profile IOPS and capacity ranges" caption-side="top"}

&sup1;For the 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share that is accessed by one instance is limited to 48,000 IOPS.

## Previous version file storage profiles
{: #fs-v2-profiles}

### IOPS tiers
{: #fs-tiers}

Existing file shares can be based on IOPS tiers that you selected when you created the file share. Table 3 describes the IOPS performance for the IOPS tier profile.

| IOPS Tier | Workload | Share size | Max IOPS |
|-----------|----------|-------------|--------------|
| 3 IOPS/GB | General-purpose workloads - Workloads that host small databases for web applications or store virtual machine disk images for a hypervisor | 10 GB to 32,000 GB | 48,000/96,000&sup1; IOPS |
| 5 IOPS/GB | High I/O intensity workloads - Workloads characterized by a large percentage of active data, such as transactional and other performance-sensitive databases| 10 GB to 9,600 GB | 48,000 IOPS|
| 10 IOPS/GB | Demanding storage workloads - Data intensive workloads created by NoSQL databases, data processing for video, machine learning, and analytics | 10 GB to 4,800 GB | 48,000 IOPS |
{: caption="Table 3. IOPS tier profiles and performance levels for each tier" caption-side="bottom"}

&sup1; For the 96,000 IOPS to be realized, a single file share must be accessed by multiple virtual server instances. A single file share that is accessed by one instance is limited to 48,000 IOPS.

The total maximum IOPS is rounded up to the next multiple of 10 when the IOPS calculation results in IOPS less than or equal to 48,000 IOPS. The total maximum IOPS is rounded up to the next multiple of 100 for IOPS calculations that result in IOPS greater than 48,000 IOPS up to 96,000 IOPS.
{: note}

### Custom IOPS profile
{: #fs-custom}

Custom IOPS profiles specify the total IOPS for the file share within the range for its size. File shares that use a custom IOPS profile can have IOPS performance level in the range 100 - 48,000 IOPS.

Table 4 shows the available IOPS ranges based on file share size.

| File Share size (GB) | IOPS range (IOPS) |
|-------------|--------------|
| 10-39   | 100-1000 |
| 40-79 | 100-2000 |
| 80-99 | 100-4000 |
| 100-499 | 100-6000 |
| 500-999 | 100-10000 |
| 1000-1999 | 100-20000 |
| 2000-3999 | 200-40000 |
| 4000-7999 | 300-40000 |
| 8000-9999 | 500-48000 |
| 10000-16000 | 1000-48000 |
{: caption="Table 4. Available IOPS based on file share size." caption-side="bottom"}

The total maximum IOPS is rounded up to the next multiple of 10 when the IOPS calculation results in IOPS less than or equal to 48,000 IOPS.
{: note}

## View profiles in the UI
{: #fs-using-ui-iops-profile}
{: ui}

When you create a file share in the console, under **Profiles**, select a profile.

## View profiles from the CLI
{: #fs-using-cli-iops-profiles}
{: cli}

To view the list of available profiles from the CLI, run the following command.

```sh
ibmcloud is share-profiles
```
{: pre}

The command returns the following profiles:

```text
ibmcloud is share-profiles
Listing file share profiles in region us-south under account VPC1 as user user@mycompany.com...
Name          Family
custom-iops   custom
dp2           defined-performance
tier-10iops   tiered
tier-3iops    tiered
tier-5iops    tiered
```
{: screen}

## View profiles with the API
{: #fs-using-api-iops-profiles}
{: api}

Use the `GET /share/profiles` request to retrieve information for all share profiles.

```curl
curl -X GET \
$vpc_api_endpoint/v1/share/profiles?$api_version&generation=2 \
-H "Authorization: $iam_token"
```
{: codeblock}

The response returns the following profiles:

```json
Name          Family
custom-iops   custom
dp2           defined-performance
tier-10iops   tiered
tier-3iops    tiered
tier-5iops    tiered
```
{: codeblock}


## How block size affects file share performance
{: #fs-profiles-block-size}

IOPS is based on a 16 KB block size for all profiles, with a 50-50 read/write random workload. Each 16 KB of data that is read or written counts as one read/write operation. A single write of less than 16 KB counts as a single write operation.

Baseline throughput is determined by the amount of IOPS multiplied by the 16 KB block size. The higher the IOPS you specify, the higher the throughput. Maximum throughput is 1536 MBps.

The block size that you choose for I/O from your application directly impacts storage performance. If the block size is smaller than the block size used by the profile to calculate the share’s bandwidth limit, the IOPS limit is reached before the throughput limit. Conversely, if the block size is larger, the throughput limit is reached before the IOPS limit.

Table 5 provides some examples of how block size and IOPS affect the throughput, calculated average I/O block size x IOPS = Throughput in MBps.

| Block Size (KB) | IOPS | Throughput (MBps) |
|-----|-----|-----|
| 4 | 1,000 | 4&sup1;|
| 8 | 1,000 | 8&sup1;|
| 16 | 1,000 | 16 |
| 32 | 500 | 16 |
| 64 | 250 | 16 |
| 128 | 128 | 16 |
| 512 | 32 | 16 |
| 1024 | 16 | 16 |
{: caption="Table 5. How block size and IOPS affect throughput." caption-side="bottom"}

&sup1;If your cap is 1000 IOPS or 16 KB block size, the throughput caps at whatever limit is reached first.

Maximum IOPS can still be obtained when you use smaller block sizes, but throughput is less. The following example shows how throughput decreases for smaller block sizes, when max IOPS is maintained.

* 16 KB * 6000 IOPS == ~94 MBps
* 8 KB * 6000 IOPS == ~47 MBps
* 4 KB * 6000 IOPS == ~23 MBps

## Next steps
{: #fs-next-steps}

[Start creating file shares](/docs/vpc?topic=vpc-file-storage-create).
