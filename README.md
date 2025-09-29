# 🚀 Task 5: Build a Kubernetes Cluster Locally with Minikube

This project demonstrates how to deploy and manage applications in Kubernetes using **Minikube**, **kubectl**, and **Docker**.

---

## 📂 Directory Structure

k8s-minikube-task/
│── deployment.yaml
│── service.yaml
│── README.md


---

## ⚡ Step-by-Step Guide

### Step 1: Install & Start Minikube

#### On Linux/macOS
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
On Windows (PowerShell as Admin)
powershell
Copy code
choco install minikube -y
Start Cluster
bash
Copy code
minikube start --driver=docker
✅ What we achieved: A local Kubernetes cluster running inside Docker.

📸 Screenshot here: minikube start success output


Step 2: Verify Cluster
bash
Copy code
kubectl get nodes
✅ What we achieved: Node(s) are ready.

📸 Screenshot here: kubectl get nodes

Step 3: Create a Deployment
deployment.yaml

yaml
Copy code
apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-deployment
  labels:
    app: hello-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: hello-app
  template:
    metadata:
      labels:
        app: hello-app
    spec:
      containers:
      - name: hello-container
        image: nginx:latest
        ports:
        - containerPort: 80
Apply:
kubectl apply -f deployment.yaml
✅ What we achieved: 2 pods running NGINX.

📸 Screenshot here: kubectl get deployments and kubectl get pods

Step 4: Expose Deployment via Service
service.yaml

yaml
Copy code
apiVersion: v1
kind: Service
metadata:
  name: hello-service
spec:
  selector:
    app: hello-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
Apply:

bash
Copy code
kubectl apply -f service.yaml
✅ What we achieved: Pods exposed via NodePort service.

📸 Screenshot here: kubectl get services

Step 5: Access the App
bash
Copy code
minikube service hello-service
✅ What we achieved: Opened NGINX welcome page in browser.

📸 Screenshot here: Browser with NGINX page

Step 6: Scaling Deployment
bash
Copy code
kubectl scale deployment hello-deployment --replicas=4
kubectl get pods
✅ What we achieved: Deployment scaled to 4 pods.

📸 Screenshot here: kubectl get pods (4 pods running)

Step 7: Describe & Logs
bash
Copy code
kubectl describe deployment hello-deployment
kubectl logs <pod-name>
✅ What we achieved: Checked pod details & logs.

📸 Screenshot here: kubectl describe deployment output

🎯 Final State of Cluster
bash
Copy code
kubectl get all
Expected:

swift
Copy code
NAME                                    READY   STATUS    RESTARTS   AGE
pod/hello-deployment-xxxx-xxx           1/1     Running   0          5m
...

NAME                    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/hello-service   NodePort    10.108.210.67   <none>        80:31234/TCP   4m
service/kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        10m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hello-deployment   4/4     4            4           5m
📸 Screenshot here: kubectl get all

✅ Outcomes / Learnings
Installed & started local Kubernetes with Minikube

Used kubectl to manage resources

Created & scaled Deployments

Exposed pods using Services

Viewed logs and descriptions