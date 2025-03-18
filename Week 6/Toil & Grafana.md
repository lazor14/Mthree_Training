# SRE TRAINING (DAY 25) - Toil & Kubernetes Monitoring with Prometheus and Grafana

## **Toil in Site Reliability Engineering (SRE)**
During today's session, we discussed **toil** in SRE, which refers to repetitive, manual, and automatable operational work that adds little long-term value. Examples include manually restarting services, responding to the same alerts repeatedly, and performing routine deployments without automation. The focus of SRE is to reduce toil through automation, allowing engineers to work on higher-value tasks that improve system reliability and efficiency.

---

## **Prometheus and Grafana Overview**
We also explored **Prometheus** and **Grafana**, two essential tools for monitoring and observability in Kubernetes environments:

### **Prometheus**
- A monitoring system that collects and stores metrics as time-series data.
- Uses **PromQL** for querying and analyzing metrics.
- Scrapes metrics from configured endpoints like Kubernetes pods and services.

### **Grafana**
- A visualization tool used to create dashboards from data sources like Prometheus.
- Allows users to set up real-time alerts and notifications.
- Supports multiple data sources, including **Loki** for log aggregation.

---

## **Execution of the Kubernetes Monitoring Setup**
Today, I executed a script to set up Kubernetes monitoring using **Minikube, Prometheus, Loki, and Grafana**. The steps performed were:

### **1. Minikube Setup**
- Minikube was started with Docker as the driver, allocating **2 CPUs and 3GB RAM**.
- Verified that the Kubernetes cluster was running and ready.
  ![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/minikube.png)

### **2. Deploying a Sample Logging Application**
- Created a `sample-app` namespace in Kubernetes.
- Deployed a **BusyBox-based logger** application that generates **INFO, DEBUG, and ERROR logs**.
- The application continuously logs messages to simulate real-world workloads.
![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/namespace.png)

### **3. Installing Prometheus**
- Added the **Prometheus Helm repository** and updated it.
- Created a `monitoring` namespace and installed **Prometheus** with minimal resource configurations.
- Verified that Prometheus started successfully and was scraping Kubernetes metrics.
![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/prometheus.png)

### **4. Installing Loki for Log Aggregation**
- Installed **Loki** using Helm to collect logs from the `sample-app` namespace.
- Configured Loki to store and retrieve logs efficiently.
  ![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/loki.png)

### **5. Installing Grafana**
- Installed **Grafana** with **Prometheus and Loki** as pre-configured data sources.
- Configured the admin password and enabled dashboards for log visualization.
- Verified that Grafana was running and accessible.
![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/grafana.png)

### **6. Accessing Grafana and Importing Dashboards**
- Used **port-forwarding** to make Grafana accessible at `http://localhost:3000`.
- Logged into Grafana with `admin/admin` credentials.
- Imported a dashboard to visualize **application logs and resource metrics**.
![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/port-forwarding.png)
---

## **Grafana Dashboard Creation**
After setting up the monitoring infrastructure, I manually created a **Grafana dashboard** to visualize Kubernetes logs and metrics.

- Opened Grafana and navigated to **Dashboards > Create a new dashboard**.
- Added a **Logs Panel** using **Loki** as the data source.
- Used the query `{namespace="sample-app"}` to retrieve logs.
![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/app_logs.png)
- Added another panel for **filtered error logs** using the query `{namespace="sample-app"} |= "ERROR"`.
![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/error_logs.png)
- Configured a **CPU Usage Panel** using **Prometheus**.
  - Used the query `sum(rate(container_cpu_usage_seconds_total{namespace="sample-app"}[5m])) by (pod)`.
![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/cpu_usage.png)
- Created a **Log Volume Panel** to track logs over time.
![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/log_volume.png)
- Adjusted dashboard settings, including **refresh rate (5s)** and **default time range (Last 15 minutes)**.
![](https://github.com/lazor14/Mthree_Training/blob/66c41ac235d7aeff92d117896b29ce93e36147fb/Week%206/Images/settings.png)
- Saved the dashboard for continuous monitoring.

The monitoring setup was successfully completed, providing real-time insights into application logs and system performance.

