---
title: "Create a HostedCluster"
date: 2021-10-10T18:57:17+02:00
---

## How to create a hosted cluster

The `hypershift` CLI tool comes with commands to help create an example hosted cluster. The cluster will come with a node pool consisting of two workers nodes.

**Prerequisites:**

- An OpenShift cluster with HyperShift installed
- Admin access to the OpenShift cluster specified by the `KUBECONFIG` environment variable
- The `hypershift` CLI tool
- The OpenShift `oc` CLI tool.
- A valid [pull secret](https://cloud.redhat.com/openshift/install/aws/installer-provisioned) file for the `quay.io/openshift-release-dev` repository
- A Route53 public zone for the `base-domain`

Run the `hypershift` command to create a cluster named `example` in the `clusters`
namespace, including the cloud infrastructure to support it.

```shell
hypershift create cluster \
  --pull-secret /my/pull-secret \
  --aws-creds ~/.aws/credentials \
  --name example \
  --base-domain hypershift.example.com
```

After a few minutes, check the `hostedclusters` in the `clusters` namespace and when ready it will look similar to
the following:

```shell
$ oc get hostedclusters -n clusters
NAME      VERSION   KUBECONFIG                 AVAILABLE
example   4.7.6     example-admin-kubeconfig   True
```

Also check the `pods` in the `clusters-example` namespace

```shell
$ oc get pods -n clusters-example
NAME                                              READY   STATUS      RESTARTS   AGE
capa-controller-manager-b6b49f696-j9q7l           1/1     Running     0          53m
cluster-api-7fd76bbb57-6kv6z                      1/1     Running     0          53m
cluster-autoscaler-dcc8d8795-llss8                1/1     Running     5          52m
cluster-policy-controller-7c89bc799c-4b6xt        1/1     Running     0          52m
cluster-version-operator-959994fb6-v97w5          1/1     Running     0          52m
control-plane-operator-5cb584b6fc-hnrcn           1/1     Running     0          53m
etcd-l5zk9cp856                                   1/1     Running     0          52m
etcd-operator-68ffbf98fd-wmcrx                    1/1     Running     0          52m
hosted-cluster-config-operator-65b656d96f-cfzqk   1/1     Running     1          52m
kube-apiserver-6c89cfc4dd-t5zfn                   3/3     Running     0          52m
kube-controller-manager-5576c4c8f4-rrxn8          1/1     Running     0          46m
kube-scheduler-776774ff68-fcsrf                   1/1     Running     0          52m
machine-config-server-example-596cdc68fb-5kr4t    1/1     Running     0          53m
manifests-bootstrapper                            0/1     Completed   3          52m
oauth-openshift-75879b669f-grmhf                  1/1     Running     0          51m
openshift-apiserver-766d9c6f79-c442c              1/1     Running     0          51m
openshift-controller-manager-6db5976f8f-4nhgb     1/1     Running     0          52m
openshift-oauth-apiserver-fb9d8c45b-7xzlp         1/1     Running     2          52m
```


Eventually the cluster's kubeconfig will become available and can be printed to
standard out using the `hypershift` CLI:

```shell
hypershift create kubeconfig
```

To delete the cluster and the infrastructure created earlier, run:

```shell
hypershift destroy cluster \
  --aws-creds ~/.aws/credentials \
  --namespace clusters \
  --name example
```