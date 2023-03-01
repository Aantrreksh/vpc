---

copyright:
  years: 2022, 2023
lastupdated: "2023-02-07"

keywords: confidential computing, secure execution, security claims, disks encryption

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Verifying disk encryption status
{: #hpvs-disks-encryption-validate}

Both the root disk and data disks in the {{site.data.keyword.hpvs}} for VPC instance are encrypted with Linux Unified Key Setup (LUKS) Encryption. You can verify the encryption status by checking the messages in the log.

- The root disk is re-created and encrypted on every boot with the original content. A passphrase for the root disk is not stored.

- The data disk encryption is configured by using the “seed” provided in the `workload` and `env` sections within the contract. During the instance initiation, the disks are attached and encrypted by using the seed to create a LUKS passphrase. If the seed information or the data disk is not configured, the instance fails to initiate. For more information, see [The `workload` - `volumes` subsection](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_volumes) of the contract.

The disk encryption status check daemon checks the crypto headers of the root disk and data disks that are attached to the instance each hour, and then writes the information messages about the disk encryption status into the log.

The following example shows the disk encryption check related messages within the log.

```text
...
Nov 29 10:24:07 hpvs211vsi verify-disk-encryption info HPL13000I: Verify LUKS Encryption...
Nov 29 10:24:07 hpvs211vsi systemd info verify-disk-encryption-invoker.service: Succeeded.
...
Nov 29 10:24:08 hpvs211vsi verify-disk-encryption info Checked for mount point /mnt/data, LUKS encryption with 1 key slot found
Nov 29 10:24:08 hpvs211vsi verify-disk-encryption info HPL13001I: Root disk and all the data disks are encrypted
...
```
{: codeblock}

You can't configure the interval of the disk encryption status checks.
{: note}

To address Denial-of-Service attacks, requests are throttled at three per five minutes.
{: note}
