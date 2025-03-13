# **Flask & kubernetes (also EC2 instance AWS)**
### **Overview of the `fixed-k8s.sh` Script**

The `fixed-k8s.sh` script automates the deployment of a containerized application in Kubernetes. It simplifies the process by handling project setup, application deployment, and resource management. The script has been enhanced to prevent common issues encountered in **WSL2 (Windows Subsystem for Linux)** and **AWS EC2** environments.

---

### **Key Functionalities**

1. **Project Structure Setup**  
   - Creates the `k8s-master-app` directory with subfolders for application code, Kubernetes manifests, configuration files, logs, and scripts.  
   - Dynamically generates necessary Kubernetes YAML configurations.  

2. **Application Deployment**  
   - Defines a **Flask-based Python application (`app.py`)**.  
   - Provides a **Dockerfile** for containerization.  
   - Lists required dependencies in `requirements.txt`.  

3. **Kubernetes Resource Management**  
   - **Namespace:** Uses `k8s-demo` for isolation.  
   - **ConfigMaps & Secrets:** Manages configurations and sensitive data.  
   - **Deployments:** Ensures the application runs with multiple replicas.  
   - **Services:** Exposes the application using NodePort.  
   - **Horizontal Pod Autoscaler (HPA):** Dynamically scales pods based on CPU/memory usage.  

4. **Automation Scripts**  
   - **Deployment (`deploy.sh`)**: Configures Minikube, builds Docker images, and deploys resources.  
   - **Cleanup (`cleanup.sh`)**: Deletes all deployed Kubernetes resources.  
   - **Testing (`test-app.sh`)**: Runs health checks on the deployed application.  

---

### **Challenges Faced in AWS EC2**

#### **1. Minikube Installation Issues**
- Minikube is intended for local Kubernetes testing, whereas **AWS prefers EKS (Elastic Kubernetes Service)**.
- Minikube requires a **Docker driver and at least 2 CPUs**, which AWS EC2 instances often lack.

**Error Message:**
```
X Exiting due to RSRC_INSUFFICIENT_CORES: Docker has less than 2 CPUs available, but Kubernetes requires at least 2 to be available.
```
**Solution:**  
- Instead of Minikube, use **AWS EKS** for Kubernetes deployment.

#### **2. Disk Space Constraints**
- AWS EC2 instances have a default disk size of **8GB**, which fills up quickly due to Kubernetes logs, images, and volumes.
- Expanding storage requires stopping the instance and resizing the volume:

```bash
sudo growpart /dev/xvda 1
sudo resize2fs /dev/xvda1
```

#### **3. Kubernetes Cluster Connection Issues**
- The script assumes Minikube’s built-in Kubernetes API (`192.168.49.2:8443`).
- In AWS, **EKS requires updating the `kubectl` context**:

```bash
aws eks update-kubeconfig --name my-cluster --region us-east-1
```
**Error Message:**
```
The connection to the server 192.168.49.2:8443 was refused - did you specify the right host or port?
```
**Solution:**
- Use `aws eks update-kubeconfig` to properly connect to the cluster.

#### **4. Port Forwarding Issues**
- The script relies on `kubectl port-forward`, which is **not ideal for AWS EC2**.
- AWS prefers **LoadBalancer Services** or **Ingress Controllers** for external access.

**Alternative Approach:** Use a LoadBalancer Service instead:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: k8s-master-app
  namespace: k8s-demo
spec:
  type: LoadBalancer
  selector:
    app: k8s-master
  ports:
    - protocol: TCP
      port: 80
      targetPort: 5000
```

---

### **Key Takeaways**

- **Minikube is not suitable for AWS EC2**; use **EKS instead**.
- **Expanding EC2 storage** prevents `No Space Left` errors.
- **Configuring `kubectl` properly** ensures a stable connection to EKS.
- **LoadBalancer Services** should replace `kubectl port-forward` for better accessibility.

---

### **Next Steps**

- Modify the script for **direct deployment to AWS EKS**.
- Implement **better disk space management** for EC2 instances.
- Replace **NodePort with AWS Load Balancer** for external access.

This ensures a more efficient and production-ready deployment workflow in AWS. 🚀

