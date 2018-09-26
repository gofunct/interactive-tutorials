The API defines us with the ability to take a Github issue and have a summary created.

This can also be accessed via the Web Application. The example below will deploy the web UI.

```
git clone https://github.com/kubeflow/examples ~/kubeflow-examples
cd ~/kubeflow-examples/github_issue_summarization/ks-kubeflow
ks env add frontendenv
ks param set ui github_token $GITHUB_TOKEN
ks param set ui modelUrl http://issue-summarization.default.svc.cluster.local:8000/api/v0.1/predictions
ks apply frontendenv -c ui
```{{execute}}

Once deployed, update the configuration to include a Github Token so the application can utilise the Github API.

Once deployed you can access the UI at https://[[HOST_SUBDOMAIN]]-30080-[[KATACODA_HOST]].environments.katacoda.com/issue-summarization/

A live demo that has been trained against a more extensive dataset is available at http://gh-demo.kubeflow.org/. Notice how the quality improves as more data is used for the training.
