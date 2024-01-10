---
id: announcements
title: "Announcements"
description: "Important announcements including deprecation & removal notices"
---

## Camunda 8.4

Release date: 9th of January 2024

End of maintenance: 9th of July 2025

### Versioning changes in Helm chart

As of the 8.4 release, the Camunda 8 **Helm chart** version is decoupled from the version of the application. The Helm chart release still follows the applications release cycle, but it has an independent version. (e.g., in the application release cycle 8.4, the chart version is 9.0.0).

For more details about the applications version included in the Helm chart, check out the [full version matrix](https://helm.camunda.io/camunda-platform/version-matrix/).

### Dockerfile numeric ID

The Dockerfile now uses a numeric user ID instead of a non-numeric user.
This will allow the Helm users to use `runAsNonRoot=true` without the need to explicitly set the ID in the Helm `values.yaml` file.

### Deprecated in 8.4

The [Zeebe configuration properties for Camunda Identity](../self-managed/zeebe-deployment/configuration/gateway.md#zeebegatewayclustersecurityauthenticationidentity)
were deprecated in `8.4`. Please use the dedicated Camunda Identity properties or the [corresponding environment variables](../self-managed/identity/deployment/configuration-variables.md#core-configuration).

### Versioning changes in Elasticsearch

As of the 8.4 release, Camunda is compatible with Elasticsearch 8.9+ and no longer supports older Elasticsearch versions. See [supported environments](/docs/reference/supported-environments.md).

### Support for Amazon OpenSearch

As of the 8.4 release, Zeebe, Operate, and Tasklist are now compatible with [Amazon OpenSearch](https://aws.amazon.com/de/opensearch-service/) 2.5.x. Note that using Amazon OpenSearch requires [setting up a new Camunda installation](/self-managed/platform-deployment/overview.md). A migration from previous versions or Elasticsearch environments is currently not supported.

:::info
The Helm charts are not yet prepared with the OpenSearch configurations as templates/pre-filled. The Helm charts can still be used to install for OpenSearch, but some adjustments are needed beforehand. Refer to the [Helm deployment documentation](/self-managed/platform-deployment/helm-kubernetes/deploy.md) for further details.
:::

### Known limitations

This release contains the following limitations:

- In **Operate `8.4.0`**
  - **Bug**
    - **Description:** Instance migration always points to the latest process version
    - **Reference:** https://github.com/camunda/issues/issues/567
    - **Mitigation:** Bug is planned to be fixed with upcoming `8.4.1` release
  - **Bug**
    - **Description:** Backwards migration over multiple versions does not work
    - **Reference:** https://github.com/camunda/issues/issues/568
    - **Mitigation:** Bug is planned to be fixed with upcoming `8.4.1` release
- In **Camunda HELM `9.0.0`**
  - **Limitation**
    - **Description:** The existing Helm charts use the Elasticsearch configurations by default and are not yet prepared with the OpenSearch configurations as templates/pre-filled. The Helm charts can still be used to install for OpenSearch, but some adjustments are needed beforehand.
    - **Reference:** n/a
    - **Mitigation:**
      1. Refer to our [docs for the installation](../self-managed/platform-deployment/helm-kubernetes/deploy.md#components-installed-by-the-helm-charts), the docs include guidance about necessary adjustments of the Helm chart configuration.
      2. The OpenSearch configuration in Helm charts will be provided in one of our future Helm releases.
- In **Connectors `8.4.0` and `8.4.1`**
  - **Missing feature**
    - **Description:** Custom OIDC provider support for Connectors is missing
    - **Reference:** https://github.com/camunda/issues/issues/569
    - **Mitigation:**
      1. Feature is planned to be delivered with an upcoming patch release. Please see [issue](https://github.com/camunda/issues/issues/569) for latest progress.
      2. [Disable Connectors component](../self-managed/platform-deployment/helm-kubernetes/guides/connect-to-an-oidc-provider.md#configuration) when configuring a custom OIDC provider.

## Camunda 8.3

Release date: 10th of October 2023

End of maintenance: 9th of April 2025

:::caution
For existing clusters we recommend updating to `8.3.1` directly and not `8.3.0` due to issues in data migration of Operate, Tasklist, and Optimize that could prolong the migration or even blocking it from finishing.
:::

:::caution Breaking change

### Zeebe Docker image now runs with unprivileged user by default

The default user in the Zeebe Docker image changed from root to an unprivileged user with the UID 1000. This was done to provide stronger compliance with the [OWASP recommendations on Docker Security](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html#rule-2-set-a-user).

Please refer to the [Update 8.2 to 8.3](/self-managed/operational-guides/update-guide/820-to-830.md) guide.
:::

:::info
The update from `8.2.x` to `8.3.x` performs a migration for nearly all entities stored in Operate, Tasklist, and Optimize to support [multi-tenancy](/self-managed/concepts/multi-tenancy.md). Therefore, migration may take longer.
:::

### Deprecated in 8.3

[Web Modeler's beta API](/apis-tools/web-modeler-api/index.md) was deprecated in 8.3 and will be removed in 8.5.
Use `v1` instead, see [migration hints](/apis-tools/web-modeler-api/index.md#migrating-from-beta-to-v1).

### Versioning changes in Elasticsearch

As of the 8.3 release, Camunda is compatible with Elasticsearch 8.8+ and no longer supports Elasticsearch 7.x. See [supported environments](/docs/reference/supported-environments.md).

### Versioning changes in Helm chart

[Helm charts versioning](/self-managed/platform-deployment/helm-kubernetes/overview.md) changed in July 2023.

Starting from July 2023 (v8.2.8), the Camunda 8 **Helm chart** version follows the same unified schema
and schedule as [Camunda 8 applications](https://github.com/camunda/camunda-platform).

Before this change, the Camunda 8 **Helm chart** version only followed the minor version.

## Camunda 8.2

Release date: 11th of April 2023

End of maintenance: 8th of October 2024

[Release notes](https://github.com/camunda/camunda-platform/releases/tag/8.2.0)
[Release blog](https://camunda.com/blog/2023/04/camunda-platform-8-2-key-to-scaling-automation/)

### Do not update from Camunda 8.1.X to 8.2.6

An issue in the Operate 8.2.6 patch was discovered after it was published on June 8th.

You should not update directly from 8.1.x to 8.2.6 (it will require manual intervention as indices break), you either first update to 8.2.5 then 8.2.6 or straight from 8.1.x to 8.2.7.

To prevent this entirely we removed the Operate 8.2.6 artifacts from this release.

As Camunda 8.2.7 was already released on Tuesday Jun 13th, you can just update to 8.2.7 directly, skipping 8.2.6.

### OpenSearch 1.3.x support

- Operate version 8.2+ support OpenSearch 1.3.x. However, 8.2.x patches will only be released on the OS 1.3 branch until end of 2023 given that OS 1.3 maintenance period ends by then. We recommend customers to go to 8.4.x which supports OS 2.5+.

### Optimize and Helm chart compatibility

For Optimize 3.10.1, a new environment variable introduced redirection URL. However, the change is not compatible with Camunda Helm charts until it is fixed in 3.10.3 (and Helm chart 8.2.9). Therefore, those versions are coupled to certain Camunda Helm chart versions:

| Optimize version                  | Camunda Helm chart version |
| --------------------------------- | -------------------------- |
| Optimize 3.10.1 & Optimize 3.10.2 | 8.2.0 - 8.2.8              |
| Optimize 3.10.3                   | 8.2.9+                     |

## Camunda 8.1

Release date: 11th of October 2022

End of maintenance: 10th of April 2024

[Release notes](https://github.com/camunda/camunda-platform/releases/tag/8.1.0)
[Release blog](https://camunda.com/blog/2022/10/camunda-platform-8-1-released-whats-new/)

## Camunda 8.0

Release date: 12th of April 2022

End of maintenance: 11th of October 2023

[Release notes](https://github.com/camunda/camunda-platform/releases/tag/8.0.0)
[Release blog](https://camunda.com/blog/2022/04/camunda-platform-8-0-released-whats-new/)

### Camunda 8.0.15 release is skipped

The `Camunda 8.0.15` release pipeline lead to corrupted `Zeebe 8.0.15` artifacts getting published.
The whole [Camunda 8.0.15 release](https://github.com/camunda/camunda-platform/releases/tag/8.0.15) was thus skipped and updates from `Camunda 8.0.14` should go straight to `Camunda 8.0.16`.

### Deprecated in 8.0

The [DeployProcess RPC](/apis-tools/zeebe-api/gateway-service.md#deployprocess-rpc) was deprecated in 8.0.
It is replaced by the [DeployResource RPC](/apis-tools/zeebe-api/gateway-service.md#deployresource-rpc).

## Camunda Cloud 1.3

Release date: 11th of January 2022

Camunda Cloud is out of maintenance.

### Deprecated in 1.3

The `zeebe-test` module was deprecated in 1.3.0. We are currently planning to remove `zeebe-test` for the 1.4.0 release.

## Camunda Cloud 1.2

Release date: 12th of October 2021

Camunda Cloud is out of maintenance.

## Camunda Cloud 1.1

Release date: 13th of July 2021

Camunda Cloud is out of maintenance.

## Camunda Cloud 1.0

Release date: 11th of May 2021

Camunda Cloud is out of maintenance.

### Removed in 1.0

The support for YAML processes was removed as of release 1.0. The `resourceType` in Deployment record and Process grpc request are deprecated; they will always contain `BPMN` as value.

## Zeebe 0.26.0

### Deprecated in 0.26.0

#### YAML workflows descriptions

YAML workflows are an alternative way to specify simple workflows using a proprietary YAML description. This feature is deprecated and no longer advertised in the documentation. YAML workflows gained little traction with users and we do not intend to support them in the future.

We recommend all users of YAML workflows to migrate to BPMN workflows as soon as possible. The feature will eventually be removed completely, though the date when this will occur has yet to be defined.

## Zeebe 0.23.0-alpha2

### Deprecated in 0.23.0-alpha2

- TOML configuration - deprecated and removed in 0.23.0-alpha2
- Legacy environment variables - deprecated in 0.23.0-alpha2, removed in 0.25.0

New configuration:

```yaml
exporters:
  elasticsearch:
    className: io.camunda.zeebe.exporter.ElasticsearchExporter
  debughttp:
    className: io.camunda.zeebe.broker.exporter.debug.DebugHttpExporter
```

In terms of specifying values, there were two minor changes:

- Memory sizes are now specified like this: `512MB` (old way: `512M`)
- Durations (e.g. timeouts) can now also be given in ISO-8601 Durations format. However, you can still use the established method and specify a timeout of `30s`