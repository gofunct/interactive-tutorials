Training should run for about 10 epochs and takes 5-10 minutes on a CPU cluster. Logs can be inspected using the following command to see the training progress.

```
PODNAME=$(kubectl get pods -l pytorch_job_name=distributed-mnist,task_index=0 -o name)
kubectl logs ${PODNAME}
```{{execute}}

This should result in output similar to the following:

<pre class="file">
Downloading http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz
Downloading http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz
Downloading http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz
Downloading http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz
Processing...
Done!
Rank  0 , epoch  0 :  1.2745472780232237
Rank  0 , epoch  1 :  0.5743547164872765
</pre>

You can see how the training is utilising all four cores by using htop on the Node. If we added additional Nodes to the Kubernetes cluster, the three replicas would be distributed across the nodes and speed up the training time.

`ssh -o StrictHostKeyChecking=no -t root@[[HOST2_IP]] htop`{{execute}}

Use `clear`{{execute interrupt}} and Ctrl+C to exit htop.

## View State

With the training running, it's possible to use Kubectl to view the state and when the training has completed.

By outputting the job in yaml, it's possible to view the internal details of the execution. This includes the state.

`kubectl get -o yaml pytorchjobs distributed-mnist`{{execute}}

The details can be outputted in different formats, such as JSON. This can be used with tools such as `jq` to parse the output.

`kubectl get -o json pytorchjobs distributed-mnist  | jq .status`{{execute}}

`kubectl get -o json pytorchjobs distributed-mnist  | jq .status.state`{{execute}}

Once the training has finished, the results look like this:

<pre class="file">
Downloading http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz
Downloading http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz
Downloading http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz
Downloading http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz
Processing...
Done!
Rank  0 , epoch  0 :  1.2745472780232237
Rank  0 , epoch  1 :  0.5743547164872765
Rank  0 , epoch  2 :  0.4351522875810737
Rank  0 , epoch  3 :  0.36984553888662536
Rank  0 , epoch  4 :  0.3216655456038045
Rank  0 , epoch  5 :  0.2951831894356813
Rank  0 , epoch  6 :  0.2750083558114448
Rank  0 , epoch  7 :  0.2595048323273659
Rank  0 , epoch  8 :  0.24862684973521526
Rank  0 , epoch  9 :  0.22692083941101393
</pre>

As the training has continued, the loss has decreased.