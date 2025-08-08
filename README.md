# Grafana Setup

This setup was AI-generated using OpenRouter and the meta-llama/llama-3-8b-instruct model.

```
Here is a minimal and production-ready Docker Compose configuration to run Grafana on port 3000 with persistent volume support and default admin credentials as environment variables:
```
version: '3'

services:
  grafana:
    image: grafana/grafana:latest
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=${GF_SECURITY_ADMIN_USER}
      - GF_SECURITY_ADMIN_PASSWORD=${GF_SECURITY_ADMIN_PASSWORD}
    volumes:
      - ./grafana-data:/var/lib/grafana
    restart: always

volumes:
  grafana-data:
    driver: local
```
Let me explain what each section does:

* `image: grafana/grafana:latest`: Uses the latest stable Grafana image from the official Docker Hub repository.
* `ports: "3000:3000"`: Maps port 3000 on the host machine to port 3000 inside the container, so you can access Grafana at `http://localhost:3000`.
* `environment:` sets environment variables for the Grafana container:
	+ `GF_SECURITY_ADMIN_USER` and `GF_SECURITY_ADMIN_PASSWORD` are set to environment variables that you can define when running the container, using the `-e` flag (e.g. `docker-compose up -e GF_SECURITY_ADMIN_USER=myuser -e GF_SECURITY_ADMIN_PASSWORD=mypassword`).
* `volumes:` mounts a persistent volume named `grafana-data` at `/var/lib/grafana` inside the container. This is where Grafana stores its data, such as dashboards and user settings.
* `restart: always` ensures that the container is restarted if it crashes or is stopped.

Note that you'll need to create a `.env` file with the environment variables defined, like this:
```
GF_SECURITY_ADMIN_USER=myuser
GF_SECURITY_ADMIN_PASSWORD=mypassword
```
Then, you can run the container with `docker-compose up` or `docker-compose up -e GF_SECURITY_ADMIN_USER=myuser -e GF_SECURITY_ADMIN_PASSWORD=mypassword`.
```
