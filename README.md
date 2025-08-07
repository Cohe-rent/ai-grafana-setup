# Grafana Setup

This setup was AI-generated using OpenRouter and the meta-llama/llama-3-8b-instruct model.

```
Here is a minimal and production-ready Docker Compose configuration to run Grafana on port 3000 with persistent volume support and default admin credentials:
```
version: '3'

services:
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    environment:
      - GF_SERVER_EXTERNAL_URL=http://grafana:3000
      - GF_SERVER_ADMIN_USER=admin
      - GF_SERVER_ADMIN_PASSWORD=change-me
    volumes:
      - grafana-data:/var/lib/grafana

volumes:
  grafana-data:
    driver: local
    driver_opts:
      type: none
      device: ${PWD}/grafana-data
      o: bind
```
Let me explain what each part does:

* `version`: specifies the Docker Compose version.
* `services`: defines the services to run.
* `grafana`: specifies the service name and image to use (latest stable Grafana image).
* `ports`: maps port 3000 on the host machine to port 3000 in the container.
* `environment`: sets environment variables for the Grafana service. These variables configure the admin user and password, and the external URL.
* `volumes`: specifies the persistent volume to use for storing Grafana data.
* `grafana-data`: defines the name of the volume, and uses the `local` driver to store the data in a directory named `grafana-data` in the current working directory. The `driver_opts` section specifies the mount options.

To use this configuration, create a new file named `docker-compose.yml` and copy the contents above into it. Then, run `docker-compose up` to start the Grafana service, and access it at `http://localhost:3000`.
```
