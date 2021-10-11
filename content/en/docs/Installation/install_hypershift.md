---
title: "Install HyperShift"
date: 2021-10-10T18:57:17+02:00
---

## How to install HyperShift

HyperShift is deployed into an existing OpenShift cluster which will host the managed control planes.

**Prerequisites:**

* Admin access to an OpenShift cluster (version 4.7+) specified by the `KUBECONFIG` environment variable
* The OpenShift `oc` CLI tool
* The `hypershift` CLI tool
- An [AWS credentials file](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html) with permissions to create infrastructure for the cluster

Install HyperShift into the management cluster:

```shell
hypershift install
```

To uninstall HyperShift, run:

```shell
hypershift install --render | oc delete -f -
```