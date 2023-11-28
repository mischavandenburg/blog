---
Title: Deploying Grafana Agent With Custom Secrets From Azure Key Vault Using Akv2k8s And K8s-Monitoring Helm Chart
date: 2023-11-28
tags:
- Kubernetes
- Azure
- Grafana
---

Grafana has developed a Helm chart which greatly simplifies the deployment of a monitoring stack to your Kubernetes clusters.

It contains:

* kube-state-metrics, which gathers metrics about Kubernetes objects
* Node exporter, which gathers metrics about Kubernetes nodes
* OpenCost, which interprets the above to create cost metrics for the cluster, and
* Grafana Agent, which scrapes the above services to forward metrics to Prometheus and logs to Loki

The Prometheus and Loki services may be hosted on the same cluster, or remotely (e.g. on Grafana Cloud).

For my current project I'm setting it up to a Grafana Cloud stack, but as stated above it can also be used with a local or other remote Prometheus instance.

# akv2k8s

We use akv2k8s on our clusters to synch secrets from Azure Key Vaults to Kubernetes secrets or injecting them as environment variables. This way we can prevent checking in secrets to our code. 

However, when I first tried to deploy this chart, external secrets were not supported. They have now been fixed by Pete Wall and I managed to deploy the k8s-monitoring by writing a wrapper Helm chart with the external secret objects.

# code

All code is available in my [lab repo](https://github.com/mischavandenburg/lab/tree/main/kubernetes/k8smonitoring-secrets). I wrote the following Chart.yaml:

```yaml
apiVersion: v2
name: k8s-monitoring
version: 1.0.0
description: This chart deploys the k8s-monitoring chart with custom secrets
dependencies:
  - name: k8s-monitoring
    version: 0.5.1
    repository: https://grafana.github.io/helm-charts/
```

Then I created a templates directory and added secrets.yaml:

```yaml
---
apiVersion: spv.no/v1alpha1
kind: AzureKeyVaultSecret
metadata:
  name: prometheus-user
spec:
  vault:
    name: {{ .Values.keyVault }}
    object:
      name: prometheus-user
      type: secret
  output:
    secret:
      name: grafana-agent-credentials-akv2k8s
      dataKey: prometheus-user
---
apiVersion: spv.no/v1alpha1
kind: AzureKeyVaultSecret
metadata:
  name: prometheus-password
spec:
  vault:
    name: {{ .Values.keyVault }}
    object:
      name: prometheus-password
      type: secret
  output:
    secret:
      name: grafana-agent-credentials-akv2k8s
      dataKey: prometheus-password
---
apiVersion: spv.no/v1alpha1
kind: AzureKeyVaultSecret
metadata:
  name: prometheus-host
spec:
  vault:
    name: {{ .Values.keyVault }}
    object:
      name: prometheus-host
      type: secret
  output:
    secret:
      name: grafana-agent-credentials-akv2k8s
      dataKey: prometheus-host
---
```

Then, to use the external secrets in the values file, I used the following configuration:

```yaml
keyVault: kv-123-hello

k8s-monitoring:
  cluster:
    name: aks-123-hello

  externalServices:
    prometheus:
      hostKey: prometheus-host
      basicAuth:
        usernameKey: prometheus-user
        passwordKey: prometheus-password

      secret:
        create: false
        name: grafana-agent-credentials-akv2k8s
        namespace: k8s-monitoring

  extraConfig: |-
    logging {
      level  = "info"
      format = "logfmt"
    }
```

This chart can be deployed using `helm install` but we are using GitOps like the gods intended. When this is configured in ArgoCD, the chart will be deployed with the secrets templates. These templates will retreive the secrets from the Azure Key Vault and synch them to the `grafana-agent-credentials-akv2k8s` secret.

# bug

Althoug this might have been a slight bug during the first implementation of this chart, I did run into the following error in my first attempts. The logs of the Grafana Agent showed:

```bash
unsupported protocol scheme \"\""
```

After a lot of debugging I decided to completely rebuild the deployment from scratch, and then I found out that I had put the externalServices.prometheus and externalServices.secret objects in a different order which messed up the rest of the values file. **It is important to keep the order that is given in the values.yaml in the k8s-monitoring repo.**

## Links:

Helm chart:

https://github.com/grafana/k8s-monitoring-helm/tree/main/charts/k8s-monitoring

GitHub issue:

https://github.com/grafana/k8s-monitoring-helm/issues/81#issuecomment-1828771508

Link to example code in my lab repo:

https://github.com/mischavandenburg/lab/tree/main/kubernetes/k8smonitoring-secrets

202311281011
