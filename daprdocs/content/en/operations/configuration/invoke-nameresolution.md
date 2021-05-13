---
type: docs
title: "Service invocation name resolution providers"
linkTitle: "Service invocation name resolution"
weight: 4050
description: "Learn how to swap out the name resolution provider to be able to invoke services across platforms and systems."
---

Dapr uses a **name resolution component** in order to discover your Dapr sidecars and applications, and allow you to invoke them using the [service invocation building block]({{< ref service-invocation-overview.md >}}).

<img src="/images/service-invocation-overview-diagram.png" width="700" alt="Diagram showing service invocation flow">

## Name resolution components

Dapr offers mutiple name resolution components to provide service discovery for sidecars and service invocation. Each [hosting platform]({{< ref hosting >}}) has a default name resolution provider, but it can be overridden with any other provider via the [Dapr config]({{< ref configuration-overview.md >}}).

### Default providers

| Platform | Default name resolution provider | Details |
|----------|----------------------------------|---------|
| [Self-hosted]({{< ref self-hosted-overview.md >}}) | mDNS | Provides service invocation for all applications running on the same Windows/macOS/Linux machine
| [Kubernetes]({{< ref kubernetes-overview.md >}}) | Kubernetes DNS | Provides service invocation for all applications running on the same Kubernetes cluster.

### Additional providers

For a full list of additional name resolution providers you can use within Dapr, visit the name resolution specs page.

## Defining a name resolution provider

{{% alert title="Note" color="primary" %}}
Dapr has default name resolution providers that are configured out of the box without any additional configuration. Only use the following steps to override these defaults.
{{% /alert %}}

{{< tabs Self-Hosted Kubernetes >}}

{{% codetab %}}
1. Open your Dapr config file that was initialized during `dapr init`:
   - Linux/macOS: `~/.dapr/config.yaml`
   - Windows: `%USERPROFILE%\.dapr\config.yaml`
   - Optionally you can create a new `config.yaml` file within your app directory that overrides your the config
1. Add a `nameresolution` entry under `spec`, and fill in the configuration keys defined in the component spec
   ```yaml
   apiVersion: dapr.io/v1alpha1
   kind: Configuration
   metadata:
     name: appconfig
   spec:
     nameResolution:
       component: "COMPONENT_NAME"
       configuration:
         key1: value1
         ...
   ```
1. Save your file and use `dapr run` to launch your app and the Dapr sidecar with the defined name resolution component
   - If you created an app-specific `config.yaml` use the `--config` flag and specify a config file
1. Invoke services using the Dapr [service invocation API]({{< ref dapr-invoke.md >}})
{{% /codetab %}}

{{% codetab %}}
1. Create a file named `config.yaml`
1. Add a `nameresolution` entry under `spec`, and fill in the configuration keys defined in the component spec
   ```yaml
   apiVersion: dapr.io/v1alpha1
   kind: Configuration
   metadata:
     name: appconfig
   spec:
     nameResolution:
       component: "COMPONENT_NAME"
       configuration:
         key1: value1
         ...
   ```
1. Save your file and use `kubectl apply ./config.yaml` to deploy your configuration to your Kubernetes cluster
1. Invoke services using the Dapr [service invocation API]({{< ref dapr-invoke.md >}})
{{% /codetab %}}

{{< /tabs >}}