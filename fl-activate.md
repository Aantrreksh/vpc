---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-23"

keywords: flow logs, activate, deactivate, suspend, resume

subcollection: vpc
---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:term: .term}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:external: target="_blank_" .external}
{:generic: data-hd-programlang="generic"}
{:download: .download}
{:DomainName: data-hd-keyref="DomainName"}

# Suspending and resuming a flow log collector
{: #managing-flow-log-collectors_activate}

You can suspend and resume a flow log collector by using the UI, the CLI, or the API.

After you create a flow log collector, its default state is `Active`.
{: note}

## Using the UI
{: #fl-managing-ui}

To suspend an `Active` flow log collector, click the overflow menu ![overflow menu](images/overflow.png) and select **Suspend**. Suspending the flow log stops the flow log from writing to the Cloud Object Storage (COS) bucket.  

![Suspend](/images/flow-log-suspend.png)

To resume a suspended flow log, select **Resume** from the overflow menu ![overflow menu](images/overflow.png).

![Resume](/images/flow-log-resume.png)

## Using the CLI
{: #fl-managing-cli}

To suspend or resume a flow log collector by using the CLI, you must pass a **true** or **false** value to the **--active** flag on the **flow-log-update** command.

```
ibmcloud is flow-log-update FLOW_LOG --active (true|false) [--json]
```

Where:

* **FLOW_LOG** is the ID of the flow log instance.
* **--active** is the intended `active` status after the update. Set to **true** to resume or **false** to suspend.
* **--json** formats the output in JSON.

## Using the API
{: #fl-managing-api}

To suspend and resume flow log collectors by using the API, follow these steps:

1. Set up your [API environment](/docs/vpc?topic=vpc-set-up-environment#api-prerequisites-setup) with
the right variables.
2. Store your `FlowLogID01` in a variable to be used in the API command:
    ```sh
    export FlowLogID01="<your_flow_log_id>"
    ```
    {:pre}
3. Choose from the following:

   * To suspend a flow logs collector:

      ```sh
      curl -s -X PATCH \
        "$vpc_api_endpoint/v1/flow_log_collectors/$FlowLogID01?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{ "active": false }' | jq
      ```
      {:pre}

   * To resume a flow log collector's operation:

      ```sh
      curl -s -X PATCH \
        "$vpc_api_endpoint/v1/flow_log_collectors/$FlowLogID01?version=$api_version&generation=2" \
        -H "Authorization: $iam_token" \
        -d '{ "active": true }' | jq
      ```      
      {:pre}
