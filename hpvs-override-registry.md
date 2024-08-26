---

copyright:
  years: 2022, 2023
lastupdated: "2023-11-07"

keywords: dynamic registry reference for hyper protect virtual server for vpc

subcollection: vpc

---

{{site.data.keyword.attribute-definition-list}}

# Using a dynamic registry reference
{: #hyper-protect-virtual-server-use-dynamic-registry-reference}

The documentation walks you through how to use a dynamic registry reference in the [contract](/docs/vpc?topic=vpc-about-contract_se).
{: shortdesc}



## Explicit registry reference
{: #explicit-registry-reference}

Typically, the docker registry is referenced via the full docker URL in the compose file. For example,

```yaml
services:
  helloworld:
    image: docker.io/library/hello-world@sha256:53f1bbee2f52c39e41682ee1d388285290c5c8a76cc92b42687eecf38e0af3f0
```
{: codeblock}

In the example, `docker.io/library/` is the registry prefix, `hello-world` is the identifier of the OCI image in that registry, and `sha256:53f1bbee2f52c39e41682ee1d388285290c5c8a76cc92b42687eecf38e0af3f0` is the unique identifier of the version of the image.

In such a case, the role that decides about the registry (and the associated pull credentials) is the workload provider, since both the registry reference as well as the pull credentials are part of the workload section of the contract.

## Dynamic registry reference
{: #dynamic-registry-reference}

There exist usecases in which the registry is **not known** when the workload section is pre-encrypted, for example, when the workload provider wants to allow the deployer to use a registry mirror or a private container registry.

In such a case, it's possible to dynamically override the registry as well as the pull credentials. This is a coordinated effort between the workload provider and the deployer.

The templating approach only works for a [compose](/docs/vpc?topic=vpc-about-contract_se#hpcr_contract_compose)-based workload and also only for images referenced via a digest (no support for DCT-based workloads).
{: note}

### Workload provider
{: #workload-provider}

The workload provider marks the registry as dynamic by using a replacement variable in the docker compose file:

```yaml
services:
  helloworld:
    image: ${REGISTRY}/hpse-docker-hello-world-s390x@sha256:43c500c5f85fc450060b804851992314778e35cadff03cb63042f593687b7347

```
{: codeblock}

Note that the digest of the image is identical across registries, so the workload provider can lock down the desired version of the image by setting the key, independent of what registry is actually being used. Using tokens in the compose file is a native feature of the [compose specification](https://docs.docker.com/reference/compose-file/#interpolation){: external}.

Now the workload provider can prepare (encrypt) the workload section **without** specifying the pull secrets for that registry.

### Deployer
{: #deployer}

The deployer fills in the missing information about the registry and the associated pull secrets.

The registry is set as an environment variable. Both the deployer and the workload provider can provide pieces to the overall environment and these are overlaid (with workload taking precedence).

The pull credentials are passed in via an `auth` section in the environment part of the contract. Just as environment variables, these sections are overlaid with the workload section taking precedence.

```yaml
---
  ---
  env:
    type: env
    auths:
      de.icr.io:
        username: xxx
        password: yyy
    env:
      REGISTRY: de.icr.io
```
{: codeblock}
