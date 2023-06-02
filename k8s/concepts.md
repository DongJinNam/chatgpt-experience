# k8s concepts

### concepts with example codes

Sure! Here are some key concepts in Kubernetes (K8s) that developers should know along with example codes:

1. Pods:
A Pod is the smallest unit in Kubernetes and represents a single instance of a running process. It can contain one or more containers that are tightly coupled and share resources. Here's an example of creating a Pod definition in Kubernetes using YAML:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: my-container
    image: nginx:latest
```

2. Deployments:
Deployments manage the lifecycle of replicated Pods and provide declarative updates and rollbacks. They ensure the desired number of Pods are running and can handle scaling, rolling updates, and rollbacks. Here's an example of a Deployment definition:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: nginx:latest
```

3. Services:
Services provide network connectivity to a set of Pods and enable load balancing and service discovery within the cluster. They allow Pods to communicate with each other using a stable network address. Here's an example of a Service definition:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
```

4. ConfigMaps:
ConfigMaps store configuration data as key-value pairs and can be used to decouple configuration from the application. Here's an example of creating a ConfigMap:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-config
data:
  MY_CONFIG_KEY: my-config-value
```

5. Secrets:
Secrets store sensitive data such as passwords, API keys, or certificates. They are base64-encoded and can be used to securely provide confidential information to applications. Here's an example of creating a Secret:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: my-secret
type: Opaque
data:
  username: base64-encoded-username
  password: base64-encoded-password
```

6. Namespaces:
Namespaces provide a way to partition resources within a cluster. They can be used to isolate applications or different stages of a development pipeline. Here's an example of creating a Namespace:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
```

7. Persistent Volumes (PV):
Persistent Volumes are storage units in Kubernetes that exist beyond the lifecycle of a Pod. They provide a way to store data and decouple it from Pods. Here's an example of creating a Persistent Volume:

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
  persistentVolumeReclaimPolicy: Retain
  storageClassName: my-storage-class
  hostPath:
    path: /data
```

8. Persistent Volume Claims (PVC):
Persistent Volume Claims are requests for storage resources by Pods. They bind to Persistent Volumes and provide access to storage. Here's an example of creating a Persistent Volume Claim:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
  storageClassName: my-storage-class
```

9. Ingress:
Ingress exposes HTTP and HTTPS routes to services within a cluster. It provides a way to configure external access to services. Here's an example of creating an Ingress:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-service
                port:
                  number: 80
```
