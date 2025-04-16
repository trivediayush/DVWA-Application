🛡️ DVWA on Kubernetes - Local Security Lab
===========================================

This project demonstrates how to deploy **Damn Vulnerable Web Application (DVWA)** on a local Kubernetes cluster using **Minikube** and then simulate common web application attacks.

📸 Screenshots
--------------

*   Minikube Running: [View](https://your-screenshot-url.com/minikube-start.png)
*   DVWA Deployment: [View](https://your-screenshot-url.com/dvwa-deploy.png)
*   Login Page: [View](https://your-screenshot-url.com/dvwa-login.png)
*   Command Injection Result: [View](https://your-screenshot-url.com/command-injection.png)
*   SQL Injection Result: [View](https://your-screenshot-url.com/sql-injection.png)
*   Stored XSS Alert: [View](https://your-screenshot-url.com/xss-alert.png)

🛠 Setup Steps
--------------

### 1\. Start Minikube

    minikube start --driver=docker

### 2\. Create `dvwa.yaml`

Save the following content into a file named `dvwa.yaml`:

    apiVersion: v1
    kind: Namespace
    metadata:
      name: dvwa
    
    ---
    apiVersion: v1
    kind: Service
    metadata:
      name: dvwa
      namespace: dvwa
    spec:
      type: NodePort
      selector:
        app: dvwa
      ports:
        - port: 80
          targetPort: 80
          nodePort: 30001
    
    ---
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: dvwa
      namespace: dvwa
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: dvwa
      template:
        metadata:
          labels:
            app: dvwa
        spec:
          containers:
            - name: dvwa
              image: vulnerables/web-dvwa
              ports:
                - containerPort: 80
    

### 3\. Apply the Configuration

    kubectl apply -f dvwa.yaml

### 4\. Access DVWA

    minikube service dvwa -n dvwa

Open the given URL (usually `http://192.168.49.2:30001`) in your browser.

### 5\. Login

*   Username: `admin`
*   Password: `password`

Go to **DVWA Security** tab and set security level to `LOW`.

🧪 Attack Demonstrations
------------------------

### 1️⃣ Command Injection

*   Go to: **DVWA → Command Injection**
*   Input: `127.0.0.1; whoami`
*   Result: Displays container user like `www-data`

![Command Injection Screenshot](https://your-screenshot-url.com/command-injection.png)

### 2️⃣ SQL Injection

*   Go to: **DVWA → SQL Injection**
*   Input: `1' OR '1'='1`
*   Result: Database returns all users bypassing login filters

![SQL Injection Screenshot](https://your-screenshot-url.com/sql-injection.png)

### 3️⃣ Stored Cross Site Scripting (XSS)

*   Go to: **DVWA → Stored XSS**
*   Input:
    
        <script>alert('XSS attack!')</script>
    
*   Result: Alert pops up when viewing the page

![Stored XSS Screenshot](https://your-screenshot-url.com/xss-alert.png)

🧹 Cleanup
----------

To remove the deployment and stop the cluster:

    kubectl delete namespace dvwa
    minikube stop

📚 References
-------------

*   [DVWA GitHub](https://github.com/ethicalhack3r/DVWA)
*   [OWASP Attack References](https://owasp.org/www-community/attacks/)

* * *

Created for security testing & education. Use responsibly in isolated environments only.

📌 Author
-------------
# 
