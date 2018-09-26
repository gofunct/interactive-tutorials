After deploying Kubeflow, the next step is to train the model.

Depending on the size of the dataset, training models with Kubeflow can take some hours. The steps below will describe how to start training the model, but you will not be expected to wait for it to finish. Instead, the next step in the scenario will use a pre-trained model.

## Clone Project

The project is available at https://github.com/kubeflow/examples/tree/master/github_issue_summarization. This contains the Ksonnet templates to describe how to train and serve the model.

Clone the repository using `git clone https://github.com/katacoda/kubeflow-examples.git examples; cd examples/github_issue_summarization/ks-kubeflow`{{execute}}

## Train Using PersistentVolumeClaim (PVC)

Data for the training can be stored in different locations based on requirements, such as a PVC or a Google Storage Bucket. 

In this case the training will be based on data stored in a local PVC. Download the training data with the following jobs:

`
ks env add tfjob-run1
ks env set tfjob-run1
ks apply tfjob-run1 -c data-pvc
ks apply tfjob-run1 -c data-downloader
ks apply tfjob-run1 -c tfjob-pvc-v1alpha2
`{{execute}}

View the progress of the download with:

`kubectl get pods -l job-name`{{execute}}

`kubectl logs $(kubectl get pods -l job-name -o=jsonpath='{.items[0].metadata.name}')`{{execute}}

Once an environment has been created, the next step is to configure the TFJob to launch the trained model. The Tensorflow code has been packaged as a Docker Image called _gcr.io/agwl-kubeflow/tf-job-issue-summarization_. You can view the code at https://github.com/kubeflow/examples/blob/master/github_issue_summarization/notebooks/train.py.

The following will define the Container Image to use and the sample size for the training.

```
ks param set tfjob namespace default
ks param set tfjob image "gcr.io/agwl-kubeflow/tf-job-issue-summarization:latest"
ks param set tfjob sample_size 100000 
```{{execute}}

`ks apply tfjob-run1 -c tfjob-v1alpha2`{{execute}}

This is deployed as a TFJob.

`kubectl get tfjob`{{execute}}

You can view the status of the deployment with:

```
kubectl get pods -ltf_job_key=tfjob-issue-summarization
```{{execute}}

The logs can be accessed via:

```
kubectl logs -f $(kubectl get pods -ltf_job_key=tfjob-issue-summarization -o=jsonpath='{.items[0].metadata.name}')
```{{execute}}

As the training will take a long time to complete, it's recommended to move on to the next step and use a pre-trained model.
