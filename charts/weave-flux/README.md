# Weave Flux OSS

Flux is a tool that automatically ensures that the state of a cluster matches what is specified in version control.
It is most useful when used as a deployment tool at the end of a Continuous Delivery pipeline. Flux will make sure that your new container images and config changes are propagated to the cluster.

## Introduction

This chart bootstraps an [Weave Flux](https://github.com/weaveworks/flux) deployment on 
a [Kubernetes](http://kubernetes.io) cluster using the [Helm](https://helm.sh) package manager.

## Prerequisites

- Kubernetes 1.7+

## Installing the Chart

To install the chart with the release name `cd`:

```console
$ helm install --name cd \
--set git.url=git@github.com:weaveworks/flux-example
stable/weave-flux
```

The [configuration](#configuration) section lists the parameters that can be configured during installation.

At startup Flux generates a SSH key and stores it on the git-deploy secret. 
Find the SSH public key in Flux logs with:

```bash
POD_NAME=$(kubectl get pods --namespace default -l "app=weave-flux,release=cd" -o jsonpath="{.items[0].metadata.name}")
kubectl logs $POD_NAME | grep identity.pub | cut -d '"' -f2 | sed 's/.\{2\}$//'
```

Copy the public key and use it to create a deploy key with write access on your GitHub repository.

## Uninstalling the Chart

To uninstall/delete the `cd` deployment:

```console
$ helm delete --purge cd
```

The command removes all the Kubernetes components associated with the chart and deletes the release.

## Configuration

The following tables lists the configurable parameters of the Weave Flux chart and their default values.

| Parameter                       | Description                                | Default                                                    |
| ------------------------------- | ------------------------------------------ | ---------------------------------------------------------- |
| `image` | Image | `quay.io/weaveworks/flux` 
| `imageTag` | Image tag | `1.2.5` 
| `imagePullPolicy` | Image pull policy | `IfNotPresent` 
| `resources` | CPU/memory resource requests/limits | None 
| `rbac.create` | If `true`, create and use RBAC resources | `true`
| `serviceAccount.create` | If `true`, create a new service account | `true`
| `serviceAccount.name` | Service account to be used | `weave-flux`
| `service.type` | Service type to be used | `ClusterIP`
| `git.url` | URL of git repo with Kubernetes manifests | None
| `git.branch` | Branch of git repo to use for Kubernetes manifests | `master`
| `git.path` | Path within git repo to locate Kubernetes manifests (relative path) | None
| `git.user` | Username to use as git committer | `Weave Flux`
| `git.email` | Email to use as git committer | `support@weave.works`

