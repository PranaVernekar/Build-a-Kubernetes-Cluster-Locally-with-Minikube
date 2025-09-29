
# 🚀 Build a Kubernetes Cluster Locally with Minikube

This demonstrates how to deploy and manage applications in Kubernetes using **Minikube**, **kubectl**, and **Docker**.

---

## 📂 Directory Structure

```

k8s-minikube-task/
│── deployment.yaml
│── service.yaml
│── README.md

````

---

## ⚡ Step-by-Step Guide

### Step 1: Install & Start Minikube

#### On Linux/macOS
```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
````

#### On Windows (PowerShell as Admin)

```powershell
choco install minikube -y
```

#### Start Cluster

```bash
minikube start --driver=docker
```

✅ **What we achieved:** A local Kubernetes cluster running inside Docker.

📸 *Screenshots: 

<img width="1919" height="1018" alt="Screenshot 2025-09-29 151823" src="https://github.com/user-attachments/assets/d487ddf9-9e48-4731-a43d-562cbade296d" />

<img width="1919" height="1017" alt="Screenshot 2025-09-29 151837" src="https://github.com/user-attachments/assets/8683eb9b-dac8-4101-a5f1-6b28dd278fcf" />

<img width="1919" height="1020" alt="Screenshot 2025-09-29 151853" src="https://github.com/user-attachments/assets/38d577b0-4357-4bd1-a0fd-dc251c57b0cb" />

---

### Step 2: Verify Cluster

```bash
kubectl get nodes
```

✅ **What we achieved:** Node(s) are ready.

📸 *Screenshot: kubectl get nodes*

<img width="1916" height="1020" alt="Screenshot 2025-09-29 151908" src="https://github.com/user-attachments/assets/caf17a17-5040-4c87-b1d8-0a3727849d28" />



---

### Step 3: Create a Deployment

**deployment.yaml**

```yaml
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
```

Apply:

```bash
kubectl apply -f deployment.yaml
```

✅ **What we achieved:** 2 pods running NGINX.

📸 *Screenshot: kubectl get deployments and kubectl get pods*

<img width="1918" height="1018" alt="Screenshot 2025-09-29 152604" src="https://github.com/user-attachments/assets/bec9058b-bd20-4d7c-b87f-d8e40379f597" />

---

### Step 4: Expose Deployment via Service

**service.yaml**

```yaml
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
```

Apply:

```bash
kubectl apply -f service.yaml
```

✅ **What we achieved:** Pods exposed via NodePort service.

📸 *Screenshot: kubectl get services*

<img width="1919" height="1018" alt="Screenshot 2025-09-29 152735" src="https://github.com/user-attachments/assets/9baadc78-3830-4a25-a4ae-084a23a3c7f1" />

---

### Step 5: Access the App

```bash
minikube service hello-service
```

✅ **What we achieved:** Opened NGINX welcome page in browser.

📸 *Screenshot: Browser showing NGINX default page*

<img width="1919" height="977" alt="Screenshot 2025-09-29 152956" src="https://github.com/user-attachments/assets/132d6f09-6f19-419a-926c-c5aaecd4fd3a" />


---

### Step 6: Scaling Deployment

```bash
kubectl scale deployment hello-deployment --replicas=4
kubectl get pods
```

✅ **What we achieved:** Deployment scaled to 4 pods.

📸 *Screenshot: kubectl get pods (4 pods running)*

<img width="1919" height="1018" alt="Screenshot 2025-09-29 153042" src="https://github.com/user-attachments/assets/858f8ad5-8208-47a7-8084-19400ca657eb" />


---

### Step 7: Describe & Logs

```bash
kubectl describe deployment hello-deployment
kubectl logs <pod-name>
```

✅ **What we achieved:** Checked pod details & logs.

📸 *Screenshot: kubectl describe deployment output*

<img width="1919" height="1016" alt="Screenshot 2025-09-29 153117" src="https://github.com/user-attachments/assets/7e15a572-7967-466d-a92d-c9bf0017cbc6" />


---

## 🎯 Final State of Cluster

```bash
kubectl get all
```

Expected:

```
NAME                                    READY   STATUS    RESTARTS   AGE
pod/hello-deployment-xxxx-xxx           1/1     Running   0          5m
...

NAME                    TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
service/hello-service   NodePort    10.108.210.67   <none>        80:31234/TCP   4m
service/kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        10m

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/hello-deployment   4/4     4            4           5m
```

📸 *Screenshot: kubectl get all*

<img width="1918" height="1018" alt="Screenshot 2025-09-29 160321" src="https://github.com/user-attachments/assets/4c5f040e-e1d7-4c87-b552-0a535c441710" />

<img width="1919" height="973" alt="Screenshot 2025-09-29 205307" src="https://github.com/user-attachments/assets/57c117db-3540-483a-aacf-d151227b51e3" />

---

## ✅ Outcomes / Learnings

* Installed & started local Kubernetes with **Minikube**
* Used **kubectl** to manage resources
* Created & scaled **Deployments**
* Exposed pods using **Services**
* Viewed **logs** and **descriptions**
