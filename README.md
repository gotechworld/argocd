# __Argo CD__ follows the GitOps pattern of using Git repositories as the source of truth for defining the desired application state

## __Argo CD__ automates the deployment of the desired application states in the specified target environments. Application deployments can track updates to branches, tags, or pinned to a specific version of manifests at a Git commit

### __Argo CD__ is implemented as a kubernetes controller which continuously monitors running applications and compares the current, live state against the desired target state (as specified in the Git repo)

&nbsp;

## Kubectl Based Installation

This method requires ```kubectl```, and it's a two steps process:

 1. Create a ```namespace```, to deploy Argo CD itself
 2. Run the HA installation manifest, via ```kubectl```.

Please run below commands in order:

```kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/ha/install.yaml
```

Now, please go ahead and check if the installation was successful. First, check if all Argo CD deployments are healthy:

```kubectl get deploy -n argocd
```

The output looks similar to (check the ```READY``` column - all ```Pods``` must be running):

```NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
argocd-applicationset-controller   1/1     1            1           51s
argocd-dex-server                  1/1     1            1           50s
argocd-notifications-controller    1/1     1            1           50s
argocd-redis-ha-haproxy            3/3     3            3           50s
argocd-repo-server                 2/2     2            2           49s
argocd-server                      2/2     2            2           49s
```

&nbsp;

Argo CD server must have a ```replicaset``` minimum value of ```2``` for the ```HA``` mode. If for some reason some deployments are not healthy, please check Kubernetes events and logs for the affected component Pods.
