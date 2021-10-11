---
title: "Add NodePools"
date: 2021-10-10T19:00:17+02:00
---

## How to add additional node pools to the example cluster

**Prerequisites:**

- An example cluster created using the _How to create a hosted cluster_ instructions

Use the `oc` tool to apply the YAML like the following to create additional node pools for the example hosted cluster:

```yaml
apiVersion: hypershift.openshift.io/v1alpha1
kind: NodePool
metadata:
  namespace: clusters
  name: example-extended
spec:
  clusterName: example
  nodeCount: 1
  platform:
    aws:
      instanceType: m5.large
    type: AWS
  release:
    image: quay.io/openshift-release-dev/ocp-release:4.8.0-rc.0-x86_64
```

With autoscaling:

```yaml
apiVersion: hypershift.openshift.io/v1alpha1
kind: NodePool
metadata:
  namespace: clusters
  name: example-extended
spec:
  clusterName: example
  autoScaling:
    max: 5
    min: 1
  platform:
    aws:
      instanceType: m5.large
    type: AWS
  release:
    image: quay.io/openshift-release-dev/ocp-release:4.8.0-rc.0-x86_64
```