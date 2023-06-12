---

copyright:
  years: 2020, 2022
lastupdated: "2022-11-11"

keywords: virtual private endpoints, VPE, endpoint gateway

subcollection: vpc
---

{{site.data.keyword.attribute-definition-list}}

# Creating an endpoint gateway
{: #ordering-endpoint-gateway}
{: help}
{: support}

You can create an endpoint gateway for an {{site.data.keyword.cloud}} service that you want to access on your private VPC network by using the UI, CLI, or API.
{: shortdesc}

## Before you begin
{: #vpe-before-you-begin}

Prior to creating an endpoint gateway, ensure that you review [Planning for virtual private endpoint gateways](/docs/vpc?topic=vpc-planning-considerations) and have satisfied the following conditions:

* A VPC
* A subnet in at least one availability zone if you intend on binding an IP address at the same time you provision the endpoint gateway
* An instance of the service
* Appropriate [IAM permissions](/docs/vpc?topic=vpc-vpe-iam) to create an endpoint gateway, create or bind a reserved IP, and view or list the target service
* Verification that the service you are configuring supports VPE  

## Creating an endpoint gateway in the UI
{: #vpe-creating-ui}
{: ui}

To create an endpoint gateway by using the {{site.data.keyword.cloud_notm}} console, follow these steps:
{: shortdesc}

1. From the [{{site.data.keyword.cloud_notm}} console](/login){: external}, select the Menu icon ![Menu icon](/images/menu_icon.png), then click **VPC Infrastructure > Virtual private endpoint gateways** in the Network section. The Virtual private endpoint gateways for VPC page appears.
1. Click **Create** to go to the provisioning page.
1. In the Details section, enter values for the following fields:
   * **Name** - Type a unique name for your endpoint gateway.
   * **Resource group** - Select a resource group for the endpoint gateway. You can use the default group for this endpoint gateway, or choose from the list. For more information, see [Best practices for organizing resources in a resource group](/docs/account?topic=account-account_setup).
   * **Tags** - Optionally, add tags to organize, track usage costs, or manage access to your resources. 
   * **Access management tags** - Optionally, add access management tags to resources to help organize access control relationships. The only supported format for access management tags is `key:value`. For more information, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
   * **Virtual private cloud** - Select a VPC from the available list where you need the VPE IP address. The systems provides you with a list of IBM services that you can access within the region of the VPC network to associate with the endpoint gateway. If the target service is outside the VPC's region, you can change it for an updated list of services.

    If the intended service or service instance does not appear, revalidate your IAM permissions. You can also choose to allocate a reserved IP address to bind to the gateway, or specify a VPC subnet to allocate a reserved IP address for binding to the gateway.
    {: note}

1. In the Security groups section, select the checkboxes of the security groups that you want to attach from the security group table. You can then manage these security groups and tighten their security rules for inbound traffic towards your endpoint gateways.

    When you create an endpoint gateway without specifying a security group, the VPC default security group is attached to the endpoint gateway. For more information, see [Configuring ACLs and security groups for use with endpoint gateways](/docs/vpc?topic=vpc-configure-acls-sgs-endpoint-gateways).  

   Endpoint gateways created prior to support for security groups being introduced do not have a security group attached and allow all inbound traffic. If you attach a security group to a VPE that does not have a security group, you cannot revert back to having no security groups. You can revert to the previous "allow all inbound traffic" behavior by attaching a security group with rules for allowing all inbound traffic. However, such a rule is inherently less secure than having a more restrictive security group in place, and is not recommended.
   {: important}

1. In the Cloud service section, select the {{site.data.keyword.cloud_notm}} service that you want to access using this endpoint gateway. The region is pre-selected to optimize performance.
1. In the Reserved IP section, select a reserved IP address. You can choose:

   * **Select one for me** - Enter a name for the IP address, or one is chosen for you. Select a subnet from the list and enable the Auto-delete toggle if you want this reserved IP to be deleted automatically if the endpoint gateway is deleted.
   * **Select from existing IPs** - Choose from a list of existing IP addresses.
   * **Select one later** - Select an IP address after creating the endpoint gateway.

1. The order Summary shows pricing estimates for your review. Review and click **Create endpoint gateway**.

## Creating an endpoint gateway from the CLI
{: #vpe-ordering-cli}
{: cli}

Before you begin, make sure to [set up your CLI environment](/docs/vpc?topic=vpc-infrastructure-cli-plugin-vpc-reference).

To create an endpoint gateway by using the CLI, follow these steps:

1. List the IBM Cloud service instances that are qualified to be set as endpoint gateway targets:

   ```sh
   ibmcloud is endpoint-gateway-targets
   ```
   {: pre}

1. Create an endpoint gateway by running the following command:

   ```sh
   ibmcloud is endpoint-gateway-create \
    --vpc-id VPC_ID \
    --target TARGET [--name NAME] [(--reserved-ip-id RESERVED_IP_ID1 --reserved-ip-id RESERVED_IP_ID2 ...) \
    | (--new-reserved-ip NEW_RESERVED_IP1 --new-reserved-ip NEW_RESERVED_IP2 ...)] \
    [--resource-group-id RESOURCE_GROUP_ID | --resource-group-name RESOURCE_GROUP_NAME] \
    [--json]  
   ```
   {: codeblock}

Where:

* **--vpc-id** is the ID of the VPC.
* **--target** is the name, or CRN, of a provider cloud service instance.

   You can use command `ibmcloud is endpoint-gateway-targets` to list the provider cloud and infrastructure services that are qualified to be set as endpoint gateway target.
   {: note}

* **--name** is the new name of the endpoint gateway.
* **--reserved-ip-id** is the ID of the reserved IP to be bound to the endpoint gateway.
* **-new-reserved-ip: RESERVED_IP_JSON|@RESERVED_IP_JSON_FILE** is the new reserved IP configuration in JSON format or a JSON file.
* **-security-group** is the ID of the security group.

    If you do not specify **-security-group**, the VPC default security group is bound to the endpoint gateway.
    {: note}
    
* **--resource-group-id** is the ID of the resource group. This option is mutually exclusive with **--resource-group-name**.
* **--resource-group-name** is the name of the resource group. This option is mutually exclusive with **--resource-group-id**.
* **--output** is the output format. Only JSON is supported.
* **-q, --quiet** suppresses verbose output.

## Creating an endpoint gateway with the API
{: #vpe-ordering-api}
{: api}

To create an endpoint gateway by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store the following values in variables to be used in the API command:

   * `ResourceGroupId` - First, get your resource group and then populate the variable:

      ```sh
      export ResourceGroupId=<your_resourcegroup_id>
      ```
      {: pre}

   * `VpcId` - Find by using the **list vpc** command (with the preceding variables) and then populate the variable based on the provided ID:

      ```sh
      export VpcId=<your_VPC_id>
      ```
      {: pre}

   * `TargetCrn` - The name, or CRN, of the instance of the IBM Cloud service where you want to set the endpoint gateway. You can use the command `ibmcloud is endpoint-gateway-targets` to list the IBM Cloud service instances that are qualified to be set as endpoint gateway targets.

      ```sh
      export TargetCrn=<CRN of a target service>
      ```
      {: pre}

   * `SubnetID` (Optional) - The subnet ID.

      ```sh
      export SubnetID=<your_subnet_id>
      ```
      {: pre}

1. When all variables are initiated, do one of the following:

   * To create an endpoint gateway for the specific VPC:   

      ```sh
      curl -X POST -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/endpoint_gateways?version=$api_version&generation=2" \
      -d {  
      "name": endpoint-gateway-1",  
      "vpc": {"id":"'$VpcId'"},
      "target": {
      "crn": "$TargetCrn",
      "resource_type": "provider_cloud_service"
      },
      "security_groups": [
      {"id": "'$sg'"}
      ]
      }'
      ```
      {: codeblock}
      
      
      After you create an endpoint gateway for your service instance and assign a reserved IP address, you must bind the IP addresses from your VPC network to the endpoint gateway. See [Binding and unbinding a reserved IP address](/docs/vpc?topic=vpc-bind-unbind-reserved-ip) for details. These bound IP addresses become the VPE to access the service mapped to the endpoint gateway.
      {: note}

   * To create an endpoint gateway with an associated reserved IP address:

      ```sh
      curl -X POST -sH "Authorization:${iam_token}" \
      "$vpc_api_endpoint/v1/endpoint_gateways?version=$api_version&generation=2" \
      -d {
      "name": endpoint-gateway-1",
      "vpc": {"id":"'$VpcId'"},
      "target": {
      "crn": "$TargetCrn",
      "resource_type": "provider_cloud_service"
      },
      "security_groups": [
      {"id": "'$sg'"}
      ],
      "ips": [
      {"subnet": { "id": "$SubnetId" } }
      ],
      }'
      ```
      {: codeblock}

After an endpoint gateway is created for an {{site.data.keyword.cloud_notm}} service, it presents the endpoints associated with it. You can edit the endpoint gateway, such as binding/unbinding IPs, changing the resource group, and updating tags. If you need to change the service endpoints for any reason, you must delete and recreate the endpoint gateway.
{: important}
