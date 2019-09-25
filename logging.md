## Setting up logging on K8S example

This guide is intended to provide an example of how to setup a simple logging stack on K8S.

There are a lot of options available in the helm stable catalog, and development / operation teams are free to chose one which works for their use case scenario.

![](images/logging1.png)

For the purpose of this demo we already have a K8S cluster running and managed by Rancher.

We have setup aa logging project to deploy this on the cluster, as this eventually helps with RBAC to managing the workload at a K8S layer.

For the purpose of this demo I will be choosing the ElasticSearch+FluentBit+Kibana stack.

![](images/logging2.png)

There are few customisations available in the app, which need to be configured on the use case.

![](images/logging3.png)

There are some key items to note:

* The app will spin up 3 elasticsearch pods, so based on the number of worker nodes or how ingress needs to be managed to elasticsearch the team may need to decide if they prefer to use NodePort or ClusterIP.

For the purpose of this example, we have used ClusterIP
![](images/logging11.png)

* The front end for viewing logs is via Kibana, which can be customised further. One key customisation to note of is the **hostname** to use to access Kibana.

The hostname is used to setup the ingress rule on thee ingress controller.
![](images/logging7.png)

For the purpose of this demo we use ***logging.demo**

* The users may also want to choose how to manage persistence for ElasticSearch. This can be easily achieved by using persistent volume claims. For the purpose of this demo we will not be changing this.


We simply now click **Launch** and this should launch the catalog app into the logging project under a new namespace named **efk**

![](images/logging4.png)

Users can now follow the status of the EFK rollout, and in a few mins you should have all the logging components deployed.

![](images/logging5.png)

Users can also now verify the ingress spec to allow access to Kibana.

![](images/logging8.png)

Kibana can now be accessed by pointing the broweser to http://logging.demo

![](images/logging9.png)

As part of the Kibana / ElasticSearch configuration, at first time the user will need to specify an index pattern to use from ElasticSearch.

![](images/logging6.png)

Once this initial setup the users can search for logs from the cluster using Kibana.

![](images/logging10.png)

This document hopefully shows how easy it is to setup a logging stack of your choice using the wide variety of community curated helm charts.

This documented is intended to be a quick start reference guide as we used most of the common defaults.

For production grade logging stacks, the users need to decide on factors such as:
* log retention duration
* persistence
* rate of log ingestion, which in turn impacts elasticsearch JVM heap sizing.

