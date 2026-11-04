# Jenkins Inbound Agent (with Docker Support)

This repository contains a simple setup for a Jenkins Inbound Agent running as a Docker container. 

*Note: I originally created this setup for my private use. However, since it doesn't contain any sensitive data and might be useful to others, I have made it publicly available under the MIT License.*

## What does this setup do?

The setup uses the official `jenkins/inbound-agent` image and extends it with the Docker CLI and the Docker Compose plugin. By mounting the Docker socket (`/var/run/docker.sock`) from the host system, the Jenkins agent itself can build and start Docker containers (known as "Docker-out-of-Docker").

## Prerequisites

- **Docker** and **Docker Compose** must be installed on the host system.
- You need a running Jenkins controller where you can create a new agent (node).

## Setup & Configuration

**0. Clone the repository**
Clone the project to your server and open the `.env` file for editing:
```bash
git clone [https://github.com/justforaprogramm/Jenkins-node-template.git](https://github.com/justforaprogramm/Jenkins-node-template.git)
cd ./Jenkins-node-template
nano .env
```