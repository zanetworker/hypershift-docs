---
title: "Install CLI"
date: 2021-10-10T18:57:17+02:00
---

## How to install the HyperShift CLI

The `hypershift` CLI tool helps you install and work with HyperShift.

**Prerequisites:**

* Go 1.16

Install the `hypershift` CLI using Go:

```shell
go install github.com/openshift/hypershift@latest
```

The `hypershift` tool will be installed to `$GOBIN/hypershift`.


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
## Troubleshooting

**Pull Secret Issues**

- If you run into an issue where the `pods` are not creating properly when you
  issue the `hypershift create cluster ...` command, it may be your `pull-secret`. There may be a
  typo or a bad copy/paste that has left the `pull-secret` malformed. Make sure the `pull-secret` is accurate and exists
  on your system and you properly pass in the path to the file. A proper representation of the `pull-secret`
  will look like the following in this example:

```shell
$ oc get secret example-pull-secret -n clusters
NAME                  TYPE     DATA   AGE
example-pull-secret   Opaque   1      74m

$ oc get secret pull-secret -n clusters-example
NAME          TYPE                             DATA   AGE
pull-secret   kubernetes.io/dockerconfigjson   1      74m

```

- If you do not see the `pull-secret` above and see an error similar to below, then you most likely have a malformed
  `pull-secret` that you passed into the `hypershift create cluster...` command:

```shell
$ oc logs deploy/control-plane-operator -n clusters-example

...
"error": "failed to ensure control plane: failed to get pull secret pull-secret: secrets \"pull-secret\" not found"}
...
```

- If your `pull-secret` was not properly created and you issue the `hypershift destroy cluster ...` command, it does not
  clean up the `clusters/secrets` - so if that was malformed or not accurate, you will need to fix and manually delete
  that secret before you rerun the `hypershift create cluster ...` command.

```shell
$ hypershift destroy cluster   --aws-creds ~/.aws/credentials   --namespace clusters   --name example

$ oc delete secret example-pull-secret -n clusters

$ hypershift create cluster --pull-secret ~/pull-secret \
      --aws-creds ~/.aws/credentials \
      --name example \
      --base-domain yourroute53domain
```
