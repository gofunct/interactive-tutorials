Kubeflow extends Kubernetes with custom resource definitions (CRD) and operators. Each custom resource is designed to support the deployment of machine learning workloads. When a resource is defined, the operator will process the deployment request.

It's possible to view the resources available by running:

`kubectl get crd`{{execute}}

In 0.2.5, the PyTorch operator is not deployed by default. The following commands will deploy custom resource and operator.

```
cd kubeflow_ks_app
ks generate pytorch-operator pytorch-operator
ks apply default -c pytorch-operator
```{{execute}}

You should see the additional PyTorch custom resource definitions.

`kubectl get crd`{{execute}}

The next step will deploy a train a PyTorch using the above resources. 