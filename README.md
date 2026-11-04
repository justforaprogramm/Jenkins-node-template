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
git clone https://github.com/justforaprogramm/Jenkins-node-template.git
cd ./Jenkins-node-template
nano .env
```

**1. Create Jenkins Node**
Create a new agent in your Jenkins UI. Remember the **name** of the agent and copy the **secret** that Jenkins displays at the end.

**2. Edit `.env` file**
Edit a file named `.env` in the root directory of this repository with the following content and adjust the values:

```env
# The URL to your Jenkins server (e.g., [http://jenkins.local:8080](http://jenkins.local:8080))
JENKINS_URL=http://<YOUR_JENKINS_URL>

# The secret that Jenkins displays after creating the node
JENKINS_SECRET=<YOUR_JENKINS_SECRET>

# The name of the agent as configured in Jenkins
JENKINS_AGENT_NAME=<YOUR_AGENT_NAME>

# The group ID (GID) of the Docker group on the HOST system
DOCKER_GID=<GID>
```

**3. adjust user and build on node server**
```
sudo mkdir -p /home/jenkins/agent
sudo chown -R 1000:1000 /home/jenkins/agent
docker compose up -d --build
```