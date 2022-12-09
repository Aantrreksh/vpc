---

copyright:
  years: 2021, 2022
lastupdated: "2022-12-09"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Troubleshooting Snapshots for VPC
{: #snapshots-vpc-troubleshooting}

When you create or manage Snapshots for VPC, you might encounter issues. Often, you can recover by following a few easy steps. Issues, symptoms, likely causes, and resolutions are described in the following sections.
{: shortdesc}

## Degraded performance during the restoration of a volume from a snapshot
{: #snapshot_ts_degraded_perf}
{: troubleshoot}

When you restore a volume from a snapshot, while the data downloads, the API `health_state` and `health_reasons` properties indicate that the snapshot is being initialized. Performance is degraded while the volume is being initialized from the snapshot. This applies to snapshots of unattached volumes and snapshots of volumes that are attached to an instance.
{: tsSymptoms}

When a volume is created from a snapshot, the data for the snapshot is stored in {{site.data.keyword.cos_full}}. After the volume is created, the data is downloaded from {{site.data.keyword.cos_short}} to the availability zone where the volume is being created. Depending on the snapshot size, the download can take several minutes, at which time some of the data blocks are read or written directly from {{site.data.keyword.cos_short}}, but not all data. You might experience lower IOPS and bandwidth than what you'd expect when you create a regular volume. The health state show as `degraded` with the health reason being `initializing_from_snapshot`.
{: tsCauses}

Wait until the data has been fully downloaded from {{site.data.keyword.cos_short}} to the volume. When the data is fully downloaded, you can expect the normal provisioned IOPS and bandwidth for the volume.
{: tsResolve}

## Restoring a volume from a snapshot fails
{: #snapshot_ts_restore_fail}
{: troubleshoot}

A fully restored volume was not created from a snapshot. The API health status shows `degraded`, with the health reason `initializing_from_snapshot` and a message indicates that initializing from the snapshot failed.
{: tsSymptoms}

Some or all of the data was not downloaded from {{site.data.keyword.cos_short}} to the volume being created. Data blocks were not written.
{: tsCauses}

Contact IBM support to determine the root cause of the failure and resolve the problem.
{: tsResolve}

## A snapshot of a volume is in an unusable state during volume restoration
{: #snapshot_ts_unusable}
{: troubleshoot}

The snapshot state is in an unusable state and the volume can't be restored from the snapshot. In this case, the health state is faulted. You can see the API health reason `initializing_from_snapshot` and with a message that the snapshot is in an unusable state.
{: tsSymptoms}

The data was not downloaded from {{site.data.keyword.cos_short}} when you tried to restore a volume because the snapshot is not available. A possible cause is that the customer root key for the snapshot was disabled or deleted.
{: tsCauses}

You can [enable a disabled root key](/docs/key-protect?topic=key-protect-disable-keys&interface=ui#enable-ui) or a [restore deleted root key](/docs/vpc?topic=vpc-vpc-encryption-managing&interface=ui#byok-restore-root-key) within 30 days of deletion. Restoring the root key makes the snapshot available. For other issues, contact IBM support to determine the root cause of the failure and fix the problem.
{: tsResolve}
