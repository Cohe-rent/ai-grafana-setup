# Grafana Setup

This setup was AI-generated using OpenRouter and the meta-llama/llama-3-8b-instruct model.

```
Here is an example of production-grade infrastructure code to deploy Grafana on port 3000 using Docker Compose:

**docker-compose.yml**
```yaml
version: '3'

services:
  grafana:
    image: grafana/grafana:8.3.4
    ports:
      - "3000:3000"
    environment:
      - GF_SERVER_HTTP_PORT=3000
      - GF_SERVER_ROOT_URL=%(protocol)s://%(domain)s:%(http_port)s/
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=password
    depends_on:
      - db
    volumes:
      - grafana-config:/etc/grafana
      - grafana-data:/var/lib/grafana

volumes:
  grafana-config:
  grafana-data:
```
Let me explain the configuration:

* We use version 3 of the Docker Compose syntax.
* We define a single service named `grafana` that uses the official Grafana 8.3.4 image.
* We map port 3000 on the host machine to port 3000 in the container using the `ports` directive.
* We set environment variables for the Grafana server, including the HTTP port, root URL, and admin credentials.
* We use the `depends_on` directive to ensure that the Grafana service depends on the `db` service (which is not defined in this example, but you would need to define it if you want to use a separate database).
* We use two volumes: `grafana-config` and `grafana-data`. The `grafana-config` volume persists the Grafana configuration files, while the `grafana-data` volume persists the Grafana data (e.g., dashboards, alerts, etc.).

**Best practices:**

* We use a specific version of the Grafana image to ensure consistency and reproducibility.
* We use environment variables to configure the Grafana server, which makes it easier to manage and maintain.
* We use volumes to persist data and configuration, which ensures that data is not lost in case the container is restarted or deleted.
* We use the `depends_on` directive to ensure that services are started in the correct order.

**Kubernetes deployment (if you prefer):**

Here is an example of a Kubernetes deployment YAML file that deploys Grafana on port 3000:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana
        image: grafana/grafana:8.3.4
        ports:
        - containerPort: 3000
        env:
        - name: GF_SERVER_HTTP_PORT
          value: "3000"
        - name: GF_SERVER_ROOT_URL
          value: "%(protocol)s://%(domain)s:%(http_port)s/"
        - name: GF_SECURITY_ADMIN_USER
          value: "admin"
        - name: GF_SECURITY_ADMIN_PASSWORD
          value: "password"
        volumeMounts:
        - name: grafana-config
          mountPath: /etc/grafana
        - name: grafana-data
          mountPath: /var/lib/grafana
      volumes:
      - name: grafana-config
        persistentVolumeClaim:
          claimName: grafana-config-pvc
      - name: grafana-data
        persistentVolumeClaim:
          claimName: grafana-data-pvc
```
This deployment YAML file defines a single replica of the Grafana container, maps port 3000 on the host machine to port 3000 in the container, sets environment variables, and mounts two persistent volumes (PVCs) for configuration and data persistence.

**Networking:**

In both examples, we use the `ports` directive to expose port 3000 on the host machine. This allows you to access Grafana from outside the container or pod.

**Persistence:**

In both examples, we use volumes to persist data and configuration. In the Docker Compose example, we use two named volumes (`grafana-config` and `grafana-data`) that persist data and configuration. In the Kubernetes example, we use two persistent volume claims (PVCs) (`grafana-config-pvc` and `grafana-data-pvc`) to persist data and configuration.

Note that you may need to adjust the configuration based on your specific use case and requirements.
```
