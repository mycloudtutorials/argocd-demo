## Kubernetes Continuous Deployment with ArgoCD 

In this demo, we will explore ArgoCD in new Kubernetes Cluster setup locally. We will explore how ArgoCD helps in the CD process, has built in auto-healing and pruning capabilities.

### Demo Steps

1. Create New Kubernetes Cluster
<br>Using kind tool
```
kind create cluster --name argocd-demo
```

2. Install Argo CD components on the Cluster
```
Follow instructions from:
https://argo-cd.readthedocs.io/en/stable/getting_started/

After the installation, verify the pod, services etc. in the argocd namespace
```

3. Setup Port-forwarding, so we can access the UI locally.
```
kubectl port-forward -n argocd svc/argocd-server 8010:443
```

4. Access the UI at localhost:8010 (port used in previous step)
```
UserName: admin
Password: <<Need to retrieve from a secret created during argocd setup. Use the following command>>

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

5. Setup ArgoCD Application. 
```
After creating the application.yaml, apply it using 
kubectl apply -f application.yaml
```
5a. (Optional as needed) - setup port forwarding
```
kubectl port-forward -n argocd svc/argocd-server 8010:443
```

6. Check the UI, explore the application, deployment topology, services, replicasets, pods etc.

7. Use a temporary pod to curl the service
```
kubectl -n web-dev-namespace run tmp1 --image=nginx:alpine --rm -it

curl my-web-service
```

8. Change the deployment manifest file in git to refer to different docker image

9. Watch the UI to take show the changes taking effect. By default, ArgoCD polls git every 3 minutes. We can click refresh and sync to make it poll instantly.

10. Run the curl again. It should show output from the different docker image.

11. To test auto-healing, lets modify the deployment directly using the following command. Change Replicas to 2.
```
kubectl -n web-dev-namespace edit deployment my-web-app
```
ArgoCD compares the updated state with the desired state (stored in git) and reverts the changes.

12. To test pruning, change the name of deployment in git. Watch ArgoCD UI, it delete the old deployment, replicaset and pods and create new ones.

