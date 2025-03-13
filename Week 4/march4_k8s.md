# Kubernetes Overview and Deployment Process

## **What is Kubernetes?**
Kubernetes (K8s) is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It ensures high availability and reliability by handling failures and distributing workloads efficiently.

## **Why Use Kubernetes?**
Managing containerized applications manually is complex. Kubernetes simplifies this by:
- Deploying applications across multiple servers for redundancy.
- Restarting failed containers automatically.
- Load balancing traffic across running instances.
- Scaling applications up or down based on demand.

### **Restaurant Analogy for Kubernetes Concepts**
| **Kubernetes Component** | **Analogy** | **Function** |
|-------------------------|------------|-------------|
| **Pod** | Chef | Smallest deployable unit that runs containers |
| **Container** | Cooking station | Runs the application (e.g., Flask, Node.js) |
| **Deployment** | Recipe | Defines how many pods should be running |
| **Service** | Waiter | Exposes the application for users |
| **Liveness Probe** | Health check | Replaces unhealthy pods |
| **Readiness Probe** | Quality check | Ensures availability before exposing services |
| **Scaling (HPA)** | Hiring more chefs | Adjusts pods based on traffic |
| **Ingress** | Menu board | Routes requests to different applications |

---

## **Project Setup and Deployment**
### **Project Structure**
```
prac_k8s/
├── k8s/                # Kubernetes configuration files
│   ├── deployment.yaml  # Defines pod deployment
│   ├── service.yaml     # Exposes the application
├── app.py              # Flask application
├── Dockerfile          # Docker image instructions
```

### **1. Setting Up Minikube**
Minikube provides a local Kubernetes cluster:
```sh
minikube start
```
Issue encountered: Kubernetes API server failed to start.

**Solution:**
```sh
minikube delete
minikube start
```

---

### **2. Creating a Flask Application**
The Flask application includes a failure simulation endpoint:
```python
from flask import Flask
import os, signal, sys

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, Kubernetes!"

@app.route("/hang")
def hang():
    print("Simulating a full crash...")
    sys.stdout.flush()
    os.kill(os.getpid(), signal.SIGKILL)  # Forcefully terminates the process

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

---

### **3. Building and Pushing a Docker Image**
```sh
docker build -t pratikkumar10/flask-app:v1 .
docker push pratikkumar10/flask-app:v1
```

---

### **4. Writing Kubernetes Deployment**
Defines how many pods to run and includes a Liveness Probe.
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-container
        image: pratikkumar10/flask-app:v1
        ports:
        - containerPort: 5000
        livenessProbe:
          httpGet:
            path: /
            port: 5000
          initialDelaySeconds: 3
          periodSeconds: 5
          timeoutSeconds: 1
          failureThreshold: 2
```
Apply the deployment:
```sh
kubectl apply -f k8s/deployment.yaml
```

---

### **5. Creating a Kubernetes Service**
Defines how the application is accessed.
```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  selector:
    app: flask-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 5000
  type: NodePort
```
Apply the service:
```sh
kubectl apply -f k8s/service.yaml
```
Retrieve the service URL:
```sh
minikube service flask-service --url
```

---

## **Challenges and Solutions**
### **1. Liveness Probe Not Restarting the Pod**
Issue: The `/hang` endpoint returned an error, but Kubernetes did not restart the pod.

**Solution:**
- Modified `/hang` to fully terminate the process.
- Updated the Docker image:
  ```sh
  docker build -t pratikkumar10/flask-app:v2 .
  docker push pratikkumar10/flask-app:v2
  ```
- Updated Kubernetes deployment:
  ```yaml
  image: pratikkumar10/flask-app:v2
  ```
- Deleted old pods:
  ```sh
  kubectl delete pod -l app=flask-app
  ```
- Verified restart:
  ```sh
  kubectl exec -it flask-app-xxxxx -- curl http://localhost:5000/hang
  ```

### **2. Kubernetes Cached Old Image**
Issue: Kubernetes used a cached Docker image even after pushing a new version.

**Solution:**
- Forced Kubernetes to pull the latest image:
  ```sh
  kubectl delete pod -l app=flask-app
  ```
- Verified image update:
  ```sh
  kubectl get pods -w
  ```

---

## **Key Learnings**
✅ Kubernetes core concepts: Pod, Deployment, Service, Liveness Probe.  
✅ Running a local Kubernetes cluster using Minikube.  
✅ Containerizing a Flask application with Docker.  
✅ Writing Kubernetes YAML configurations for deployments and services.  
✅ Implementing Liveness Probe to restart failing pods.  
✅ Debugging Kubernetes issues using `kubectl describe pod` and logs.  
✅ Ensuring Kubernetes pulls the latest Docker image for updates.

