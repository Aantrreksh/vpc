---

copyright:
  years: 2019, 2023
lastupdated: "2023-06-06"

keywords: api, change log, new features, restrictions, migration

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# VPC API change log
{: #api-change-log}

Read the VPC API change log to learn about updates and improvements to the {{site.data.keyword.vpc_full}} (VPC) [API](/apidocs/vpc). The change log lists changes that are ordered by the date they were released. Changes to existing API versions are designed to be compatible with existing client applications.
{: shortdesc}

By design, new features with backward-incompatible changes apply only to version dates on and after the feature's release. Changes that apply to older versions of the API are designed to maintain compatibility with existing applications and code. If backward-incompatible changes require non-trivial client code changes to use an API version, the API change log might provide links to instructions, tips, or best practices for updating client code.
{: note}

Some changes, such as new response properties or new optional request parameters, are considered backward compatible. Other changes, such as new required request parameters, are not considered backward compatible. To avoid disruption from changes to the API, use the following best practices when you call the API:

- Catch and log any `4xx` or `5xx` HTTP status code, along with the included `trace` property
- Follow HTTP redirect rules for any `3xx` HTTP status code
- Consume only the resources and properties your application needs to function
- Avoid depending on behavior that is not explicitly documented

SDK changes are based on API changes. For information about the latest changes to the VPC SDKs, see the change logs in the SDK repositories:

- [Java SDK change log](https://github.com/IBM/vpc-java-sdk/blob/master/CHANGELOG.md)
- [Node SDK change log](https://github.com/IBM/vpc-node-sdk/blob/master/CHANGELOG.md)
- [Python SDK change log](https://github.com/IBM/vpc-python-sdk/blob/master/CHANGELOG.md)
- [Go SDK change log](https://github.com/IBM/vpc-go-sdk/blob/master/CHANGELOG.md)

## Upcoming changes
{: #upcoming-changes}

**`InstanceTemplate` response schema change.** In an upcoming release, future methods of creating instances, and therefore creating instance templates, may not require a primary network interface. To accommodate this, the `primary_network_interface` property is now optional in the instance template response model.

At this time, all instances, and therefore all instance templates, continue to require that a primary network interface be specified. Therefore, existing instance templates are unaffected. Additionally, new instance templates will continue to include a primary network interface until further notice. However, to ensure your clients will not be affected in the future, verify that they are tolerant of the `primary_network_interface` property not being included when consuming `InstanceTemplate` responses. 
{: important}

**`Instance` response schema change.** In an upcoming release, volume attachments returned in the `boot_volume_attachment` and `volume_attachments[]` properties of an instance will not include the `volume` sub-property if the volume has not yet been provisioned. Such volumes are currently represented with empty `crn`, `id`, and `href` properties along with an undocumented sentinel value for `name`.

To prepare for this change, verify that your client checks that the `volume` property exists for a volume attachment before attempting to access its `crn`, `id`, `href`, or `name` sub-properties.

**Asynchronous `DELETE` response code change.** In an upcoming release, the response code output for asynchronous `DELETE` operations will change from `204` to `202`. A response code of `204` implies the action is completed, which could be misleading for operations that are still processing. A response code of `202` is more appropriate. This behavior change will occur only for an API version date after its release. A response code of `204` will continue to be returned for API versions up to this version date.

The new response code will be rolled out gradually. Each phase of the rollout will be tied to a dated API version. These changes will be announced in future change log updates.
{: note}

**Security group targets.** In an upcoming release, new resource types will be permitted as security group targets. If you add resources of these new types to a security group, existing client applications will be exposed to the new types when iterating over the security group's targets. To avoid disruption, check that client applications are written to gracefully handle unexpected resource types in a security group's targets.

**VPN site-to-site gateway cipher upgrade.** In an upcoming release, connections with auto IKE or IPsec policies that are out of conformance will be automatically upgraded to the [enhanced auto-negotiation policy](/docs/vpc?topic=vpc-using-vpn#policy-negotiation). Connections with customized IKE or IPsec policies that contain weak ciphers will be disabled. For more information, see [Upgrading a VPN from a custom IKE or IPsec policy](/docs/vpc?topic=vpc-upgrading-weak-ciphers&interface=api#upgrade-vpn-with-custom-policy).

## 23 May 2023
{: #23-may-2023}

### For all version dates
{: #23-may-2023-all-version-dates}

**Removal of weak VPN for VPC ciphers.** Effective 18 May 2023, the following VPN IKE and IPsec ciphers are removed:

- Authentication algorithms `md5` and `sha1`
- Encryption algorithm `triple_des`
- Diffie–Hellman groups `2` and `5`

As a result, you will no longer be able to create an IKE/IPsec policy or VPN connection that includes a weak cipher, but you can still [upgrade weak cipher suites](/docs/vpc?topic=vpc-upgrading-weak-ciphers&interface=ui) on an existing policy or connection. For more information, see [Upgrading weak cipher suites on a VPN gateway](/docs/vpc?topic=vpc-upgrading-weak-ciphers&interface=api).

## 2 May 2023
{: #2-may-2023}

### For all version dates
{: #2-may-2023-all-version-dates}

**Exporting custom images.** You can now [export custom images](/apidocs/vpc-beta#create-image-export-job) to an authorized IBM Cloud Object Storage bucket. Specify the target `storage_bucket` to export the image to. The image will be exported as `qcow2` unless you specify another value using the `format` property. For more information, see [Exporting a custom image to IBM Cloud Object Storage](/docs/vpc?topic=vpc-managing-custom-images&interface=api#custom-image-export-to-cos-api), or start using the new [export jobs](/apidocs/vpc-beta#list-image-export-jobs) methods.

## 18 April 2023
{: #18-april-2023}

### For all version dates
{: #18-april-2023-all-version-dates}

**Resource suspension for bare metal servers.** The [list all bare metal servers](/apidocs/vpc/latest#list-bare-metal-servers) and [retrieve a bare metal server](/apidocs/vpc/latest#get-bare-metal-server) methods now provide `lifecycle_reasons` and `lifecycle_state` properties. A bare metal server that violates IBM Cloud’s [Acceptable Use Policy](https://www.ibm.com/services/us/imc/html/aup1.html){: external} will now have its `lifecycle_state` property set to `suspended`. A suspended bare metal server is automatically powered off and you cannot update, delete, or power it on. For more information, see [Viewing bare metal status and lifecycle_state in the API](/docs/vpc?topic=vpc-managing-bare-metal-servers&interface=api) and [Resource suspension](/docs/vpc?topic=vpc-resource-suspension).

## 11 April 2023
{: #11-april-2023}

### For all version dates
{: #11-april-2023-all-version-dates}

**Console type configuration for bare metal server profiles.** When you [retrieve a bare metal server profile](/apidocs/vpc/latest#get-bare-metal-server-profile) or [list all bare metal server profiles](/apidocs/vpc/latest#list-bare-metal-server-profiles), the response now provides a `console_types` property that denotes the console type configuration for a bare metal server with this profile.

**Network interface configuration for bare metal server profiles.** When you [retrieve a bare metal server profile](/apidocs/vpc/latest#get-bare-metal-server-profile) or [list all bare metal server profiles](/apidocs/vpc/latest#list-bare-metal-server-profiles), the response now provides a `network_interface_count` property. When the `type` is `range`, the new property provides `max` and `min` sub-properties that denote the maximum and minimum number of network interfaces that are supported for a bare metal server with the specified profile. The values for `max` and `min` include both the primary network interface and secondary network interfaces. When the `type` is `dependent`, the network interface count depends on another value that is specified when the server is created. For more information about network interfaces, see [Overview of bare metal server network interfaces](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers#overview-bare-metal-network-interfaces), and [Managing network interfaces for bare metal servers on VPC](/docs/vpc?topic=vpc-managing-nic-for-bare-metal-servers&interface).

## 28 March 2023
{: #28-march-2023}

### For all version dates
{: #28-march-2023-all-version-dates}

**VCPU manufacturer support for instances and dedicated hosts.** When [provisioning](/apidocs/vpc/latest#create-instance) an instance or dedicated host, you can now use the new `vcpu_manufacturer` property in the [instance](/apidocs/vpc/latest#list-instance-profiles) or [dedicated host](/apidocs/vpc/latest#list-dedicated-host-profiles) profile to choose between profiles from different processor manufacturers. You can also view the virtual server instance VCPU configuration through the `vcpu` sub-property `manufacturer`. For more information and limitations, see [x86-64 instance profiles](/docs/vpc?topic=vpc-profiles&interface=ui#balanced) and [Dedicated host profiles](/docs/vpc?topic=vpc-dh-profiles&interface=ui#balanced-dh-pr).

**Network interface configuration for instance profiles.** When you [retrieve an instance profile](/apidocs/vpc/latest#get-instance-profile) or [list all instance profiles](/apidocs/vpc/latest#list-instance-profiles), the response now provides a `network_interface_count` property. When the `type` is `range`, the new property provides `max` and `min` sub-properties that denote the maximum and minimum number of network interfaces that are supported for a virtual server instance with the specified profile. The values for `max` and `min` include both the primary network interface and secondary network interfaces. When the `type` is `dependent`, the network interface count depends on another value that is specified when the instance is created. For more information about instance profiles and network interface count, see [Bandwidth allocation with multiple network interfaces](/docs/vpc?topic=vpc-profiles&interface=api#bandwidth-multi-vnic). 

**Private DNS integration for load balancers.** When you [create](/apidocs/vpc/latest#create-load-balancer) or [update](/apidocs/vpc/latest#update-load-balancer) a load balancer, you can now bind the IP addresses of your VPC load balancers to your private DNS zone by specifying the new `dns.instance` and `dns.zone` properties. When you specify these properties, load balancer IPs will no longer be registered to the publicly resolvable `lb.appdomain.cloud` domain name. For more information, see [IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-nlb-dns&interface=api) and [IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-lb-dns&interface=api).

## 21 March 2023
{: #21-march-2023}

### For all version dates
{: #21-march-2023-all-version-dates}

**Instance provision by volume.**  You can now reuse an existing boot volume to provision a virtual server instance by specifying the existing volume using the `id` or `crn` sub-properties of the `boot_volume_attachment` property. The specified volume must be unattached and must have an operating system with the same architecture as the instance profile. You can use the new volume `attachment_state` property and expanded `operating_system` property to determine its eligibility. You can also use the new [list volumes](/apidocs/vpc/latest#list-volumes) filters to list volumes that have specific `attachment_state`, `operating_system`, and `encryption_type` values.

By default, a boot volume created as part of provisioning a virtual server instance will be deleted when the instance is deleted. You can control this by specifying the `delete_volume_on_instance_delete` property when [creating the instance](/apidocs/vpc/latest#create-instance) or updating the [boot volume attachment](/apidocs/vpc/latest#update-instance-volume-attachment). For more information, see [Creating virtual server instances](/docs/vpc?topic=vpc-creating-virtual-servers&interface=api).

**VPC route priority.** You can now control the priority of [VPC routes](/docs/vpc?topic=vpc-about-custom-routes). When you [create](/apidocs/vpc/latest#create-vpc-routing-table-route) or [update](/apidocs/vpc/latest#update-vpc-routing-table-route) a VPC route, use the new `priority` property to specify a value between `0` and `4` (default: `2`). Smaller values have higher priority. For more information, see [Determining route preference](/docs/vpc?topic=vpc-about-custom-routes#cr-determining-route-preference).

**Modifiable next hop for VPC routes.** You can now [update](/apidocs/vpc/latest#update-vpc-routing-table-route) the `next_hop` property of a VPC route. For more information about next hop, see [Creating a route](/docs/vpc?topic=vpc-create-vpc-route).

## 7 March 2023
{: #7-march-2023}

### For all version dates
{: #7-march-2023-all-version-dates}

**Idle connection timeout control for application load balancers.** You can now control the maximum time a client can be inactive when connected to the server by specifying the `idle_connection_timeout` property when [creating a load balancer](/apidocs/vpc/latest#create-load-balancer), [creating a load balancer listener](/apidocs/vpc/latest#create-load-balancer-listener), and [updating a load balancer listener](/apidocs/vpc/latest#update-load-balancer-listener). The `idle_connection_timeout` value defaults to the minimum of 50 seconds, and has a maximum of 2 hours, specified in seconds. For more information, see [Creating an application load balancer](/docs/vpc?topic=vpc-load-balancers&interface=ui).

## 14 February 2023
{: #14-february-2023}

### For all version dates
{: #14-february-2023-all-version-dates}

**VPC instance metadata new endpoint URL.** You can now use the fully qualified domain name (FQDN) `api.metadata.cloud.ibm.com` for the metadata service endpoint. The FQDN resolves to the link-local IP address `169.254.169.254` without requiring the application of special configurations. For more information, see [Endpoint URLs](/apidocs/vpc-metadata#endpoint-url) in the VPC Instance Metadata API.

**VPC instance metadata communication protocol and hop limit.** You can now control the communication protocol and hop limit for IP response packets used by the [VPC Instance Metadata service](/docs/vpc?topic=vpc-imd-about). When you [create](/apidocs/vpc/latest#create-instance) or [update](/apidocs/vpc/latest#update-instance) an instance, use the new `metadata_service.protocol` property to specify either `http` (default) or `https` (secure access) communication. In addition, use the new `metadata_service.response_hop_limit` property to specify a value between `1` (default) and `64`. Both of these properties apply only when the metadata service is enabled by setting `metadata_service.enabled` to `true`. The default is `false`. For more information, see [Configure metadata settings on an existing instance with the API](/docs/vpc?topic=vpc-imd-configure-service&interface=api#metadata-config-api).

## 7 February 2023
{: #7-february-2023}

### For all version dates
{: #7-february-2023-all-version-dates}

**Snapshot clones for fast restore.** You can now quickly restore a volume from a snapshot by using a fast restore snapshot clone. You can create a fast restore clone when you [create](/apidocs/vpc/latest#create-snapshot) a new snapshot or [update](/apidocs/vpc/latest#create-snapshot-clone) an existing snapshot by adding one or more zonal clones for a snapshot in the same region as the snapshot. Later, you can restore a volume, and all of its data, from that fast restore snapshot clone. When you no longer need a zonal snapshot clone, you can [delete](/apidocs/vpc/latest#delete-snapshot-clone) it. Although the delete operation cannot be reversed, you can create an new, equivalent zonal clone from the snapshot.

When [creating](/apidocs/vpc/latest#create-backup-policy-plan) or [updating](/apidocs/vpc/latest#update-backup-policy-plan) a backup policy plan, you can now specify the `clone_policy.zones` in which backup service will create snapshot clones.

For more information, see [Restoring a volume using fast restore](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-use-fast-restore) or dive into the new [API methods](/apidocs/vpc/latest#list-snapshot-clones).

## 31 January 2023
{: #31-january-2023}

### For all version dates
{: #31-january-2023-all-version-dates}

**Bare metal server secure boot.** When you [create](/apidocs/vpc/latest#create-bare-metal-server) or [update](/apidocs/vpc/latest#update-bare-metal-server) a bare metal server, you can now enable secure boot. The default is `false` (disabled). If enabled, the image must support secure boot or the server will fail to boot. To toggle secure boot, the server must be `stopped`. For more information, see [Bare metal server images](/docs/vpc?topic=vpc-bare-metal-image).

**Bare metal server trusted platform module (TPM) support.** When you [create](/apidocs/vpc/latest#create-bare-metal-server) or [update](/apidocs/vpc/latest#update-bare-metal-server) a bare metal server, you can now set a TPM mode. Specify a `mode` value (`disabled` or `tpm_2`) in the `trusted_platform_module` property. The default is `disabled`. To change the TPM mode, the server must be `stopped`. To determine the supported TPM modes, use the `supported_trusted_platform_modes` property included in the [bare metal server profile](/apidocs/vpc/latest#list-bare-metal-server-profiles). For more information, see [Secure boot with Trusted Platform Module (TPM)](/docs/vpc?topic=vpc-secure-boot-tpm).

## 17 January 2023
{: #17-january-2023}

### For all version dates
{: #17-january-2023-all-version-dates}

**Bare metal server DELETE response code change.** Bare metal `DELETE` methods now return an HTTP response code of `202` upon success:

- [Delete a network interface](/apidocs/vpc/latest#delete-bare-metal-server-network-interface)
- [Disassociate a floating IP from a network interface](/apidocs/vpc/latest#remove-bare-metal-server-network-interface-floatin)
- [Delete a bare metal server](/apidocs/vpc/latest#delete-bare-metal-server)

Unlike previous response code changes, the transition from `204` to `202` applies to all API versions.  Therefore, a response code of `204` will not be returned for any API requests for these methods, regardless of the `version` query parameter value. Future transitions from `204` to `202` will be tied to a dated API version, as described in [Upcoming changes](#upcoming-changes).
{: note}

## 20 December 2022
{: #20-december-2022}

### For all version dates
{: #20-december-2022-all-version-dates}

**Backup policy jobs.** You can now [list all jobs for a backup policy](/apidocs/vpc/latest#list-backup-policy-jobs) or [retrieve a backup policy job](/apidocs/vpc/latest#get-backup-policy-job). A backup policy job is triggered when a scheduled backup snapshot is being created or deleted. If the create or delete action is successful, the job contains information about the backup snapshot that was created or deleted. If the job ran unsuccessfully, the job contains the reason for the failure. For more information, see [Viewing backup jobs](/docs/vpc?topic=vpc-backup-view-policy-jobs).

## 13 December 2022
{: #13-december-2022}

### For all version dates
{: #13-december-2022-all-version-dates}

**Health states for block storage volumes.** When you [list all volumes](/apidocs/vpc/latest#list-volumes) or [retrieve volume details](/apidocs/vpc/latest#get-volume), the responses now include `health_state` and  `health_reasons` properties. For more information, see [Block storage volume health states](/docs/vpc?topic=vpc-managing-block-storage&interface=api#block-storage-vpc-health-states).

## 15 November 2022
{: #15-november-2022}

### For all version dates
{: #15-november-2022-all-version-dates}

**Access management tag support.** As described in [Authorization](/apidocs/vpc/latest#api-authorization) in the VPC API reference, you can now use [IBM Cloud Identity and Access Management](/docs/account?topic=account-iamoverview) to control access to VPC resources by using access management tags. For details, see [Managing IAM access for VPC Infrastructure Services](/docs/vpc?topic=vpc-iam-getting-started&interface=api#iam-access-management-tags).

Some VPC APIs currently require additional authorizations beyond those defined in the API specification. For more information, see [Known issues](/docs/vpc?topic=vpc-known-issues#api-spec-auth-known-issue).
{: important}

## 25 October 2022
{: #25-october-2022}

### For all version dates
{: #25-october-2022-all-version-dates}

**VPC route naming restriction.** You can no longer create VPC routes that begin with the prefix `ibm-` or rename an existing route to have the prefix `ibm-`. Existing routes that begin with `ibm-` will not be affected. If you have automation that creates routes using the prefix `ibm-`, you will need to remove or change the prefix used by your automation for it to succeed.

## 11 October 2022
{: #11-october-2022}

### For all version dates
{: #11-october-2022-all-version-dates}

**Public internet ingress routing.** You can now route public internet ingress traffic destined to a floating IP to a next-hop IP. When you [create a new VPC routing table](/apidocs/vpc/latest#create-vpc-routing-table) or [update an existing VPC routing table](/apidocs/vpc/latest#update-vpc-routing-table), the new `route_internet_ingress` property lets you route traffic that originates from the public internet. For more information, see [Creating a routing table by using the API](/docs/vpc?topic=vpc-create-vpc-routing-table&interface=api) and limitations and guidelines for [Ingress routes](/docs/vpc?topic=vpc-about-custom-routes&interface=ui#routes-ingress).

## 4 October 2022
{: #4-october-2022}

### For all version dates
{: #4-october-2022-all-version-dates}

**Enhanced network interface support for flow logs.** Flow logs are now collected for all network interfaces attached to a subnet, even if those network interfaces are in another account. As a result, flow logs for network interfaces associated with IBM Cloud Kubernetes Service (IKS)/Red Hat OpenShift Kubernetes Service (ROKS) worker nodes, load balancers, and VPN gateways are now collected. For example, if you have an existing [flow log collector](/apidocs/vpc/latest#get-flow-log-collector) that targets a VPC or subnet that also has attached IKS worker nodes, it will now collect flow logs for traffic flowing through those IKS worker nodes in those VPCs and subnets. For more information, see [Flow log limitations](/docs/vpc?topic=vpc-limitations-flow-logs).

## 27 September 2022
{: #27-september-2022}

### For all version dates
{: #27-september-2022-all-version-dates}

**Sharing images across accounts within an enterprise.** You can now use a [catalog to share custom images](/docs/vpc?topic=vpc-planning-custom-images#custom-image-cloud-private-catalog) with users in other accounts within the same enterprise. When you [create an image](/apidocs/vpc/latest#create-image), a new `catalog_offering` property includes a `managed` sub-property that is set to `false` by default.  When the custom image is imported to a catalog the `managed` sub-property is set to `true`, indicating that the image is added to a catalog offering `version` and is managed from a catalog. Any user who has been authorized to the catalog offering version can [provision a virtual server instance](/apidocs/vpc/latest#create-instance) with that image by specifying the offering version's CRN as `catalog_offering.version.crn`. Alternatively, users can specify the offering's CRN as `catalog_offering.offering.crn` to provision a virtual server instance with the latest image associated with the catalog offering. For more information, see [Custom images in a private catalog](/docs/vpc?topic=vpc-planning-custom-images&interface=api#private-catalog-image-reference-vpc-api) in Getting started with custom images, the tutorial [Onboarding a virtual server image for VPC](/docs/account?topic=account-catalog-vsivpc-tutorial), and the [Import offering](/apidocs/resource-catalog/private-catalog#import-offering){: external} method in the Catalog Management API.

The image may not be deleted or used in a different catalog product offering version while it is managed from a catalog. If the catalog is deleted, a 7 day reclamation period will apply that prevents any images managed by the catalog from being deleted or re-used during the reclamation period. For more information, see [Deleting a custom image in a private catalog](/docs/vpc?topic=vpc-planning-custom-images&interface=api#deleting-private-catalog-custom-image-vpc) and [Using resource reclamations](/docs/account?topic=account-resource-reclamation&interface=api#restore-resource-cli).

Image references may refer to custom images in other accounts. Before using this feature, verify that your clients handle image reference lookup failures gracefully and do not assume inaccessible images have been deleted, even when running with full access to your images.  To avoid possible retrieval or use of the wrong image by `name`, specify the image `id`, `crn`, or `href` instead. See [Using cross-account image references in a private catalog in the API](/docs/vpc?topic=vpc-planning-custom-images&interface=api#private-catalog-image-reference-vpc-api) for more information.
{: important}

**Increased network interface limits for virtual server instances.** You can now have up to 14 secondary network interfaces on a virtual server instance. The previous limit for secondary network interfaces was 4. The number of interfaces that a virtual server instance supports is dependent on the VCPU count that is included in the [instance profile](/docs/vpc?topic=vpc-profiles). For more information about the number of interfaces that a virtual server supports, see [Bandwidth allocation with multiple network interfaces](/docs/vpc?topic=vpc-profiles&interface=ui#bandwidth-multi-vnic). To utilize the increased limit for network interfaces, you can create secondary network interfaces by specifying `network_interfaces` when you [create an instance](/apidocs/vpc/latest#create-instance). You also can add secondary network interfaces to an existing instance by [creating a network interface on an instance](/apidocs/vpc/latest#create-instance-network-interface).

For an existing, running instance with 17 or more vCPUs to take advantage of the new network interface limits, it must be stopped and then started again. A reboot action on the running virtual server does not activate the increased network interface limit.
{: note}

**IBM&reg; LinuxONE Bare Metal Servers.** Accounts with access to the profiles for s390x bare metal servers can now [create](/apidocs/vpc/latest#create-bare-metal-server) LinuxONE Bare Metal Servers. These profiles have a `cpu_architecture` of `s390x` and must be used with Red Hat Enterprise Linux for s390x and SUSE Linux Enterprise Server (SLES) for s390x. For more information, see [Creating bare metal servers on VPC](/docs/vpc?topic=vpc-creating-bare-metal-servers&interface=ui) and [s390x bare metal server images](/docs/vpc?topic=vpc-s390x-bare-metal-images).

In support of s390x bare metal servers, the following enumerations have been expanded:

- Because s390x local disks are attached through Fiber Channel protocol, a value of `fcp` has been added to the `interface_type` enumeration that is returned when you [retrieve](/apidocs/vpc/latest#get-bare-metal-server-disk) or [list](/apidocs/vpc/latest#list-bare-metal-server-disks) the disks for a bare metal server. For more information, see [Storage overview of s390x bare metal servers](/docs/vpc?topic=vpc-s390x-bare-metal-servers-storage).

- Because s390x provides TCP/IP connectivity by using HiperSockets, a value of `hipersocket` has been added to the `interface_type` enumeration returned when you [retrieve](/apidocs/vpc/latest#get-bare-metal-server-network-interface) or [list](/apidocs/vpc/latest#list-bare-metal-server-network-interfaces) the network interfaces on an s390x bare metal server. Similarly, when you [create](/apidocs/vpc/latest#create-bare-metal-server) an s390x bare metal server, or [add](/apidocs/vpc/latest#create-bare-metal-server-network-interface) a network interface to an existing s390x bare metal server, you must specify an `interface_type` of `hipersocket`. For more information, see the [_IBM HiperSockets Implementation Guide_](https://www.redbooks.ibm.com/abstracts/sg246816.html){: external} and [Network overview of s390x bare metal servers](/docs/vpc?topic=vpc-s390x-bare-metal-servers-network).

s390x bare metal servers have different network bandwidth and maximum network interface limits from x86 bare metal servers. For more information, see [s390x bare metal server profiles](/docs/vpc?topic=vpc-s390x-bare-metal-servers-profile).

## 20 September 2022
{: #20-september-2022}

### For all version dates
{: #20-september-2022-all-version-dates}

**Deprecated VPN for VPC ciphers.** The following VPN IKE and IPsec ciphers are now deprecated:

- Authentication algorithms `md5` and `sha1`
- Encryption algorithm `triple_des`
- Diffie–Hellman groups `2` and `5`

You have until 13 December 2022 to upgrade to more secure ciphers. After this date, VPN connections using deprecated ciphers will have a `status` of `down` (and no longer transfer data) until you upgrade from the weak cipher. For more information, see [Upgrading weak cipher suites on a VPN gateway](/docs/vpc?topic=vpc-upgrading-weak-ciphers).

**Additional VPN for VPC ciphers.** VPN gateways now provide new algorithms to help meet your security and compliance requirements.

[IKE policy](/apidocs/vpc/latest#list-ike-policies) methods now support the `sha384` value for the `authentication_algorithm` property, `aes192` value for the `encryption_algorithm` property, and groups 15-18, 20-24, and 31 for the `dh_group` property.

[IPsec policy](/apidocs/vpc/latest#list-ipsec-policies) methods now support `sha384` and `disabled` values for the `authentication_algorithm` property, `aes192`, `aes128gcm16`, `aes192gcm16`, and `aes256gcm16` values for the `encryption_algorithm` property, and groups 15-18, 20-24, and 31 for the `dh_group` property.

Specifying IKE and IPsec policies when configuring a VPN connection is optional. If a policy is not specified, one is chosen through _auto-negotiation_. For more information, see [About policy negotiation](/docs/vpc?topic=vpc-using-vpn#policy-negotiation).
{: note}

## 13 September 2022
{: #13-september-2022}

### For all version dates
{: #13-september-2022-all-version-dates}

**Updating subnets for application load balancers.** You can now update the subnets attached to an application load balancer by specifying `subnets` when [updating a load balancer](/apidocs/vpc/latest#update-load-balancer). The specified subnets must be in the same VPC as the load balancer's current subnets. If the update requires moving your load balancer to a different zone, its `provisioning_status` will change to `migrate_pending` until the move is complete. For more information, see [Updating subnets for Application Load Balancers for VPC](/docs/vpc?topic=vpc-alb-updating-subnets&interface=api).

Verify that any clients retrieving the `provisioning_status` property will gracefully handle unknown values. For example, a client might bypass the load balancer, log a message, or halt processing and surface an error.

Because the `subnets` property is an array, the specified value will replace the load balancer's existing array of subnets. To guard against concurrent updates, you must provide the resource's current ETag using the `If-Match` header. For guidance on the use of ETags, see [Concurrent update protection](/apidocs/vpc/latest#concurrent-update-protection).
{: important}

### For version `2022-09-13` or later
{: #version-2022-09-13}

**Load balancer DELETE response code change.** For requests using a `version` query parameter of `2022-09-13` or later, all load balancer `DELETE` methods will return an HTTP response code of `202` upon success:

- [Delete a load balancer](/apidocs/vpc/latest#delete-load-balancer)
- [Delete a load balancer listener](/apidocs/vpc/latest#delete-load-balancer-listener)
- [Delete a load balancer listener policy](/apidocs/vpc/latest#delete-load-balancer-listener-policy)
- [Delete a load balancer listener policy rule](/apidocs/vpc/latest#delete-load-balancer-listener-policy-rule)
- [Delete a load balancer pool](/apidocs/vpc/latest#delete-load-balancer-pool)
- [Delete a load balancer pool member](/apidocs/vpc/latest#delete-load-balancer-pool-member)

The underlying deletion operations were already asynchronous, and remain unchanged.

To avoid regressions, before issuing requests using a `version` query parameter of `2022-09-13` or later, ensure that any clients deleting load balancer resources will also regard a response code of `202` as success.
{: important}

A response code of `204` will continue to be returned for API requests using a `version` query parameter of `2022-09-12` and earlier.

## 6 September 2022
{: #6-september-2022}

### For all version dates
{: #6-september-2022-all-version-dates}

**Improved reserved IP support for bare metal servers**. The following methods have been added for convenience and parity with the virtual server instance reserved IP methods:

- [List all reserved IPs bound to a network interface](/apidocs/vpc/latest#list-instance-network-interface-ips) for a bare metal server
- [Retrieve bound reserved IP](/apidocs/vpc/latest#get-instance-network-interface-ip) for a bare metal server

## 23 August 2022
{: #23-august-2022}

### For all version dates
{: #23-august-2022-all-version-dates}

**Additional user tag support for boot and data volumes.** You can now add user tags to boot and data volumes when provisioning a virtual server instance or adding a volume attachment. You can specify the `user_tags` property when you [create an instance](/apidocs/vpc/latest#create-instance), [create an instance template](/apidocs/vpc/latest#create-instance-template), and [create a volume attachment](/apidocs/vpc/latest#create-instance-volume-attachment). For more information, see [Create and attach a block storage volume when you create a new instance](/docs/vpc?topic=vpc-creating-block-storage&interface=api) and [Working with tags](/docs/account?topic=account-tag&interface=api).

## 16 August 2022
{: #16-august-2022}

### For all version dates
{: #16-august-all-version-dates}

**Improved VLAN support for bare metal servers.** The restriction limiting a VLAN ID in the `allowed_vlans` property to a single PCI [network interface on a bare metal server](/apidocs/vpc/latest#get-bare-metal-server-network-interface) has been removed. As a result, you can now move VLAN interfaces between PCI interfaces on the same bare metal server.

## 26 July 2022
{: #26-july-2022}

### For all version dates
{: #26-july-2022-all-version-dates}

**Resource suspension for virtual server instances.** The [list all instances](/apidocs/vpc/latest#list-instances) and [retrieve an instance](/apidocs/vpc/latest#get-instance) methods now provide `lifecycle_reasons` and `lifecycle_state` properties. A virtual server instance that violates IBM Cloud’s [Acceptable Use Policy](https://www.ibm.com/services/us/imc/html/aup1.html){: external} will now have its `lifecycle_state` property set to `suspended`. A suspended instance is automatically powered off and you cannot update, delete, or power it on. For more information, see [Viewing instance status and lifecycle_state in the API](/docs/vpc?topic=vpc-managing-virtual-server-instances&interface=api#instance-status-api) and [Resource suspension](/docs/vpc?topic=vpc-resource-suspension).

## 5 July 2022
{: #5-july-2022}

### For all version dates
{: #5-july-2022-all-version-dates}

**Client VPN for VPC.** Client-to-site connectivity is now available. This feature allows remote devices to securely connect to a VPC using an OpenVPN (or other compatible) software client. For more information about VPN client-to-site connectivity and how it complements the existing VPN site-to-site connectivity, see [About client-to-site VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-overview).

- A **VPN server** allows VPN clients from the internet to connect to a VPC. When [creating a VPN server](/apidocs/vpc/latest#create-vpn-server), you can specify a security group to protect the VPN server, and subnets in your VPC in which the VPN server will allocate its [reserved IP addresses](/apidocs/vpc/latest#list-subnet-reserved-ips). For more information, see [Creating a VPN server](/docs/vpc?topic=vpc-vpn-create-server) and the new [VPN server methods](/apidocs/vpc/latest#list-vpn-servers).
- A **VPN client** represents a client connecting from the internet. You can [retrieve the OpenVPN client configuration](/apidocs/vpc/latest#get-vpn-server-client-configuration) to use to configure a VPN client for a VPN server. For more information, see [Setting up a client VPN environment and connecting to a VPN server](/docs/vpc?topic=vpc-vpn-client-environment-setup) and the new [VPN client methods](/apidocs/vpc/latest#list-vpn-server-clients).
- A **VPN route** controls which subnets the VPN client can access and how the traffic from the VPN client reaches these subnets. For more information, see [Managing VPN routes](/docs/vpc?topic=vpc-vpn-client-to-site-routes) and the new [VPN server route methods](/apidocs/vpc/latest#list-vpn-server-routes).

**Configuring route propagation for VPN gateways and VPN servers.** When you [create a VPC routing table](/apidocs/vpc/latest#create-vpc-routing-table), you can now control if the routing table accepts routes from a VPN gateway or server by specifying the `accept_routes_from` property. When you [view a route in a VPC routing table](/apidocs/vpc/latest#get-vpc-routing-table-route), the new `origin` property shows who created the route (either `user` or `service`), and the `service` routes include a new `creator` property that references the resource that created the route. Routes with the `creator` property present cannot be deleted directly. For more information, see [Configuring route propagation for VPN gateways](/docs/vpc?topic=vpc-vpn-site-to-site-routes-propagating) and [VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-route-propagation).

**Concurrent update protection.** ETags returned on `GET`, `POST`, and `PATCH` requests represent the modifiable state of the resource. When updating the `accept_routes_from` property on a [routing table](/apidocs/vpc/latest#update-vpc-routing-table), or updating the `client_authentication`, `client_dns_servers`, or `subnets` properties on a [VPN server](/apidocs/vpc/latest#update-vpn-server), you must provide the resource's ETag using the `If-Match` header. For general guidance on the use of ETags, see [Concurrent update protection](/apidocs/vpc/latest#concurrent-update-protection).

## 28 June 2022
{: #28-june-2022}

### For all version dates
{: #28-june-2022-all-version-dates}

**Block storage.** You can now create a volume from a snapshot without having to also create and attach it to a virtual server instance. When you [create a volume](/apidocs/vpc/latest#create-volume), a new `source_snapshot` property lets you specify a snapshot which will be used as source data for the new volume. The volume's data is fully restored later, when you attach it to an instance. Volume performance is initially degraded until the volume data is fully restored. For more information, see [Restoring an unattached data volume from a snapshot with the API](/docs/vpc?topic=vpc-snapshots-vpc-restore&interface=api#snapshots-vpc-restore-unattached-api).

**Cross-zone member support for network load balancers.** You can now [create a load balancer pool](/apidocs/vpc/latest#create-load-balancer-pool) with members across any zone in the region. You can also use the [create pool member](/apidocs/vpc/latest#create-load-balancer-pool-member) and [replace pool member](/apidocs/vpc/latest#replace-load-balancer-pool-members) methods to update an existing pool with members across any zone in the region. The zone of the network load balancer is still identified by the subnet that you specify when you create a load balancer.

Network load balancers with `route_mode` enabled do not support cross zone members.

## 21 June 2022
{: #21-june-2022}

### For all version dates
{: #21-june-2022-all-version-dates}

**Backup for VPC.** You can now create backup policies to schedule automatic backups of your block storage volumes. Backups are made when a user tag in a block storage volume matches a user tag defined in a backup policy. Backups are created by a schedule defined in a [backup plan](/apidocs/vpc-beta#create-backup-policy-plan). Each plan also has a deletion policy for managing backups created by the plan, which you can customize by specifying the `deletion_trigger` sub-property. At the scheduled interval, a backup snapshot is created of that volume. You can have up to four backup plans per policy. See [Backup for VPC](/docs/vpc?topic=vpc-backup-service-about).

The backup policy jobs API remains in [beta](/apidocs/vpc-beta#24-may-2022).
{: note}

## 29 March 2022
{: #29-march-2022}

The `2022-03-29` release includes incompatible changes. To avoid regressions in client functionality, be sure to read and follow the [`2022-03-29` API migration guide](/docs/vpc?topic=vpc-2022-03-29-migration) before specifying version `2022-03-29` or later in any API requests.
{: important}

### For version `2022-03-29` or later
{: #version-2022-03-29}

**Reserved IPs for compute.** Using a `version` query parameter of `2022-03-29` or later, you can now fully control the IP addresses assigned to your network interfaces by specifying a new or existing reserved IP when you [create an instance](/apidocs/vpc#create-instance) or [create a bare metal server](/apidocs/vpc#create-bare-metal-server).

**Migration of network interface IP addresses.** In support of reserved IPs, for requests using a `version` query parameter of `2022-03-29` or later, the network interface `primary_ipv4_address` string property has been migrated to the `primary_ip` object property. See [Migrating use of IP addresses](/docs/vpc?topic=vpc-2022-03-29-migration#migrate-ip-addresses) for guidance on how to migrate to `primary_ip`.

**Removal of security group network interfaces.** The methods for associating security groups with network interfaces that were deprecated in [API version `2021-02-23`](/docs/vpc?topic=vpc-api-change-log#23-february-2021) have been removed as of version `2022-03-29`. See [Migrating use of security group associations](/docs/vpc?topic=vpc-2022-03-29-migration#migrate-security-group-assoc) for guidance on how to migrate to use security group targets, which allow security groups to be associated with VPC resources beyond network interfaces, such as endpoint gateways and load balancers.

### For all version dates
{: #29-march-2022-all-version-dates}

**Reserved IP management.** You can explicitly [Reserve an IP in a subnet](/apidocs/vpc#create-subnet-reserved-ip) ahead of time, and [List all reserved IPs on a subnet](/apidocs/vpc#list-subnet-reserved-ips) to see all your VPC resources that are using IP addresses on that subnet, including load balancers and VPN gateways (with [limitations](/docs/vpc?topic=vpc-known-issues#ip-known-issues)). For more information, see [Managing IP addresses](/docs/vpc?topic=vpc-managing-ip-addresses).

**UDP support for network load balancers.** When [creating a network load balancer](/apidocs/vpc#create-load-balancer) (NLB), you can now set User Datagram Protocol (UDP) as the communications protocol for NLB listeners and pools by specifying `udp` for the `protocol` sub-property of the [`listener`](/apidocs/vpc#create-load-balancer-listener-request) and [`pool`](/apidocs/vpc#create-load-balancer-pool) properties respectively. (Health checks do not support UDP for monitoring the health of pool members.) For more information, see [Configuring UDP for network load balancers](/docs/vpc?topic=vpc-nlb-udp&interface=api).

You must set the same protocol for the load balancer pool and listeners using that pool.
{: tip}

Not all network load balancer offerings will support UDP. Before creating a UDP network load balancer (or updating an existing NLB listener to use UDP), check that the `udp_supported` property of the [load balancer profile](/apidocs/vpc#list-load-balancer-profiles) is `true`.
{: important}

## 22 March 2022
{: #22-march-2022}

### For all version dates
{: #22-march-2022-all-version-dates}

**Concurrent update protection.** To prevent multiple clients from unknowingly overwriting each other's updates, select API methods support entity-tags and conditional requests. For details, see [Concurrent update protection](/apidocs/vpc#concurrent-update-protection) in the Virtual Private Cloud API.

## 22 February 2022
{: #22-february-2022}

### For all API version dates
{: #22-february-2022-all-version-dates}

**Instance availability policies for compute host failures.** A new `availability_policy` property has been added to the [create](/apidocs/vpc#create-instance) and [update](/apidocs/vpc#update-instances) instance methods to control the behavior when the instance's underlying compute host experiences a failure. The `host_failure` sub-property can be used to set the host failure `availability_policy` of the virtual server instance. The default policy is `restart`, which relocates the instance to a healthy host and restarts the instance. The policy may be set to `stop` to have the instance remain stopped if the compute host experiences a failure.

For more information, see [Host failure recovery policies](/docs/vpc?topic=vpc-host-failure-recovery-policies&interface=api).

## 15 February 2022
{: #15-february-2022}

### For all version dates
{: #15-february-2022-all-version-dates}

**Resizable boot volumes.** You can now increase the capacity of a boot volume, up to 250 gigabytes (GB). When [creating an instance](/apidocs/vpc#create-instance) from an image or an [instance template](/apidocs/vpc#create-instance-template), you can specify a larger capacity than the image's `minimum_provisioned_size` default. Specify `capacity` in the `volume` sub-property of the `boot_volume_attachment` property.  You can also increase the size of an existing boot volume by specifying the `capacity` property when [updating the volume](/apidocs/vpc#update-volume).

## 8 February 2022
{: #8-february-2022}

### For all version dates
{: #8-february-2022-all-version-dates}

**Port ranges for public network load balancers.** When [creating a public network load balancer](/docs/vpc?topic=vpc-nlb-ui-creating-network-load-balancer&interface=api) you can now specify a range of listener ports. When you configure a load balancer with a port range, the `port` property of the load balancer's [pool members](/apidocs/vpc#list-load-balancer-pool-members) will not be used for port translation on incoming traffic. Instead, traffic will arrive at the member on the same port it arrived on at the listener.

Before using this feature on a load balancer, update client applications that integrate with it to check the `port_min` and `port_max` properties on the [load balancer listener](/apidocs/vpc#get-load-balancer-listener). If those properties do not have the same value, the client must consider the inclusive range between `port_min` and `port_max` as the possible ports on which traffic can arrive at the member. However, if `port_min` and `port_max` have the same value, the behavior will be unchanged and traffic will arrive on the port specified by the `port` property of the load balancer pool member.

## 1 February 2022
{: #1-february-2022}

### For all version dates
{: #1-february-2022-all-version-dates}

**Bare metal servers for VPC.** You can now create bare metal servers to host VMware&reg; clusters in {{site.data.keyword.vpc_short}}. You can set up VMware management applications and create VMware virtual machines on the bare metal servers. The new [bare metal server](/vpc#list-bare-metal-servers) APIs use a similar structure and employ the same concepts as the existing [instance](/apidocs/vpc?code=go#list-instances) APIs. There is also a parallel but separate set of [bare metal server profile](/apidocs/vpc#list-bare-metal-server-profiles) APIs with similar conventions to the existing [instance profile](/apidocs/vpc-scoped#list-instance-profiles) APIs. After you've learned one concept, it will apply to the other. However, be aware that there are some [limitations](/docs/vpc?topic=vpc-known-issues#bare-metal-servers-limitations).

For more information, see [About Bare Metal Servers for VPC](/docs/vpc?topic=vpc-about-bare-metal-servers) and [Bare metal server profiles](/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=api), or dive into the new [API methods](/apidocs/vpc#list-bare-metal-server-profiles).

## 25 January 2022
{: #25-january-2022}

### For all version dates
{: #25-january-2022-all-version-dates}

**Security groups for endpoint gateways.** For enhanced security, you can now associate security groups with [endpoint gateways](/docs/vpc?topic=vpc-about-vpe). When you [create an endpoint gateway](/apidocs/vpc#create-endpoint-gateway), you can now specify the `security_groups` property, which associates those security groups with the endpoint gateway. If you do not specify `security_groups`, the endpoint gateway will be associated with the VPC's default security group. Before using the default security group, review your default security group rules and, if necessary, edit the rules to accommodate your endpoint gateway traffic.

Responses that return an endpoint gateway now include the `security_groups` property. On endpoint gateways created before 25 January 2022, the `security_groups` property in the response is an empty array (`[]`), and no security groups are set.

You can update security groups for an endpoint gateway by [adding an endpoint gateway](/apidocs/vpc#create-security-group-target-binding) to or [removing an endpoint gateway](/apidocs/vpc#delete-security-group-target-binding) from a security group's targets.

You will not be able to remove the only remaining security group from an endpoint gateway. As a result, if you add a security group to an endpoint gateway which had no security groups, you will not be able to revert the endpoint gateway to have no security groups.

Finally, the security group `targets` property can now refer to an endpoint gateway, as can the responses for the [get security group target](/apidocs/vpc#get-security-group-target) and [list security group target](/apidocs/vpc#list-security-group-targets) methods.

**Snapshots for VPC.** A `captured_at` property has been added to each [snapshot](/apidocs/vpc#list-snapshots), indicating the date and time when the snapshot was captured from the volume. The `captured_at` timestamp value is a close approximation to the actual snapshot time, typically within a few seconds. The actual snapshot capture is between the `created_at` and `captured_at` timestamps. (The `created_at` property indicates when the [snapshot creation](/apidocs/vpc#create-snapshot) process was initiated.)

If `captured_at` is absent from the response, the snapshot's data has not yet been captured. Additionally, the property may be absent for snapshots created before 1 January 2022.

## 23 November 2021
{: #23-november-2021}

### For all version dates
{: #23-november-2021-all-version-dates}

**Snapshots for VPC.** Restrictions have been removed for deleting snapshots. You can now [delete](/apidocs/vpc#delete-snapshot) any snapshot in the chain of snapshots.  If the snapshot is actively being used to restore a volume, the snapshot will remain in `deleting` until the restore completes. The `deletable` property, which indicated whether a snapshot could be deleted, has been deprecated.

## 19 October 2021
{: #19-october-2021}

### For all version dates
{: #19-october-2021-all-version-dates}

**GPU instances.** Updated instance and instance profile methods now include details about GPUs attached to the instance. New profiles provide support for GPUs. These GPUs provide accelerated computing to help you run workloads with more powerful compute capabilities.

The [list all instances](/apidocs/vpc#list-instances) method returns a new `gpu` property with additional sub-properties: `count`, `manufacturer`, `model`, and `memory`. The [retrieve an instance profile](/apidocs/vpc#get-instance-profile) method returns new properties: `gpu_count`, `gpu_manufacturer`, `gpu_model`, and `gpu_memory`. For more information, see [Managing GPUs](/docs/vpc?topic=vpc-managing-gpus).

## 28 September 2021
{: #28-september-2021}

### For all version dates
{: #28-september-2021-all-version-dates}

**Route mode for VNF support for network load balancers.** [Network load balancers](/apidocs/vpc#create-load-balancer) now support a new "route mode" enabling virtual network functions (VNFs) as back-end targets. A `route_mode` property has been added to the load balancer resource to indicate if the load balancer is in route mode. A `route_mode_supported` property has been added to the load balancer profile resource to indicate if the profile supports route mode. Presently, only network load balancer profiles support route mode.

The [Create load balancer](/apidocs/vpc#create-load-balancer) and [Create load balancer listener](/apidocs/vpc#create-load-balancer-listener) methods now accept properties `port_min` and `port_max`. You can request a load balancer listener for a single port by setting either the `port` property, or by setting the `port_min` and `port_max` properties to the same value. All load balancer listener responses now include `port_min` and `port_max` properties, with `port_min` matching the value of the existing `port` property.

When creating load balancers with route mode enabled, you must specify the listener's `port_min` value as `1`,  the `port_max` value as `65535`, and omit the `port` property. Other port range values are not currently supported, as noted in [Known issues](/docs/vpc?topic=vpc-known-issues).
{: note}

For more information, see [Creating a route mode Network Load Balancer for VPC](/docs/vpc?topic=vpc-nlb-vnf&interface=api).

## 7 September 2021
{: #7-september-2021}

### For all version dates
{: #7-september-2021-all-version-dates}

**Instance bandwidth.** New properties have been added to the [create](/apidocs/vpc#create-instance) and [update](/apidocs/vpc#update-instance) instance methods to allow adjustment to the amount of total bandwidth (in megabits per second) allocated exclusively to attached volumes. The range of acceptable volume bandwidth values depends on the selected instance profile. A new `total_volume_bandwidth` property, added to each [instance profile](/apidocs/vpc#list-instance-profiles), provides the range of possible values, and the default value used when creating an instance. An increase in `total_volume_bandwidth` will result in a corresponding decrease to `total_network_bandwidth`.

The volume bandwidth allocated to your existing instances will be unaffected unless:

- The instance's `total_volume_bandwidth` is lowered
- The total bandwidth requested by the instance's attached volumes exceeds the amount already requested by its attached volumes

For more information about this feature, see [Bandwidth allocation for instance profiles](/docs/vpc?topic=vpc-bandwidth-allocation-profiles).

## 31 August 2021
{: #31-august-2021}

### For all version dates
{: #31-august-2021-all-version-dates}

**Block storage volumes:**

- **Adjustable IOPS.** To manage the performance the your data volumes attached to running virtual server instances, use the [update volume](/apidocs/vpc#update-volume) method to specify a different tiered `profile` value, or a different `iops` value within the [custom IOPS](/docs/vpc?topic=vpc-block-storage-profiles&interface=api#custom) tier. For more information, see [Adjusting IOPS for block storage volumes](/docs/vpc?topic=vpc-adjusting-volume-iops&interface=api).

- **Expandable volumes.** You can now expand a secondary volume attached to a running virtual server instance. Use the `capacity` property in the [update volume](/apidocs/vpc#update-volume) method to request a new volume capacity, up to 16 TB (depending on the volume's profile).  While the volume's capacity is being updated, the volume will remain available for use, but will have a `status` value of `updating`. For more information, see [Expanding block storage volume capacity](/docs/vpc?topic=vpc-expanding-block-storage-volumes).

   If you expand an existing data volume, be aware that existing applications will be exposed to the new `updating` value. To avoid disruption, first check that your applications are written to gracefully handle unexpected `status` values.
   {: important}

## 24 August 2021
{: #24-august-2021}

### For all version dates
{: #24-august-2021-all-version-dates}

**Application load balancers.** Use the [HTTPS redirect feature](/docs/vpc?topic=vpc-load-balancers#https-redirect-listener) to redirect traffic from an HTTP load balancer listener to an HTTPS listener.

An HTTPS redirect can be configured on either [load balancer listeners](/docs/vpc?topic=vpc-load-balancers) or [load balancer policies](/docs/vpc?topic=vpc-layer-7-load-balancing), or both.  An HTTPS redirect is configured on the listener using the new `https_redirect` property, and will be used if none of the listener policy's rules match (or if it has no rules). An HTTPS redirect is configured on a load balancer policy using the new policy `action` value of `https_redirect`, and will be used only when all of the policy's rules match.

If you configure an HTTPS redirect on a listener policy, be aware that existing applications will be exposed to the new `https_redirect` value. To avoid disruption, check that your applications are written to gracefully handle unexpected `action` values first.
{: important}

Additional API restrictions are enforced after an HTTPS redirect is configured:

- You will not be able to [update the `protocol` and `accept_proxy_protocol` properties](/apidocs/vpc#update-load-balancer-listener) of the HTTP and HTTPS listeners. Instead, delete the listener and create a new listener with the new property values.
- You will not be able to [delete an HTTPS listener](/apidocs/vpc#delete-load-balancer-listener) until the HTTP listener referring to it is deleted.

## 17 August 2021
{: #17-august-2021}

### For all version dates
{: #17-august-2021-all-version-dates}

**Larger size boot volumes for custom images.** You can import custom images with a boot disk size from 10 GB to 250 GB, which will become the image's `minimum_provisioned_size` after import. When you specify the image as part of [creating an instance](/docs/vpc?topic=vpc-creating-virtual-servers), the boot volume `capacity` is set to the image's `minimum_provisioned_size`. For details, see [Planning custom images](/docs/vpc?topic=vpc-planning-custom-images).

**Placement groups.** Placement groups for {{site.data.keyword.vpc_full}} are logical groupings of virtual server instances that can be configured to reduce the risk of correlated failures inherent in your physical environment, such as networking issues, power loss, or hardware failure. Define a placement group strategy for high-availability workloads, such as for host or power spread. For more information, see [About placement groups](/docs/vpc?topic=vpc-about-placement-groups-for-vpc) or dive into the new [API methods](/apidocs/vpc#list-placement-groups).

## 10 August 2021
{: #10-august-2021}

### For all version dates
{: #10-august-2021-all-version-dates}

**LinuxONE (s390x processor architecture).** You can now [create virtual server instances](/apidocs/vpc#create-instance) on LinuxONE in {{site.data.keyword.Bluemix}} using new virtual server instance profiles. Instances provisioned with these profiles will have a VCPU architecture of s390x and interoperate with other VPC storage and networking features such as block storage volumes, floating IPs, and security groups. For more information, see [x86 instance profiles](/docs/vpc?topic=vpc-profiles&interface=api#balanced-s390x-profiles), and  [Service limitations](/docs/vpc?topic=vpc-limitations).

## 27 July 2021
{: #27-july-2021}

### For version `2021-07-27` or later
{: #version-2021-07-27}

**Instances.** When you [create an instance](/docs/vpc?topic=vpc-creating-virtual-servers) using a `version` query parameter of `2021-07-27` or later, the value for the boot volume's `capacity` is now based on the image's `minimum_provisioned_size`. If you use a `version` query parameter of `2021-07-26` or earlier, the value for the boot volume's `capacity` will continue to be 100 GB.

## 29 June 2021
{: #29-june-2021}

### For all version dates
{: #29-june-2021-all-version-dates}

**Keys.** Pagination has been added to the [List all keys](/apidocs/vpc#list-keys) method. Pagination will not occur until your account includes more than 50 keys in a region, but we recommend that you update your existing client applications in preparation. Contact IBM support if you need assistance.

## 15 June 2021
{: #15-june-2021}

### For all version dates
{: #15-june-2021-all-version-dates}

**Load balancer pools.** New cookie-based values have been added to the `session_persistence` enumeration returned by the load balancers pool methods. If you [create](/apidocs/vpc#create-load-balancer-pool) or [update](/apidocs/vpc#update-load-balancer-pool) pools with these new values to enforce session persistence, client applications will expose cookie values in all requests. For details, see [Cookie-based session persistence](/docs/vpc?topic=vpc-advanced-traffic-management#cookie).

## 8 June 2021
{: #8-june-2021}

### For version `2021-06-08` or later
{: #version-2021-06-08}

**Load balancers.** For requests using a `version` query parameter of `2021-06-08` or later, you can now use pagination when [listing all load balancers](/apidocs/vpc#list-load-balancers) in the region. Requests using a `version` query parameter of `2021-06-07` or earlier remain unpaginated, but may time out if you have many load balancers.

If you expect to use many load balancers at once, migrate your applications to the paginated API to improve responsiveness and reliability. Contact [IBM support](/docs/get-support?topic=get-support-using-avatar) if you need help migrating your existing client applications.

## 25 May 2021
{: #25-may-2021}

### For all version dates
{: #25-may-2021-all-version-dates}

**Image from volume.** On a `POST /images` request, you can now specify `source_volume` with an instance boot volume identity. Specifying the `encryption_key` property in that request encrypts the image with a root key of your choosing. For details, see [Creating an image from a volume](/docs/vpc?topic=vpc-create-ifv&interface=api#image-from-volume-vpc-api).

## 18 May 2021
{: #18-may-2021}

### For all version dates
{: #18-may-2021-all-version-dates}

**Snapshots for VPC.** Use the new regional snapshot service to create point-in-time copies of your block storage boot or data volumes. Select a snapshot during instance provisioning and restore a new, fully-provisioned boot volume to start the instance. You can also create and attach a data volume from a snapshot within a running virtual server instance. Learn about [creating and using snapshots](/docs/vpc?topic=vpc-snapshots-vpc-about) and explore the new [API methods](/apidocs/vpc#delete-snapshots).

## 6 May 2021
{: #6-may-2021}

### For all version dates
{: #6-may-2021-all-version-dates}

**Scheduled scaling.** Use scheduled scaling for VPC to schedule actions that automatically add or remove instance group capacity, based on daily, intermittent, or seasonal demand. You can create multiple scheduled actions that scale capacity monthly, weekly, daily, hourly, or even every set number of minutes. Explore the instance group [managers methods](/apidocs/vpc#list-instance-group-managers) and the new [manager actions methods](/apidocs/vpc#list-instance-group-manager-actions).

## 30 March 2021
{: #30-march-2021}

### For all version dates
{: #30-march-2021-all-version-dates}

**Instance storage.** New instance profiles can now optionally include a set of solid state disks. These instance storage disks provide temporary storage to improve the performance of cloud-native workloads that need scratch space, large data caches, or data replicated across availability zones.

The following API methods have been added:

- [List all disks on an instance](/apidocs/vpc#list-instance-disks) (`GET /instances/{instance_id}/disks`)
- [Retrieve an instance disk](/apidocs/vpc#get-instance-disk) (`GET /instances/{instance_id}/disks/{id}`)
- [Update an instance disk](/apidocs/vpc#update-instance-disk) (`PATCH /instances/{instance_id}/disks/{id}`)
- [List all disks on a dedicated host](/apidocs/vpc#list-dedicated-host-disks) (`GET /dedicated_hosts/{dedicated_host_id}/disks`)
- [Retrieve a dedicated host disk](/apidocs/vpc#get-dedicated-host-disk) (`GET /dedicated_hosts/{dedicated_host_id}/disks/{id}`)
- [Update a dedicated host disk](/apidocs/vpc#update-dedicated-host-disk) (`PATCH /dedicated_hosts/{dedicated_host_id}/disks/{id}`)

API methods that return instance and dedicated host profiles now include a `disks` property with information about the storage capability (where present) of resources provisioned with those profiles.

API methods that return instances and dedicated hosts now include a `disks` property with information about the disks configured for those resources.

For more information, see [About instance storage](/docs/vpc?topic=vpc-instance-storage).

**Virtual server instance console.** You can now access your instances by connecting to a VNC or serial console. Learn about [Accessing virtual server instances by using VNC or serial consoles](/docs/vpc?topic=vpc-vsi_is_connecting_console), and explore the new instance console API methods:

- [Create a console access token for an instance](/apidocs/vpc#create-instance-console-access-token) (`POST /instances/{instance_id}/console_access_token`)
- [Retrieve the console WebSocket for an instance](/apidocs/vpc#get-instance-console) (`GET /instances/{instance_id}/console`)

**Instance resize.** You can now resize an instance by providing the `profile` property in the API method `PATCH /instances/{id}` ([Update an instance](/apidocs/vpc#update-instance)). For more information, see [Resizing a virtual server instance](/docs/vpc?topic=vpc-resizing-an-instance&interface=api).

## 23 March 2021
{: #23-march-2021}

### For all version dates
{: #23-march-2021-all-version-dates}

**New parameter-based rule types for an application load balancer.** When creating a load balancer listener policy rule, the `field` property may now be set to `query` or `body` to perform additional forms of layer 7 load balancing:

- `query` - Write layer 7 rules that use the query string to route traffic to a specific target. For a `query` type rule, `field` and `value` must be percent-encoded, same as the query string in the URL.
- `body` - If the body of the `POST` request uses form encoding (UTF-8), then you can create layer 7 rules to route traffic based on the parameter name and value in the body. The `Content-Type` in the request is ignored.

If you use these new rule types, be aware that existing client applications will be exposed to those new values in the existing  properties. To avoid disruption, check that client applications are written to gracefully handle unexpected values for these properties before using these new rule types for an application load balancer.
{: important}

## 19 March 2021
{: #19-march-2021}

### For all version dates
{: #19-march-2021-all-version-dates}

**Bring your own license.** You can now [bring your own license](/docs/vpc?topic=vpc-byol-vpc-about) (BYOL) for custom images that you create and import to IBM Cloud VPC. When you import a custom image, you can choose from new `byol` Red Hat Enterprise Linux (RHEL) and Windows operating systems.

A new `dedicated_host_only` property has been added to operating system resources. Any instance with a boot volume created from an image with `operating_system.dedicated_host_only` set to `true` must be placed on a dedicated host (or into a dedicated host group). Because Windows BYOL images have `dedicated_host_only` set to `true`, they must be placed on a dedicated host (or into a dedicated host group). There are no restrictions on placing instances using RHEL BYOL images.

Every operation that returns an `OperatingSystem` resource now includes a `dedicated_host_only` property.

## 9 March 2021
{: #9-march-2021}

### For all version dates
{: #9-march-2021-all-version-dates}

**Additional VPN for VPC IKEv2 encryption/hash/Diffie Hellman (DH) group support.** For enhanced security, VPN for VPC now supports SHA2-512 (a Secure Hash Algorithm) and DH group 19 (a 256-bit elliptic curve algorithm) to generate a symmetric key.

If you use these new algorithms, be aware that existing client applications will be exposed to those new values in the existing `authentication_algorithm` and `dh_group` properties. To avoid disruption, check that client applications are written to gracefully handle unexpected values for these properties before using these new algorithms.
{: important}

The following VPN for VPC methods have been updated:

- In IKE policies, the `authentication_algorithm` property now includes a `sha512` value, and the `dh_group` property includes a `19` value.
- In IPsec policies, the `authentication_algorithm` property now includes a `sha512` value, and the `pfs` property includes a `group_19` value.

**New VPN gateway property.** Each element of the existing VPN gateway `members` array now includes a `private_ip` property, which provides the IP address assigned to that VPN gateway member.

## 23 February 2021
{: #23-february-2021}

### For all version dates
{: #23-february-2021-all-version-dates}

**Application load balancer security group integration.** For enhanced security, application load balancers can now be associated with security groups. You can specify one or more security groups when you create the application load balancer, and associate security groups with your existing application load balancers. If you omit security groups during load balancer creation, the default security group for the VPC is used.

If you plan to use default security groups for new application load balancers, review your default security group rules. If necessary, edit the rules to accommodate your expected application load balancer traffic.
{: tip}

The following load balancer methods have been updated:

- [Create a load balancer](/apidocs/vpc#create-load-balancer) (`POST /load_balancers`) can now accept a list of security groups
- [Get load balancer details](/apidocs/vpc#get-load-balancer) (`GET /load_balancers/{id}`) now returns references to the security groups to which a load balancer is attached

New security group methods have been added for managing security group targets:

- [Attach a security group to a target network interface or load balancer](/apidocs/vpc#create-security-group-target-binding) (`PUT /security_groups/{security_group_id}/targets/{id}`)
- [List targets attached to a security group](/apidocs/vpc#list-security-group-targets) (`GET /security_groups/{security_group_id}/targets`)
- [Retrieve a target in a security group](/apidocs/vpc#get-security-group-target) (`GET /security_groups/{security_group_id}/targets/{id}`)
- [Delete targets from a security group](/apidocs/vpc#delete-security-group-target-binding) (`DELETE /security_groups/{security_group_id}/targets/{id}`)

Use the security group target methods to manage security group attachments to both load balancers and network interfaces. The original methods specific to network interfaces are now deprecated:

- `GET /security_groups/{security_group_id}/network_interfaces`
- `DELETE /security_groups/{security_group_id}/network_interfaces/{id}`
- `GET /security_groups/{security_group_id}/network_interfaces/{id}`
- `PUT /security_groups/{security_group_id}/network_interfaces/{id}`

For more information, see [Integrating an IBM Cloud Application Load Balancer for VPC with security groups](/docs/vpc?topic=vpc-alb-integration-with-security-groups).

**Bring Your Own IP (BYOIP) support for VPC.** VPC address prefixes are no longer restricted to [RFC-1918](https://tools.ietf.org/html/rfc1918) addresses. You must now configure VPCs that use both non-RFC-1918 addresses and have public connectivity (floating IPs or public gateways) using a custom route that contains the new `delegate_vpc` property. You must specify this property for destination CIDRs that are non-RFC-1918 compliant and outside of the VPC, such as for destinations that are reachable through {{site.data.keyword.dl_full}}, {{site.data.keyword.cloud_notm}} Transit Gateway, or VPC classic access.

The `delegate_vpc` property is not required if a VPC uses only RFC-1918 addresses or has no public connectivity.
{: note}

The following API methods have been updated:

- View the `delegate_vpc` property in the requests and responses for `/vpcs/{vpc_id}/routing_tables/{routing_table_id}/routes`.
- View reserved IP ranges in `POST /vpcs/{vpc_id}/address_prefixes`, which creates an address pool prefix.

## 27 January 2021
{: #27-january-2021}

### For all version dates
{: #27-january-2021-all-version-dates}

**Checksum (SHA256) for imported images.** When you import a custom image to {{site.data.keyword.vpc_short}}, you can now view the checksum that was generated for the image during the import operation. By generating a checksum for the image locally, and checking that the checksums match, you can verify the integrity of the imported image. For more information, see [Validating a custom image after importing](/docs/vpc?topic=vpc-managing-images#validate-custom).

The `sha256` checksum is available in the `file` details in API method `GET /images/{id}`. See [Retrieve an image](/apidocs/vpc#get-image).

## 19 January 2021
{: #19-january-2021}

### For all version dates
{: #19-january-2021-all-version-dates}

The quantity of memory for virtual server instance profiles is now provisioned in gibibytes (GiB), instead of gigabytes (GB). For example, creating a new `bx2-4x16` virtual server instance provisions the instance with 16 GiB (17,179,869,184 bytes), instead of 16 GB (16,000,000,000 bytes). Virtual server instances that have already been provisioned are not affected.

### For version `2021-01-19` or later
{: #version-2021-01-19}

For requests using a `version` query parameter of `2021-01-19` or later, memory for virtual server instances is now expressed in gibibytes (GiB), instead of gigabytes (GB). For example, the `memory` property returned from `GET /instances/{id}` now reports in GiB (truncated to a whole number).

## 16 December 2020
{: #16-december-2020}

### For all version dates
{: #16-december-2020-all-version-dates}

**Customer-managed encryption for block storage volumes and encrypted custom images.** When you disable or delete a customer root key (CRK) that is encrypting your block storage or custom image resources, the API displays a status of `unusable` for these resources, along with the reason codes `encryption_key_deleted` or `encryption_key_disabled`.

The `unusable` status appears in the following API methods:

- [List all volumes](/apidocs/vpc#list-volumes) (`GET /volumes`)
- [Retrieve a specific volume](/apidocs/vpc#get-volume) (`GET /volumes/{id}`)
- [List all images](/apidocs/vpc#list-volumes) (`GET /images`)
- [Retrieve the specified image](/apidocs/vpc#get-image) (`GET /images/{id}`)

For more information on key states and resource statuses, see [User actions that impact root key states and resource status](/docs/vpc?topic=vpc-vpc-encryption-managing#byok-root-key-states).

**Dedicated hosts** are now supported in the VPC API. Learn more about using [dedicated hosts](/docs/vpc?topic=vpc-creating-dedicated-hosts-instances&interface=api) and explore the new [API methods](/apidocs/vpc#list-dedicated-host-groups).

## 20 November 2020
{: #20-november-2020}

### For all version dates
{: #20-november-2020-all-version-dates}

**Datapath log forwarding with {{site.data.keyword.la_full_notm}}** is now available for [IBM Cloud Application Load Balancer for VPC](/docs/vpc?topic=vpc-load-balancers#load-balancers). Data and health check logs are valuable for debugging, analysis, and maintenance purposes. With the datapath logging feature enabled, your load balancer forwards these logs to your account's [IBM Log Analysis](https://cloud.ibm.com/observe/logging){: external} dashboard.

View the `logging` property in the following API methods:

- [List all load balancers](/apidocs/vpc#list-load-balancers) (`GET /load_balancers`)
- [Retrieve a load balancer](/apidocs/vpc#get-load-balancer) (`GET /load_balancers/{id}`)

For more information, see [Datapath log forwarding with IBM Log Analysis](/docs/vpc?topic=vpc-datapath-logging#datapath-logging).

## 19 November 2020
{: #19-november-2020}

### For all version dates
{: #19-november-2020-all-version-dates}

**Support for ingress routing** is included as part of [routing tables](/apidocs/vpc#list-vpc-routing-tables), which were released on 30 October 2020. Use [ingress routing](/apidocs/vpc#create-vpc-routing-table) to control the policy for packets that are coming in to your VPC or one of its zones. The policy can vary, depending on the type of source and the destination IP address range.

Routing tables for the VPC API are the same for both egress and ingress routing, with the following additional properties that you can specify for ingress routing:

- `route_direct_link_ingress`
- `route_transit_gateway_ingress`
- `route_transit_gateway_ingress`

For more information, see [About routing tables and routes](/docs/vpc?topic=vpc-about-custom-routes).

## 13 November 2020
{: #13-november-2020}

### For version `2020-11-13` or later
{: #version-2020-11-13}

**Static-route-based VPN gateways** are now available for requests using a `version` query parameter of `2020-11-13` or later. For a [static-route-based VPN gateway](/apidocs/vpc#create-vpn-gateway), virtual tunnel interfaces are created. Any traffic that is routed to these interfaces with [user-defined routes](/docs/vpc?topic=vpc-create-vpc-route) is encrypted. For more information, see [About VPN gateways](/docs/vpc?topic=vpc-using-vpn).

## 30 October 2020
{: #30-october-2020}

### For all version dates
{: #30-october-2020-all-version-dates}

- **Custom routing tables** are now supported in the VPC API. This feature controls where network traffic is directed on a per-subnet basis. Explore new API methods for [routing tables](/apidocs/vpc#list-vpc-routing-tables) and [routes](/apidocs/vpc#create-vpc-routing-table-route). This feature subsumes the [VPC routing API](/apidocs/vpc#list-all-routes-in-the-vpc-s-default-routing-table), which remains supported but is deprecated and might be removed in a future API release.

- **Virtual private endpoint gateways.** Use [virtual private endpoint gateways](/apidocs/vpc#list-endpoint-gateways) to connect to supported {{site.data.keyword.cloud_notm}} services from your VPC network by using the IP addresses of your choice, which is allocated from a subnet within your VPC. For more information, see [About virtual private endpoint gateways](/docs/vpc?topic=vpc-about-vpe).

- **VPC network interfaces.** IP anti-spoofing checks had already been provided for enhanced security. However, certain use cases, such as having a virtual server instance act as a network gateway, require selective disabling of these checks. To accommodate these use cases, if you have the `is.instance.instance.ip-spoofing` IAM action, you can now enable the `allow_ip_spoofing` property when you [create a network interface](/apidocs/vpc#create-instance-network-interface). Alternatively, toggle the property when you [update an existing network interface](/apidocs/vpc#update-instance-network-interface). See also [About IP spoofing checks](/docs/vpc?topic=vpc-ip-spoofing-about).

- **Proxy protocol for application load balancers for VPC.** When you configure a load balancer [pool](/apidocs/vpc#create-load-balancer-pool) to use proxy protocol, the pool will pass information about the client to a back-end pool member when a connection is opened.

    You can also configure a load balancer [listener](/apidocs/vpc#create-load-balancer-listener) to accept proxy protocol information. This feature is useful when the client is, itself, a proxy (which, in turn, was connected to by the actual client) that supports the proxy protocol. This allows client information to be obtained and passed on to any pools that, themselves, have the proxy protocol enabled.

    For more information, see [Enabling proxy protocol](/docs/vpc?topic=vpc-advanced-traffic-management#proxy-protocol-enablement).

## 5 October 2020
{: #2020-10-05}

### For all version dates
{: #2020-10-05-all-version-dates}

**Encrypted images.** Use the VPC API to create your own image, encrypt it with your own key, and import it, encrypted, into {{site.data.keyword.cloud_notm}}. After you import the image, use it like any other image. If you use the image to provision an instance, its boot volume is encrypted using the image's root encryption key or another root encryption key of your choosing.

Dive into the APIs to [import an encrypted image](/apidocs/vpc#create-image) and [provision an instance](/apidocs/vpc#create-instance) from that encrypted image. See also [Creating an encrypted custom image](/docs/vpc?topic=vpc-create-encrypted-custom-image).

## 31 August 2020
{: #2020-08-31}

### For all version dates
{: #2020-08-31-all-version-dates}

**Network load balancers.** You can now use the [load balancers API](/apidocs/vpc#list-load-balancer-profiles) to distribute traffic among multiple server instances within the same region of your VPC. To learn how to create and manage a network load balancer, see [About IBM Cloud Network Load Balancer for VPC](/docs/vpc?topic=vpc-network-load-balancers).

The network load balancers API is shared between {{site.data.keyword.cloud_notm}} application load balancers and network load balancers.
{: note}

## 25 August 2020
{: #2020-08-25}

### For all version dates
{: #2020-08-25-all-version-dates}

This API release supports the following changes:

- Create an instance group of a fixed size and use it as a stand-alone feature
- Use the instance group manager to manage the instance group, which can associate an auto scale policy with that group

For more information, see [Creating an instance group for auto scaling](/docs/vpc?topic=vpc-creating-auto-scale-instance-group).

The following new methods are available for [instance groups](/apidocs/vpc#list-instance-groups):

- `GET` and `POST` for `/instance_groups`
- `DELETE`, `GET`, and `PATCH` for `/instance_groups/{id}`
- `DELETE` for `/instance_groups/{instance_group_id}/load_balancer`
- `GET` and `POST` for `/instance_groups/{instance_group_id}/managers`
- `DELETE`, `GET`, and `PATCH` for `/instance_groups/{instance_group_id}/managers/{id}`
- `GET` and `POST` for `/instance_groups/{instance_group_id}/managers/{instance_group_manager_id}/policies`
- `DELETE`, `GET`, and `PATCH` for `/instance_groups/{instance_group_id}/managers/{instance_group_manager_id}/policies/{id}`
- `GET` for `/instance_groups/{instance_group_id}/memberships`
- `DELETE`, `GET`, and `PATCH` for `instance_groups/{instance_group_id}/memberships/{id}`

You can also use the new [instance template](/apidocs/vpc#list-instance-templates) feature independently of auto scale. For example, create a template, and then create instances from that template, without creating an instance group.

The following new endpoints are now available for instances:
- `GET` and `POST` for `/instance/templates`
- `GET`, `PATCH`, and `DELETE` for `/instance/templates/{id}`

`POST /instances` now supports a `source_template`.

## 23 July 2020
{: #2020-07-23}

### For all version dates
{: #2020-07-23-all-version-dates}

The [flow log collectors API](/apidocs/vpc#list-flow-log-collectors) is now generally available.

## 22 July 2020
{: #2020-07-22}

### For all version dates
{: #2020-07-22-all-version-dates}

This API release supports the following enhancements for customer-managed encryption for block storage boot and data volumes:

- `POST /instances` -- [Create an instance](/apidocs/vpc#create-instance) and new volume, encrypted, using your customer root key (CRK). CRKs are imported to {{site.data.keyword.cloud_notm}} or created in a key management service.
- `POST /volumes` -- Create an unattached data [volume](/apidocs/vpc#create-volume), encrypted, using your CRK.

## 12 May 2020
{: #2020-05-12}

### For all version dates
{: #2020-05-12-all-version-dates}

Configure load balancer pool resources and their health monitors to use the HTTPS protocol. This enhancement enables end-to-end SSL encryption with HTTPS listeners, along with HTTPS health checks for increased availability. See the [load balancers API](/apidocs/vpc#list-load-balancers).

## 1 May 2020
{: #2020-05-02}

### For all version dates
{: #2020-05-02-all-version-dates}

This API release supports the following changes:

- [Flow log collectors](/apidocs/vpc#list-flow-log-collectors) API is available as beta
- `GET` /security_groups now supports `vpc.crn` and `vpc.name` filters

## 17 April 2020
{: #2020-04-17}

### For all version dates
{: #2020-04-17-all-version-dates}

{{site.data.keyword.cloud_notm}} interprets volume capacity units in gibibytes, but the API documentation used gigabytes. This issue is now resolved in the documentation.

## 10 April 2020
{: #2020-04-10}

### For all version dates
{: #2020-04-10-all-version-dates}

Usage recommendations are provided for the following load balancer properties:

- `protocol` property for load balancer pools
- `type` property for load balancer pool health monitors

The guidance notes that new values for these properties might be added in the future, and unexpected values are handled gracefully. See the [load balancers API](/apidocs/vpc#list-load-balancers).

## 6 February 2020
{: #2020-02-06}

### For all version dates
{: #2020-02-06-all-version-dates}

Support is temporarily suspended for creating instances from an existing boot volume. This feature was available through the API only, with no CLI or UI support. In the interim, you must specify the `image` property when you call `POST /instances`.

You can still create instances that reference existing data volumes.
{: note}

## 3 December 2019
{: #2019-12-03}

### For all version dates
{: #2019-12-03-all-version-dates}

Device IDs are now shown when you retrieve an instance's volume attachments.

## 26 November 2019
{: #2019-11-26}

### For all version dates
{: #2019-11-26-all-version-dates}

This API release supports the following changes:

- [Network access control list (ACL)](/apidocs/vpc#list-network-acls) methods
- Instance filtering by VPC

## 21 November 2019
{: #2019-11-21}

### For all version dates
{: #2019-11-21-all-version-dates}

A VPC’s cloud service endpoint source IPs now appear in output. Learn about [cloud service endpoint source addresses](/docs/vpc?topic=vpc-vpc-behind-the-curtain#cse-source-addresses) and how [DNS resolves shared cloud service endpoints](/docs/vpc?topic=vpc-service-endpoints-for-vpc#dns-domain-name-system-resolver-endpoints).

## 5 November 2019
{: #2019-11-05}

### For all version dates
{: #2019-11-05-all-version-dates}

This API release supports the following changes:

- [Load balancers](/apidocs/vpc#list-load-balancers) API is available as beta
- [VPN gateways](/apidocs/vpc#list-vpn-gateways) are available as beta
- Pagination is now supported for [instances](/apidocs/vpc#list-instances)
- Classic access to VPCs (also known as classic peering) is supported
