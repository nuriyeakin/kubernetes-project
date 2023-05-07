- In this project, We will learn how to deploy a Ml model on the Kubernetes.
- Used technologies
    - Docker
    - Kubernetes
    - FastApi

### 1. Pip Install
```
pip install -r requirements.txt
```   

### 2. Uvicorn

```
uvicorn main:app --host 0.0.0.0 --port 8000
```

### 3. Docker İmage Build

```
docker image build -t ml_fastapi .
```

### 4. Docker Container Build

```
docker run --rm --name ml_fastapi -p 8000:8000 -d ml_fastapi
```
#### OR

```
docker run -d --name ml_fastapi -p 8000:8000 ml_fastapi
```

### 5. Docker Hub Repository

- You have to create one repository on your account with this name "ml-prediction-with-fastapi"

### 6. Docker Tag
```
docker tag "image_name" "your_docker_repository_name"
```
```
docker tag ml_fastapi hanoguz00/ml-prediction-with-fastapi
```
```
docker image ls
```

### 7. Docker push image with tag

```
docker push hanoguz00/ml-prediction-with-fastapi
```

### 8. Create kubernetes.yaml for kubernetes

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: k8s-project1-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: fast-api
  template:
    metadata:
      labels:
        app: fast-api
    spec:
      containers:
      - name: fast-api
        image: hanoguz00/ml-prediction-with-fastapi
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 8000
---
apiVersion: v1
kind: Service
metadata:
  name: k8s-project1-service
spec:
  selector:
    app: fast-api
  ports:
  - port: 8000
    targetPort: 8000
  type: LoadBalancer
```

```
kubectl apply -f kubernetes.yaml
```

### 9. Create kubernetes pods

```
kubectl apply -f k8sProject1.yaml
```

```
kubectl get deployment
```

```
kubectl get service 
```

### 10. Activate service.

- Minikube will assign an External IP address to the ‘fast-api-service’.
```
minikube service k8s-project1-service
```