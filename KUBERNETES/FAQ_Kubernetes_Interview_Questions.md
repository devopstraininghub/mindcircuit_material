
### 1. What is Kubernetes?


Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It helps manage clusters of containers in a reliable and scalable way.



### 2. Write a Kubernetes Deployment manifest to create an Nginx pod with 3 replicas.



```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
```



### 3. Write a Kubernetes ConfigMap YAML to store database configuration with keys `DB_HOST=db.example.com` and `DB_PORT=5432`.



```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: db-config
data:
  DB_HOST: db.example.com
  DB_PORT: "5432"
```



### 4. What is HPA (Horizontal Pod Autoscaler)?


HPA automatically scales the number of pod replicas in a deployment or replica set based on observed CPU utilization or other custom metrics.



### 5. How do you scale up the replicas in a deployment using a command?



```bash
kubectl scale deployment nginx-deployment --replicas=5
```



### 6. How to copy files from the host to a container using a command?



```bash
kubectl cp /path/on/host <pod-name>:/path/in/container
```



### 7. How to restart a pod without stopping the deployment?



```bash
kubectl rollout restart deployment <deployment-name>
```



### 8. How do pods communicate with each other?


Pods communicate using internal IPs assigned by the Kubernetes network. Services provide stable DNS names and load balancing to enable communication between pods.



### 9. What are the security best practices for a Kubernetes cluster?



* Use RBAC to control access
* Run containers as non-root
* Use Network Policies
* Store sensitive data in Secrets
* Use pod security standards
* Enable auditing and logging
* Keep components up-to-date



### 10. How do you ensure the application is highly available and scalable?



* Use Deployments with multiple replicas
* Distribute pods across multiple nodes
* Use HPA to autoscale based on load
* Configure readiness and liveness probes
* Use LoadBalancers or Ingress Controllers



### 11. How do you troubleshoot a pod in a Pending state?



* Run:

  ```bash
  kubectl describe pod <pod-name>
  ```
* Check for:

  * Resource limits on nodes
  * Taints and tolerations
  * PVC binding issues
  * NodeSelector/affinity misconfigurations



### 12. How do you create a Kubernetes cluster? Explain the steps.



* Using Minikube (for local testing):

  ```bash
  minikube start
  ```
* Using `kubeadm` (for production):

  1. Install Docker, kubelet, kubeadm, kubectl
  2. Run `kubeadm init` on the master node
  3. Set up kubeconfig
  4. Install a CNI plugin like Calico
  5. Join worker nodes using the `kubeadm join` command



### 13. What critical issues have you faced with a Kubernetes cluster?



* Pods stuck in Pending due to lack of resources
* CrashLoopBackOff due to bad image or config
* Network issues due to missing CNI plugin
* HPA not working due to missing metrics server
* Disk pressure evicting pods unexpectedly



### 14. What is an indentation error in a YAML file?


Indentation errors happen when YAML syntax is not properly aligned. YAML uses spaces (not tabs) and expects consistent indentation. Misaligned sections cause parsing failures in Kubernetes.



### 15. How do you enter into a running pod or container in Kubernetes and Docker?



* Kubernetes:

  ```bash
  kubectl exec -it <pod-name> -- /bin/bash
  ```

* Docker:

  ```bash
  docker exec -it <container-id> /bin/bash
  ```


### 16. Why ArgoCD?


ArgoCD is a GitOps tool used for declarative deployment in Kubernetes. It:

* Automatically syncs your cluster state with a Git repository.
* Supports version control for infrastructure and apps.
* Enables secure, auditable, and automated deployments.



### 17. How do you use `kubectl logs`?



```bash
kubectl logs <pod-name> [-c container-name]
```

* View logs of a pod.
* If the pod has multiple containers, use `-c` to specify the container name.



### 18. How do you set up Prometheus and Grafana in Kubernetes?



* Use the kube-prometheus-stack Helm chart for installation.
* Prometheus scrapes metrics from nodes and services.
* Grafana is used to visualize these metrics using dashboards.



### 19. ReplicaSet vs Deployment?



* ReplicaSet ensures the desired number of pod replicas.
* Deployment manages ReplicaSets and adds rollout strategies (e.g., updates, rollbacks).
* Always use Deployment for better control over application lifecycle.



### 20. Canary vs Rolling Deployment?



* Canary: Sends a small % of traffic to the new version for testing.
* Rolling: Gradually replaces old pods with new ones without downtime.



### 21. What is Blue-Green Deployment?



* Maintain two environments: Blue (current) and Green (new).
* After testing, switch traffic to Green to complete deployment.
* Easy rollback by switching back to Blue.



### 22. Differentiate StatefulSet vs DaemonSet?



* StatefulSet:

  * Stable pod names and persistent volumes
  * Useful for databases like MySQL, Kafka, etc.
* DaemonSet:

  * Ensures one pod per node
  * Ideal for logging, monitoring, and network agents



### 23. Can you attach a PV to a Deployment?


Yes. Use a `PersistentVolumeClaim` in the pod spec, and Kubernetes binds it to a `PersistentVolume`.



### 24. Init Containers vs Sidecar Containers?

* Init Containers:

  * Run before the main container starts
  * Used for setup or waiting for dependencies
* Sidecar Containers:

  * Run alongside the main app
  * Used for logging, monitoring, proxying, etc.



### 25. How do you create a PersistentVolume (PV)?



```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /data
```



### 26. All pods go offline in EKS. Where do you check logs?



* `kubectl get pods --all-namespaces`: Check pod status
* `kubectl logs`: View logs
* `kubectl describe node/pod`: View resource/scheduling issues
* CloudWatch Logs: If integrated
* Check Cluster Autoscaler, node groups, and VPC CNI plugin logs



### 27. Someone deleted IPs in EKS. How do you trace it in AWS?


* Use AWS CloudTrail
* Filter for actions like `DeleteNetworkInterface`, `DetachNetworkInterface`
* Check the IAM user, IP, and time of deletion
* Also review VPC and EC2 Network Interface logs



### 28. API Server is down, but EC2 nodes are up. What to check?



* EKS Console → Cluster health
* VPC endpoints → Ensure control plane can communicate
* Network ACLs, Route tables, Subnets
* IAM role attached to nodes
* Use `aws eks describe-cluster` and CloudWatch Logs



### 29. Example of a recent Kubernetes issue you resolved?


A pod was in a CrashLoopBackOff state due to an incorrect image tag.

* Checked logs with `kubectl logs`
* Found `ImagePullBackOff`
* Updated the correct image in the deployment YAML
* Applied changes and verified success



### 30. What Kubernetes services are you using and why?



* ClusterIP: For internal communication between pods
* NodePort: Exposes service on a static port across all nodes
* LoadBalancer: Public access via cloud provider's LB
* Ingress: For path-based or domain-based routing using HTTP/HTTPS



### 31. Can multiple pods run under a single Deployment?


No. A Deployment manages multiple replicas of the same pod template.
If you need different pods/containers, use:

* Multiple Deployments
* A Pod with multiple containers (sidecar pattern)

### 32. How do you back up and restore a Kubernetes cluster?

Use tools like etcdctl for etcd backup

Velero for backup/restore of resources and volumes

Take regular snapshots of critical configs and secrets

### 33. How do you integrate CI/CD with Kubernetes?

Use Jenkins, GitHub Actions, or GitLab CI

Build & push container image

Use kubectl or ArgoCD to deploy to the cluster

Apply manifests via Helm or Kustomize


### 34. What is a Service in Kubernetes?


A Service is a Kubernetes object that exposes a set of pods as a network service. It ensures stable access to pods using selectors and DNS.

Types of Services:

* ClusterIP – default, internal access only
* NodePort – exposes via static port on each node
* LoadBalancer – provisions an external load balancer (in cloud)
* ExternalName – maps a service to an external DNS name



### 35. What is a Namespace in Kubernetes?


Namespaces provide a way to divide cluster resources between multiple users. Useful in multi-team environments to separate concerns.



### 36. What is a ConfigMap and how is it different from a Secret?



* ConfigMap stores non-sensitive configuration data as key-value pairs.
* Secret stores sensitive data (e.g., passwords, tokens) in base64 encoded format.

Both can be mounted as environment variables or volumes in pods.


### 37. How do you perform rolling updates in Kubernetes?


Using a Deployment object:

```bash
kubectl set image deployment/my-deploy my-container=myimage:v2
```

Kubernetes will gradually update pods to the new version.


### 38. What is the role of `etcd` in Kubernetes?


`etcd` is a distributed key-value store used by Kubernetes to store all cluster data (state, configurations, secrets).



### 39. How does Kubernetes handle container failures?


Kubernetes uses health checks, restart policies, and controller loops to:

* Detect failed pods
* Restart them
* Reschedule to other nodes if needed


### 40. How do you upgrade a Kubernetes cluster?



* For EKS: Use AWS Console or `eksctl upgrade cluster`
* For kubeadm:

  1. Drain and cordon nodes
  2. Upgrade control plane using `kubeadm upgrade`
  3. Upgrade kubelet and kubectl
  4. Uncordon nodes



### 41. What is a Job and CronJob in Kubernetes?



* Job: Runs a task until completion (e.g., batch processing).
* CronJob: Schedules jobs at specific times using cron syntax.

### 42. What is the use of `kube-proxy`?


`kube-proxy` maintains network rules on nodes and enables communication to pods from inside and outside the cluster.


### 43. What is Pod Affinity and Anti-Affinity?


* Affinity: Schedule pods close to each other (e.g., for low latency).
* Anti-Affinity: Schedule pods on different nodes (e.g., for high availability).

### 44. How to debug container CrashLoopBackOff?


* Check logs:

  ```bash
  kubectl logs <pod-name>
  ```
* Describe the pod:

  ```bash
  kubectl describe pod <pod-name>
  ```
* Check for wrong image, env issues, or missing files/configs.

### 45. What are LimitRange and ResourceQuota?

* LimitRange: Sets default/min/max CPU/memory per pod/container.
* ResourceQuota: Enforces limits on total resource usage in a namespace.

### 46. What is pdb in k8s?

In Kubernetes (k8s), PDB stands for Pod Disruption Budget.

A Pod Disruption Budget (PDB) is a Kubernetes resource that ensures a minimum number or percentage of pods in a replicated application remain available during voluntary disruptions.

* Why Use a PDB?
Without a PDB, Kubernetes might evict too many pods at once, possibly causing application downtime. A PDB helps maintain application availability during routine operations.

### 47. 


