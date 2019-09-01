---
layout: page
title: Helm action
group: Delivering
---

# Helm action

Deploys a helm chart using GitHub actions. Supports canary deployments and
provides a built in helm chart for apps that listen over http to get your ramped
up quickly.

## Example

```yaml
# .github/workflows/deploy.yml
name: Deploy
on: ['deployment']

jobs:
  deployment:
    runs-on: 'ubuntu-latest'
    steps:
    - uses: actions/checkout@v1

    - name: 'Deploy'
      uses: 'deliverybot/helm@v1'
      with:
        release: 'nginx'
        namespace: 'default'
        chart: 'app'
        token: '${{ github.token }}'
        values: |
          name: foobar
      env:
        KUBECONFIG_FILE: '${{ secrets.KUBECONFIG }}'
```

## Parameters

### Inputs

Inputs labelled below with "(deployment)" are additionally loaded from the
payload of the deployment resource.

- `release`: Helm release name. Will be combined with track if set. (required)
  (deployment)
- `namespace`: Kubernetes namespace name. (required) (deployment)
- `chart`: Helm chart path. If set to "app" this will use the built in helm
  chart found in this repository. (required) (deployment)
- `values`: Helm chart values, expected to be a YAML or JSON string.
  (deployment)
- `track`: Track for the deployment. If the track is not "stable" it activates
  the canary workflow described below. (deployment)
- `task`: Task name. If the task is "remove" it will remove the configured helm
  release. (deployment)
- `dry-run`: Helm dry-run option.
- `token`: Github repository token. If included and the event is a deployment
  then the deployment_status event will be fired.

Additional parameters: If the action is being triggered by a deployment event
and the `task` parameter in the deployment event is set to `"remove"` then this
action will execute a `helm delete $service`

### Environment

- `KUBECONFIG_FILE`: Kubeconfig file for Kubernetes cluster access.

## Examples

### Canary deployments

If a track is chosen that is not equal to stable, this updates the helm chart
in a few ways:

1. Release name is changed to `{release}-{track}` (eg. myapp-canary).
2. The service is disabled on the helm chart `service.enabled=false`
3. The ingress is disabled on the helm chart `ingress.enabled=false`

Not enabling the service or ingress allows the stable ingress and service
resources to pick up the canary pods and route traffic to them.

```yaml
# .github/workflows/deploy.yml
name: Deploy
on: ['deployment']

jobs:
  deployment:
    runs-on: 'ubuntu-latest'
    steps:
    - uses: actions/checkout@v1

    - name: 'Deploy'
      uses: 'deliverybot/helm@v1'
      with:
        release: 'nginx'
        track: canary
        namespace: 'default'
        chart: 'app'
        token: '${{ github.token }}'
        values: |
          name: foobar
      env:
        KUBECONFIG_FILE: '${{ secrets.KUBECONFIG }}'
```
