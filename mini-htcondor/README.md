# Self-Contained HTCondor Helm Chart

This folder contains a Kubernetes Helm chart used for deploying HTCondor central manager, workers and a submit node within the same cluster.

## Chart Details

This chart will do the following:

* Start up the Central Manager
* Instantiate workers to connect to the local CM
* Create a submit node from which one can submit jobs

## Installing the Chart

To install the chart, use the following:

```console
~$ helm install htcondor .
```

## Configuration

The table below lists all configurable parameters for mini-htcondor chart along with their default values. Pass each parameter with ```--set key=value,[,key=value]``` to ```helm install``` to customise execution.

| Parameter                  | Description | Default |
|:---------------------------|:------------|:--------|
| `images.pullPolicy`        | Container pull policy | IfNotPresent |
| `images.cm.image`          | Central Manager container image | "htcondor/cm" |
| `images.cm.tag`            | CM image tag override. Default: .Chart.AppVersion | "" |
| `images.cm.port`           | Port CM listens on | 9618 |
| `images.worker.image`      | Execute node container image | "htcondor/execute" |
| `images.worker.tag`        | Worker image tag override. Default: .Chart.AppVersion | "" |
| `images.submit.image`      | Submit node container image | "htcondor/submit" |
| `images.submit.tag`        | submit image tag override. Default: .Chart.AppVersion | "" |
| `cm.enabled`               | Install the Central Manager if enabled | true |
| `worker.enabled`           | Install worker nodes if enabled | true |
| `worker.replicas`          | Number of workers to create | 2 |
| `submit.enabled`           | Install a submit not if enabled | true |
| `podSecurityContext`       | Pod security context | {} |
| `securityContext`          | Security context | {} |
| `nodeSelector`             | Node labels for assignment | {} |
| `tolerations`              | Tolerations for pod assignment | {} |
| `resources`                | Registry containers resource requests and limits | See `value.yaml` for details |
| `affinity`                 | Affinity for pod assignment | {} |
