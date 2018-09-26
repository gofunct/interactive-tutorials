TfJob provides a Kubeflow custom resource that makes it easy to run distributed or non-distributed TensorFlow jobs on Kubernetes. The TFJob controller takes a YAML specification for a master, parameter servers, and workers to help run distributed computation.

A Custom Resource Definition (CRD) provides the ability to create and manage TF Jobs in the same fashion as built-in Kubernetes resources. Once deployed, the CRD can configure the TensorFlow job, allowing users to focus on machine learning instead of infrastructure.

## Create TFJob Deployment Definition

To deploy the TensorFlow workload described in the previous step, Kubeflow needs a TFJob definition. In this scenario, you can view it by running `cat ~/example.yaml`{{execute}}

The definition defines three components:

__Master__: Each job must have one master. The master will coordinate training operations execution between workers.

__Worker__: A job can have 0 to N workers. Each worker process runs the same model, providing parameters for processing to a Parameter Server.

__PS__: A job can have 0 to N parameter servers. Parameter server enables you to scale your model across multiple machines.

More information can be found at https://www.tensorflow.org/deploy/distributed

## Deploying TfJob

The TFJob can be deployed by running `kubectl apply -f ~/example.yaml && kubectl get tfjob && kubectl get pods`{{execute}}

By deploying the job, Kubernetes will schedule the workloads for execution across the available nodes. As part of the deployment, Kubeflow will configure TensorFlow with the required settings allowing the different components to communicate.

View the progress of the Tensorflow job by following the logs of the master.
`kubectl get pods --no-headers -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}' | grep master | xargs kubectl logs -f`{{execute}}


A cleaner approach is planned for a future release of Kubeflow. Follow the latest at https://github.com/kubeflow/tf-operator/issues/536
