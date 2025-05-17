A sample app on **Minikube** and perform **common Kubernetes operations** with code.

---

## 🧱 Prerequisites

* ✅ Minikube installed
* ✅ kubectl installed
* ✅ Docker installed (optional for local image builds)

---

## 🚀 Step 1: Start Minikube

```bash
minikube start
```

---

## 📦 Step 2: Sample Node.js App (Dockerized)

### `Dockerfile`

```dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["node", "index.js"]
```

### `index.js`

```javascript
const express = require('express');
const app = express();
app.get('/', (req, res) => res.send('Hello from Minikube!'));
app.listen(3000, () => console.log('App listening on port 3000'));
```

### `package.json`

```json
{
  "name": "minikube-app",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": { "start": "node index.js" },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

---

## 🛠 Step 3: Build Docker Image in Minikube

Use Minikube’s Docker daemon:

```bash
eval $(minikube docker-env)
docker build -t minikube-node-app .
```

---

## 🧾 Step 4: Kubernetes YAMLs

### `deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: node-app
  template:
    metadata:
      labels:
        app: node-app
    spec:
      containers:
      - name: node-container
        image: minikube-node-app
        ports:
        - containerPort: 3000
```

### `service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: node-service
spec:
  type: NodePort
  selector:
    app: node-app
  ports:
  - port: 80
    targetPort: 3000
    nodePort: 30008
```

---

## 📤 Step 5: Deploy to Minikube

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

## 🔍 Step 6: Access the App

```bash
minikube service node-service --url
```

---

## ⚙️ Common Minikube/K8s Operations

### View Pods

```bash
kubectl get pods
```

### View Services

```bash
kubectl get svc
```

### Logs of a Pod

```bash
kubectl logs <pod-name>
```

### Exec into Pod

```bash
kubectl exec -it <pod-name> -- /bin/sh
```

### Delete All Resources

```bash
kubectl delete -f deployment.yaml
kubectl delete -f service.yaml
```

---
