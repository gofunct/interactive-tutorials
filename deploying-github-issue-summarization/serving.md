Once the trained model (_output_model.h5_) is available, it needs to be packaged up as a Docker Image. As the serving will be done via Seldon Core, the command below would build the required Docker Image containing your trained model. This has been done for you and uploaded as _gcr.io/kubeflow-images-public/issue-summarization:0.1_.

The commands below represent what's required to build the image.

```
docker run -v $(pwd)/my_model/:/my_model seldonio/core-python-wrapper:0.7 /my_model IssueSummarization 0.1 gcr.io --base-image=python:3.6 --image-name=gcr-repository-name/issue-summarization

cd build
./build_image.sh
```

## Install the kubeflow/seldon package

Kubeflow installs the kubeflow/seldon package by default. If we had wanted to setup Kubeflow manually, this would have been added using `ks pkg install kubeflow/seldon`

## Generate the Seldon component and deploy it
As we've made a change to the configuration, it's required to generate the template containing Seldon and deploy it to the Kubernetes cluster.
```
cd ~/kubeflow_ks_app/
kubectl create namespace kubeflow
ks generate seldon seldon --name=seldon
ks apply default -c seldon
```{{execute}}

## Deploy Trained Model via Seldon Core (seldon-serve-simple)
With the Seldon components deployed, it's possible to use this to serve our trained model via the Docker Image _gcr.io/kubeflow-images-public/issue-summarization:0.1_.  The approach below defines two replicas, this means we'll have two instances of our application available and load balanced by Kubernetes for reliability and performance.
```
ks generate seldon-serve-simple issue-summarization-model-serving \
  --name=issue-summarization \
  --image=gcr.io/kubeflow-images-public/issue-summarization:0.1 \
  --replicas=2
ks apply default -c issue-summarization-model-serving
```{{execute}}

## Disable the livenessProbe

`kubectl patch deployment issue-summarization-issue-summarization  --type json   -p='[{"op": "remove", "path": "/spec/template/spec/containers/0/livenessProbe"}]'`{{execute}}

View the status of the deployment at `kubectl get pods`{{execute}}

## Query

Once deployed, it's possible to call the API and have a respond from our trained model.

`kubectl run -i --tty curl --image=benhall/curl -- sh`{{execute}}

```
curl -X POST -H 'Content-Type: application/json' -d '{"data":{"ndarray":[["issue overview add a new property to disable detection of image stream files those ended with -is.yml from target directory. expected behaviour by default cube should not process image stream files if user does not set it. current behaviour cube always try to execute -is.yml files which can cause some problems in most of cases, for example if you are using kuberentes instead of openshift or if you use together fabric8 maven plugin with cube"]]}}' http://issue-summarization.default.svc.cluster.local:8000/api/v0.1/predictions
```{{execute}}
