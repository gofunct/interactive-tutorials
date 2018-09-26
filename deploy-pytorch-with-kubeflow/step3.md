The Distributed MNIST Model has been packaged into a Container Image. The Python PyTorch code can be viewed on [Github](https://github.com/kubeflow/pytorch-operator/blob/9605eb6783e3549654082ea4b18a9cb0391e8548/examples/dist-mnist/dist_mnist.py)

To deploy the training model, a `PyTorchJob` is required. This defines the Container Image to use and the number of replicas to use to distribute the training.

An example can be viewed with `cat ~/example.yaml`{{execute}}

This is deployed via Kubectl, the Kubernetes CLI.

`kubectl create -f ~/example.yaml`{{execute}} 

The Kubeflow PyTorch Operator and Kubernetes will schedule the workload and start the required number of replicas. You can view the status with `kubectl get pods -l pytorch_job_name=distributed-mnist`{{execute}}

You should see one master and three workers created. The next step will discuss how to view the results.