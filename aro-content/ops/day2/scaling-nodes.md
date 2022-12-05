## Introduction

<<<<<<< HEAD:aro-content/ops/day2/scaling-nodes.md
When deploying your Azure Red Hat OpenShift (ARO) cluster, you can configure many aspects of your worker nodes, but what happens when you need to change your worker nodes after they've already been created? These activities include scaling the number of nodes, changing the instance type, adding labels or taints, just to name a few.

Many of these changes are done using MachineSets. MachineSets ensure that a specified number of Machine replicas are running at any given time. Think of a MachineSet as a "template" for the kinds of Machines that make up the worker nodes of your cluster. These are similar to other Kubernetes resources, like a ReplicaSet is to Pods. One important caveat, is that MachineSets allow users to manage many Machines as a single entity, but are contained to a specific availability zone. If you'd like to learn more, see the [Red Hat documentation on machine management](https://docs.openshift.com/container-platform/latest/machine_management/index.html){:target="_blank"}.
=======
There may be times when you need to change aspects of your worker nodes. Things like scaling, changing the type, adding labels or taints to name a few. Most of these things are done through the use of machine sets. A machine is a unit that describes the host for a node and a machine set is a group of machines. Think of a machine set as a “template” for the kinds of machines that make up the worker nodes of your cluster. Similar to how a replicaset is to pods. A machine set allows users to manage many machines as a single entity though it is contained to a specific availability zone. If you'd like to learn more see [Overview of machine management](https://docs.openshift.com/container-platform/latest/machine_management/index.html)
>>>>>>> a78436f (initial v2):aro-content/ops/2-2-worker-nodes.md

## Scaling worker nodes

### View the machine sets that are in the cluster

Let's see which machine sets we have in our cluster.  If you are following this lab, you should only have three so far (one for each availability zone).

From the terminal run:

```bash
oc get machinesets -n openshift-machine-api
```

You will see a response like:

```
$ oc get machinesets -n openshift-machine-api
NAME                           DESIRED   CURRENT   READY   AVAILABLE   AGE
ok0620-rq5tl-worker-westus21   1         1         1       1           72m
ok0620-rq5tl-worker-westus22   1         1         1       1           72m
ok0620-rq5tl-worker-westus23   1         1         1       1           72m
```
This is telling us that there is a machine set defined for each availability zone in westus2 and that each has one machine.

<<<<<<< HEAD:aro-content/ops/day2/scaling-nodes.md
    For this workshop, we've deployed your ARO cluster with six total machines (three workers machines and three control plane machines), one in each availability zone. The output will look something like this:
=======
### View the machines that are in the cluster
>>>>>>> a78436f (initial v2):aro-content/ops/2-2-worker-nodes.md

Let's see which machines (nodes) we have in our cluster.

From the terminal run:

```bash
oc get machine -n openshift-machine-api
```

You will see a response like:

```
$ oc get machine -n openshift-machine-api
NAME                                 PHASE     TYPE              REGION    ZONE   AGE
ok0620-rq5tl-master-0                Running   Standard_D8s_v3   westus2   1      73m
ok0620-rq5tl-master-1                Running   Standard_D8s_v3   westus2   2      73m
ok0620-rq5tl-master-2                Running   Standard_D8s_v3   westus2   3      73m
ok0620-rq5tl-worker-westus21-n6lcs   Running   Standard_D4s_v3   westus2   1      73m
ok0620-rq5tl-worker-westus22-ggcmv   Running   Standard_D4s_v3   westus2   2      73m
ok0620-rq5tl-worker-westus23-hzggb   Running   Standard_D4s_v3   westus2   3      73m
```

As you can see we have 3 master nodes, 3 worker nodes, the types of nodes, and which region/zone they are in.

<<<<<<< HEAD:aro-content/ops/day2/scaling-nodes.md
    ```bash
    oc -n openshift-machine-api scale --replicas=2 ${MACHINESET}
    ```
=======
### Scale the number of nodes up via the CLI
>>>>>>> a78436f (initial v2):aro-content/ops/2-2-worker-nodes.md

Now that we know that we have 3 worker nodes, let's scale the cluster up to have 4 worker nodes. We can accomplish this through the CLI or through the OpenShift Web Console. We'll explore both.

<<<<<<< HEAD:aro-content/ops/day2/scaling-nodes.md
    ```bash
    oc -n openshift-machine-api get machinesets
    ```

    The output should look something like this:
=======
From the terminal run the following to imperatively scale up a machine set to 2 worker nodes for a total of 4. Remember that each machine set is tied to an availability zone so with 3 machine sets with 1 machine each, in order to get to a TOTAL of 4 nodes we need to select one of the machine sets to scale up to 2 machines.
>>>>>>> a78436f (initial v2):aro-content/ops/2-2-worker-nodes.md

```bash
oc scale --replicas=2 machineset <machineset> -n openshift-machine-api
```

<<<<<<< HEAD:aro-content/ops/day2/scaling-nodes.md
    Note, that the number of *desired* and *current* nodes matches the scale we specified, but only one is *ready* and *available*.
=======
For example:
>>>>>>> a78436f (initial v2):aro-content/ops/2-2-worker-nodes.md

```
$ oc scale --replicas=2 machineset ok0620-rq5tl-worker-westus23 -n openshift-machine-api
machineset.machine.openshift.io/ok0620-rq5tl-worker-westus23 scaled
```

<<<<<<< HEAD:aro-content/ops/day2/scaling-nodes.md
    ```bash
    oc -n openshift-machine-api get machine
    ```

    The output should look something like this:
=======
View the machine set
>>>>>>> a78436f (initial v2):aro-content/ops/2-2-worker-nodes.md

```bash
oc get machinesets -n openshift-machine-api
```

You will now see that the desired number of machines in the machine set we scaled is "2".

<<<<<<< HEAD:aro-content/ops/day2/scaling-nodes.md
Now let's scale the cluster back down to a total of 3 worker nodes, but this time, from the web console.

1. Return to your tab with the OpenShift Web Console. If you need to reauthenticate, follow the steps in the [Access Your Cluster](../setup/3-access-cluster/) section.
=======
```
$ oc get machinesets -n openshift-machine-api
NAME                           DESIRED   CURRENT   READY   AVAILABLE   AGE
ok0620-rq5tl-worker-westus21   1         1         1       1           73m
ok0620-rq5tl-worker-westus22   1         1         1       1           73m
ok0620-rq5tl-worker-westus23   2         2         1       1           73m
```

If we check the machines in the clusters
>>>>>>> a78436f (initial v2):aro-content/ops/2-2-worker-nodes.md

```bash
oc get machine -n openshift-machine-api
```

<<<<<<< HEAD:aro-content/ops/day2/scaling-nodes.md
    ![Web Console - Cluster Settings](/assets/images/web-console-machineset-sidebar.png){ align=center }

1. In the overview you will see the same information about the MachineSets that you saw on the command line. Now, locate the MachineSet which has "2 of 2" machines, and click on the ⋮ icon, then select *Edit machine count*.

    ![Web Console - MachineSets Menu](/assets/images/web-console-machinesets-three-dots.png){ align=center }
    ![Web Console - MachineSets Count Menu](/assets/images/web-console-machinesets-edit-count-menu.png){ align=center }

1. Next, reduce the count from "2" to "1" and click *Save* to save your changes.

    ![Web Console - MachineSets Edit Count](/assets/images/web-console-machinesets-edit-count.png){ align=center }

Congratulations! You've successfully scaled your cluster up and back down to three nodes.
=======
You will see that one is in the "Provisioned" phase (and in the zone of the machineset we scaled) and will shortly be in "running" phase.

```
$ oc get machine -n openshift-machine-api
NAME                                 PHASE         TYPE              REGION    ZONE   AGE
ok0620-rq5tl-master-0                Running       Standard_D8s_v3   westus2   1      74m
ok0620-rq5tl-master-1                Running       Standard_D8s_v3   westus2   2      74m
ok0620-rq5tl-master-2                Running       Standard_D8s_v3   westus2   3      74m
ok0620-rq5tl-worker-westus21-n6lcs   Running       Standard_D4s_v3   westus2   1      74m
ok0620-rq5tl-worker-westus22-ggcmv   Running       Standard_D4s_v3   westus2   2      74m
ok0620-rq5tl-worker-westus23-5fhm5   Provisioned   Standard_D4s_v3   westus2   3      54s
ok0620-rq5tl-worker-westus23-hzggb   Running       Standard_D4s_v3   westus2   3      74m
```

### Scale the number of nodes down via the Web Console

Now let's scale the cluster back down to a total of 3 worker nodes, but this time, from the web console. (If you need the URL or credentials in order to access it please go back to the relevant portion of Lab 1)

Access your OpenShift web console from the relevant URL. If you need to find the URL you can run:

```bash
az aro show \
   --name <CLUSTER-NAME> \
   --resource-group <RESOURCEGROUP> \
   --query "consoleProfile.url" -o tsv
```

Expand "Compute" in the left menu and then click on "MachineSets"

![machinesets-console](../assets/images/scale-down-console.png)

In the main pane you will see the same information about the machine sets from the command line.  Now click on the "three dots" at the end of the line for the machine set that you scaled up to "2". Select "Edit machine count" and decrease it to "1". Click save.

![machinesets-edit](../assets/images/edit-machinesets.png)

This will now decrease that machine set to only have one machine in it.
>>>>>>> a78436f (initial v2):aro-content/ops/2-2-worker-nodes.md
