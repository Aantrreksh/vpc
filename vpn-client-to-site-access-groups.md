---

copyright:
  years: 2021, 2022
lastupdated: "2022-07-07"

keywords:

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Creating an IAM access group and granting the role to connect to the VPN server
{: #create-iam-access-group}

FOR USER ID AND PASSCODE AUTHENTICATION ONLY

To create an IAM access group and grant the user role to connect to the VPN server, follow these steps:

1. From the IBM Cloud console, navigate to the [Access groups](https://cloud.ibm.com/iam/groups){: external} page (**Manage > Access (IAM) > Access groups**) and click **Create**.
1. Type a name for your access group and optional description, then click **Create**.
1. Click the **Access** tab, then click **Assign access**.
1. From the drop-down menu, select **VPC Infrastructure Services**. Then, click **Next**. 
1. For Resources, select **All resources**, then click **Next**.
1. For Roles and actions, select **Users of the VPN server need this role to connect to the VPN server**, then click **Review**.
1. Review the Create policy summary and click **Add**.
1. In the Access summary side panel, click **Assign**.
3. Add users to your group.

   * For existing users:
      * Click the **Users** tab, then click **Add users**.
      * Select the checkboxes next to each user that requires VPN Client for VPC access, then click **Add to group**.
   * For new users:
      * Click **Manage > Access (IAM)**, then click **Invite users** in the upper right.
      * Enter the email address of each user that needs to be invited in the "Enter email address" box. Separate emails by commas, spaces, or line breaks. You can enter up to 100 email addresses.
      * In the group table, click the **Add** link next to the IAM group that you just created, then click **Invite**. 
        
        Each user receives an email asking them to join an account in IBM Cloud.
      
For more information, see [Setting up access groups](/docs/account?topic=account-groups). For IAM required permissions and the minimum IAM role to perform a task, see [Client-to-site VPN server tasks](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls#vpn-server-authorizations-required-for-api-and-cli-calls).
{: note}
