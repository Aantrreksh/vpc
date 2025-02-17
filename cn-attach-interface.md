---

copyright:
  years: 2024
lastupdated: "2024-10-15"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Attaching cluster network interfaces to an instance
{: #attach-interfaces-cluster-network}

Contact your IBM Support representative if you are interested in getting early access to this offering. It is currently available for early access users of the `gx3d-160x1792x8h100` [virtual server instance profiles](/docs/vpc?topic=vpc-profiles#gpu) in the `us-east` region.
{: beta}

After you create an instance, you can attach it to a cluster network instance subnet. Alternatively, you can plan out your network by creating your interfaces up front before attaching your instances.  
{: shortdesc} 

## Before you begin
{: #attach-interfaces-prerequisites}

Review [Planning considerations](/docs/vpc?topic=vpc-planning-cluster-network&interface=ui) and [Known issues and limitations](/docs/vpc?topic=vpc-limitations-cluster-network&interface=ui).

You can attach cluster network interfaces to an instance with the UI, CLI, API, or Terraform.

## Attaching cluster network interfaces to an instance in the UI
{: #attach-interfaces-cluster-network-ui}
{: ui}
 
Currently, the UI does not support this feature for beta.
{: note}

## Attaching cluster network interfaces to an instance in the CLI
{: #attach-interfaces-cluster-network-cli}
{: cli}

To attach interfaces to an instance in the CLI, follow these steps:

1. [Set up your CLI environment](/docs/vpc?topic=vpc-set-up-environment&interface=cli).
1. Log in to your account with the CLI. After you enter the password, the system prompts for the account and region that you want to use:

    ```sh
    ibmcloud login --sso
    ```
    {: pre}

1. Enable the following feature flag:

   ```sh
   export IBMCLOUD_IS_FEATURE_CLUSTER_NETWORK=true
   ```
   {: pre}

1. To attach an interface to an instance, enter the following command:

   ```bash
   ibmcloud is instance-cluster-network-attachments INSTANCE --cnac CLUSTER_NETWORK_ATTACHMENT
   ```
   {: pre}

   Where:

   `INSTANCE`
   :    ID or name of the instance.

   `--output`
   :    Specify output format, only JSON is supported. One of: `JSON`.

   `-q`, `quiet`
   :    Suppress verbose output.

### Command examples
{: #command-examples-cluster-network-attach-interfaces}

To create instance `my-ins-1` with cluster network atttachments:

```sh
ibmcloud is instance-create my-ins-1 cli-vpc us-south-2 fx3d-160c1792x9h100 cli-subnet --image ibm-ubuntu-20-04-6-minimal-amd64-5 --cluster-network-attachments '[{"name":"instance-cnac-1","cluster_network_interface":{"id":"7208-18204195-be40-4f12-aaaa-2649e19acb91"}},{"name":"instance-cnac-2","cluster_network_interface":{"id":"7208-cf6023a3-86c7-459f-84b8-536b4f812541"}},{"name":"instance-cnac-3","cluster_network_interface":{"id":"7208-2d55d27c-835c-4566-acbc-da36bcf49da7"}},{"name":"instance-cnac-4","cluster_network_interface":{"id":"7208-161e2919-c505-4e6d-bd49-88e7e0c0f1f3"}},{"name":"instance-cnac-5","cluster_network_interface":{"id":"7208-d700a40e-af61-45ee-b71a-a09203db76bd"}},{"name":"instance-cnac-6","cluster_network_interface":{"id":"7208-c7dbc8b9-6b47-4b83-8649-512e4e8f0a81"}},{"name":"instance-cnac-7","cluster_network_interface":{"id":"7208-f4773e40-49b5-4d44-8c68-7a75513bbf16"}},{"name":"instance-cnac-8","cluster_network_interface":{"id":"7208-46e097b5-c1ea-4669-aafd-7a4cc82d0e02"}}]'
```
{: codeblock}

To create instance `my-ins-2` with a cluster network attachments JSON file:

```sh
ibmcloud is instance-create my-ins-2 deceiving-strode-dimly-undesirable us-south-2 hx3d-160x1002x8h100 test-subnet --image ibm-ubuntu-20-04-6-minimal-amd64-5 --cluster-network-attachments @~/cnac.json
```
{: codeblock}

### Related commands
{: #related-commands-attachments}

* [Creating a cluster network](/docs/vpc?topic=vpc-create-cluster-network&interface=cli)
* [Create a cluster network subnet](/docs/vpc?topic=vpc-create-cluster-network-subnet&interface=cli)
* [Creating a cluster network interface](/docs/vpc?topic=vpc-create-cluster-network-interface&interface=cli)
* [View details of a cluster network](/docs/vpc?topic=vpc-view-details-cluster-network&interface=cli)
* [Delete a cluster network](/docs/vpc?topic=vpc-delete-cluster-network&interface=cli)

## Attaching cluster network interfaces to an instance with the API
{: #attach-interfaces-cluster-network-api}
{: api}

To attach cluster network interfaces to an instance with the API, follow these steps:

1. Set up your API environment [with the right variables](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup).
1. Store any additional variables to be used in the API commands; for example:

   `version` (string): The API version, in format `YYYY-MM-DD`.

1. When all variables are initiated, enter the following command to attach the cluster network interface to the instance:

   ```sh
   curl -X POST "$vpc_api_endpoint/v1/instances/$instance_id/cluster_network_attachments?version=$tomorrow&generation=2&maturity=development" -H "Authorization: Bearer $iam_token" -d '{
     "name": "my-cluster-network-attachment",
     "cluster_network_interface": {
       "id": "cluster_network_interface_id"
     }
   }'
   ```
   {: codeblock}

To view the complete set of cluster network APIs, see the [VPC API reference](/apidocs/vpc-scoped?code=go#list-cluster-network-profiles).

## Attaching cluster network interfaces to an instance with Terraform
{: #attach-interfaces-cluster-network-terraform}
{: terraform}

Terraform will support this feature after it reaches General Availability (GA) and is officially released.
{: note}

The following example provisions an instance cluster network attachment by using Terraform:

```terraform
resource "ibm_is_instance_cluster_network_attachment" "is_instance_cluster_network_attachment_instance" {
  instance = var.is_instance_cluster_network_attachment_instance_id
  before {
    # href = "https://us-south.iaas.cloud.ibm.com/v1/instances/5dd61d72-acaa-47c2-a336-3d849660d010/cluster_network_attachments/0767-fb880975-db45-4459-8548-64e3995ac213" // conflicts with id
    id = "0767-fb880975-db45-4459-8548-64e3995ac213"
  }
  cluster_network_interface {
    # id              = var.is_cluster_network_interface_id // conflicts with other properties
    auto_delete     = var.is_cluster_auto_delete
    name            = var.is_cluster_name
    subnet          = var.is_cluster_subnet_id
    primary_ip {
      address = "192.168.3.4"
      name    = "my-cluster-subnet-reserved-ip-1"
    }
  }
  name = var.is_instance_cluster_network_attachment_name
}
```
{: codeblock}
