---

copyright:
  years: 2021
lastupdated: "2021-08-26"

keywords:  network, encryption, client VPN, server VPN

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Configuring security groups and ACLs for use with VPN (Beta)
{: #vpn-client-to-site-security-groups}  

Client VPN for VPC is available to all IBM Cloud users. After the Beta period ends, you will be given a time period to migrate your VPN servers to the standard pricing plan to avoid disruption of service.
{: beta}

Security groups and access control lists (ACLs) can be configured on the VPN server's subnet where the VPN server is deployed, as well as on other VPC subnets that communicate over the VPN tunnel.
{: shortdesc}

If you configure security groups and ACLs on the VPN server's subnet, make sure that the following rules are in place to allow VPN tunnel traffic. For more information, see [About security groups](/docs/vpc?topic=vpc-using-security-groups) and [Setting up network ACLs](/docs/vpc?topic=vpc-using-acls).

If you don't specify a security group when you provision the VPN server, the default security group of the VPC is attached to the VPN server. Keep in mind that you cannot change the attached security group after a VPN server is provisioned, but you can change the rules in the security group.  
{: note}

## Rules for VPN protocol traffic
{: #rules-vpn-protocol-traffic}

VPN servers can run on TCP or UDP protocols. You can specify the protocol and port when you provision the VPN server. You must open the corresponding protocol and port with the security group rules and ACL.

|Inbound/Outbound Rules| Protocol | Source/Destination Type | Source | Value |
|-----------|-----------|------|------|-------|
|inbound| `VPN server protocol`| CIDR block | `0.0.0.0/0` | `VPN server port` |
|outbound| `VPN server protocol`| CIDR block | `0.0.0.0/0` | `VPN server port` |
{: caption="Table 1. Security group rules for a VPN server" caption-side="bottom"}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| inbound | `VPN server protocol` | Any | Any | `VPN server subnet` | `VPN server port`|
| outbound | `VPN server protocol`  | `VPN server subnet` | `VPN server port` | Any | Any |
{: caption="Table 2. ACL rules for a VPN server" caption-side="bottom"}

For example, by default, the VPN server runs on UDP port `443`, so you should configure the security group and ACL rules as follows:

|Inbound/Outbound Rules| Protocol | Source/Destination Type | Source | Value |
|-----------|-----------|------|------|-------|
|inbound| UDP| CIDR block | `0.0.0.0/0` | `443` |
|outbound| UDP| CIDR block | `0.0.0.0/0` | `443` |
{: caption="Table 3. Configuration information for security group rules of a VPN server with default protocol and port" caption-side="bottom"}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| inbound | UDP | Any | Any | `VPN server subnet` | `443` |
| outbound | UDP  | `VPN server subnet` | `443` | Any | Any |
{: caption="Table 4. Configuration information for ACL of a VPN server with default protocol and port" caption-side="bottom"}

## Rules for VPN traffic with deliver route
{: #rules-vpn-traffic-with-deliver-route}

When the client is connected to the VPN server, the VPN server drops all traffic from the client by default. You have to configure the VPN route on the VPN server to allow this traffic. The action of the route may be either `deliver` or `translate`.

* When the action is `deliver`, after the packet is decrypted from the tunnel, the packet is forwarded directly, so the source IP of the packet is the VPN client IP, which ifrom the client IP pool.
* When the action is `translate`, after the packet is decrypted from the tunnel, the source IP of the packet is translated to the private IP of the VPN server, then the packet is sent.

Note which CIDR should be specified when you create the security group and ACL rules.

|Inbound/Outbound Rules| Protocol | Source/Destination Type | Source | Value |
|-----------|-----------|------|------|-------|
|inbound| ALL| CIDR block | `VPN route destination CIDR` | Any |
|outbound| ALL| CIDR block | `VPN route destination CIDR` | Any |
{: caption="Table 5. Security group rules for a VPN deliver route on the VPN server" caption-side="bottom"}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| inbound | ALL | `VPN route destination CIDR` | Any | `VPN server client IP pool` | Any|
| outbound | ALL  | `VPN server client IP pool` | Any | `VPN route destination CIDR` | Any |
{: caption="Table 6. ACL rules for a VPN deliver route on the VPN server subnet" caption-side="bottom"}

At the same time, you need to configure the security group and ACL on the VPC VSI to unblock traffic from the VPN client:

|Inbound/Outbound Rules| Protocol | Source/Destination Type | Source | Value |
|-----------|-----------|------|------|-------|
| inbound | ALL | CIDR block | `VPN server client IP pool` | Any |
| outbound | ALL | CIDR block | `VPN server client IP pool` | Any |
{: caption="Table 7. Security group rules for a VPN deliver route on the VPC VSI" caption-side="bottom"}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| inbound | ALL | `VPN server client IP pool` | Any | `VPN route destination CIDR` | Any|
| outbound | ALL  | `VPN route destination CIDR` | Any | `VPN server client IP pool` | Any |
{: caption="Table 8. ACL rules for a VPN deliver route on the VPC VSI subnet" caption-side="bottom"}

For example, if you use `172.16.0.0/20` as the client IP pool when you provision the VPN server, and you want to access the resources of subnet `10.240.128.0/24` from the VPN client, you need to create a VPN route with destination:`10.240.128.0/24`, and action: `deliver`. You should also configure security group and ACL rules as follows:

|Inbound/Outbound Rules| Protocol | Source/Destination Type | Source | Value |
|-----------|-----------|------|------|-------|
|inbound| ALL| CIDR block | `10.240.128.0/24` | Any |
|outbound| ALL| CIDR block | `10.240.128.0/24` | Any |
{: caption="Table 9. Security group rules for a VPN deliver route on the VPN server" caption-side="bottom"}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| inbound | ALL | `10.240.128.0/24` | Any | `172.16.0.0/20` | Any|
| outbound | ALL | `172.16.0.0/20` | Any | `10.240.128.0/24` | Any |
{: caption="Table 10. ACL rules for a VPN deliver route on the VPN server subnet" caption-side="bottom"}

|Inbound/Outbound Rules| Protocol | Source/Destination Type | Source | Value |
|-----------|-----------|------|------|-------|
|inbound| ALL| CIDR block | `172.16.0.0/20` | Any |
|outbound| ALL| CIDR block | `172.16.0.0/20` | Any |
{: caption="Table 11. Security group rules for a VPN deliver route on the VPC VSI" caption-side="bottom"}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| inbound | ALL | `172.16.0.0/20` | Any | `10.240.128.0/24` | Any|
| outbound | ALL  | `10.240.128.0/24` | Any | `172.16.0.0/20` | Any |
{: caption="Table 12. ACL rules for a VPN deliver route on the VPC VSI subnet" caption-side="bottom"}

## Rules for VPN traffic with translate route
{: #rules-vpn-traffic-with-translate-route}

The rules for the VPN translate route are almost the same as the deliver route. The only difference is the source IP of the packet is translated to the VPN server's private IP. So you need to use the VPN server's subnet instead of the VPN server's client IP pool.

If you select multiple subnets when you provision the VPN server, you must include all subnets in the security group and ACL rules.
{: important}

|Inbound/Outbound Rules| Protocol | Source/Destination Type | Source | Value |
|-----------|-----------|------|------|-------|
|inbound| ALL| CIDR block | `VPN route destination CIDR` | Any |
|outbound| ALL| CIDR block | `VPN route destination CIDR` | Any |
{: caption="Table 13. Security group rules for a VPN translate route on the VPN server" caption-side="bottom"}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| inbound | ALL | `VPN route destination CIDR` | Any | `VPN server subnet CIDR` | Any|
| outbound | ALL  | `VPN server subnet CIDR` | Any | `VPN route destination CIDR` | Any |
{: caption="Table 14. ACL rules for a VPN translate route on the VPN server subnet" caption-side="bottom"}

Normally, the destination of the VPN translate route is out of the VPC. For example, you want to use a VPN server to access the IBM classic infrastructure VSI. At the same time, you need to configure firewall rules on the destination to unblock traffic from the VPN client.

For example, if you use the subnets `10.240.64.0/24` and `10.240.129.0/24` when provisioning the VPN server, there is a subnet `10.187.190.0/26` in IBM classic infrastructure, and you want the access classic VSI from the VPN client, then you need to create a VPN route with destination: `10.187.190.0/26`, action: `translate` and configure security group and ACL rules as follows:

|Inbound/Outbound Rules| Protocol | Source/Destination Type | Source | Value |
|-----------|-----------|------|------|-------|
| inbound | ALL| CIDR block | `10.187.190.0/26` | Any |
| outbound | ALL| CIDR block | `10.187.190.0/26` | Any |
{: caption="Table 15. Security group rules for a VPN translate route on the VPN server" caption-side="bottom"}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| inbound | ALL | `10.187.190.0/26` | Any | `10.240.64.0/24` | Any|
| inbound | ALL | `10.187.190.0/26` | Any | `10.240.129.0/24` | Any|
| outbound | ALL  | `10.240.64.0/24` | Any | `10.187.190.0/26` | Any |
| outbound | ALL  | `10.240.129.0/24` | Any | `10.187.190.0/26` | Any |
{: caption="Table 16. ACL rules for a VPN translate route on the VPN server subnet" caption-side="bottom"}

| Inbound/Outbound Rules | Protocol | Source IP | Source Port | Destination IP | Destination Port |
|--------------|------|------|------|------|------------------|
| inbound | ALL | `10.240.64.0/24` | Any | `10.187.190.0/26` | Any|
| inbound | ALL | `10.240.129.0/24` | Any | `10.187.190.0/26` | Any|
| outbound | ALL  | `10.187.190.0/26` | Any | `10.240.64.0/24` | Any |
| outbound | ALL  | `10.187.190.0/26` | Any | `10.240.129.0/24` | Any |
{: caption="Table 17. Firewall rules for a VPN translate route on the target firewall device (optional)" caption-side="bottom"}
