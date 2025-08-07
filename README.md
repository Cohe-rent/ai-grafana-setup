# Grafana Setup

This setup was AI-generated using OpenRouter and the meta-llama/llama-3-8b-instruct model.

```
Here are the required files to deploy Grafana using Terraform and Docker:

**main.tf**
```terraform
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "2.3.0"
    }
  }
}

provider "docker" {
  host = "unix:///var/run/docker.sock"
}

resource "docker_image" "grafana" {
  name   = "grafana/grafana:latest"
}

resource "docker_container" "grafana" {
  name  = "grafana"
  image = docker_image.grafana.latest

  ports {
    internal = 3000
    external = 3000
  }

  env = [
    "GF_SERVER_HTTP_PORT=3000",
    "GF_SECURITY_ADMIN_USER=${var.admin_user}",
    "GF_SECURITY_ADMIN_PASSWORD=${var.admin_password}",
  ]

  volumes {
    volume {
      name        = "grafana-data"
      driver      = "local"
      driver_opts = {
        type    = "none"
        device  = "/tmp/grafana-data"
        o       = "bind"
      }
    }
  }
}

output "grafana_url" {
  value = "http://localhost:3000"
}
```

**variables.tf**
```terraform
variable "admin_user" {
  type        = string
  description = "Grafana admin user"
}

variable "admin_password" {
  type        = string
  description = "Grafana admin password"
  sensitive   = true
}
```

**outputs.tf**
```terraform
output "grafana_url" {
  value       = docker_container.grafana.ip
  description = "Grafana URL"
}
```

**docker-compose.yml** (useful for local development)
```yml
version: '3'

services:
  grafana:
    image: grafana/grafana:latest
    environment:
      - GF_SERVER_HTTP_PORT=3000
      - GF_SECURITY_ADMIN_USER=${GRAFANA_ADMIN_USER}
      - GF_SECURITY_ADMIN_PASSWORD=${GRAFANA_ADMIN_PASSWORD}
    ports:
      - "3000:3000"
    volumes:
      - grafana-data:/var/lib/grafana
    depends_on:
      - postgres

volumes:
  grafana-data:
```

**README.md**
```
# Grafana Deployment using Terraform and Docker

## Prerequisites

* Terraform 1.0.0 or later
* Docker 20.10.0 or later
* Docker Compose 2.4.0 or later (for local development)

## Configuration

1. Set the following environment variables:
	* `GRAFANA_ADMIN_USER`: your desired admin username
	* `GRAFANA_ADMIN_PASSWORD`: your desired admin password
2. Run `terraform init` to download the required providers
3. Run `terraform apply` to provision the Grafana instance
4. Access Grafana at `http://localhost:3000` using the admin user and password you set
5. For local development, use `docker-compose up` to start the Grafana container

## Notes

* This setup uses a local volume for persistent storage
* The `docker-compose.yml` file is only used for local development and can be ignored for production use
```

To use the setup:

1. Create a file called `.env` in the same directory with the following contents:
```makefile
GRAFANA_ADMIN_USER=your_username
GRAFANA_ADMIN_PASSWORD=your_password
```
2. Run `terraform init` to download the required providers.
3. Run `terraform apply` to provision the Grafana instance.
4. Access Grafana at `http://localhost:3000` using the admin user and password you set.

Note: Make sure to replace `your_username` and `your_password` with your desired admin credentials.
```
