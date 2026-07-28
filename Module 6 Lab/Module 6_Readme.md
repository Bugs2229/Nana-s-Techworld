# Module 6 Lab

# Artifact Repository Manager with Nexus
 
Central artifact management setup for two teams sharing a company-wide Sonatype Nexus 3 instance — one team publishes npm packages (Node.js app), the other publishes Maven jars (Java app). Includes automation to fetch and run the latest artifact on a deployment server.
 
## Overview
 
- **Nexus 3** installed on a Linux server, acting as the single source of truth for build artifacts across projects.
- **npm hosted repository** for Project 1's Node.js app, with a scoped user/role for publish access.
- **Maven hosted repository** for Project 2's Java app, with a scoped user/role for publish access.
- **Read-only deploy user** with access to both repositories, used by a DigitalOcean droplet to pull the latest build.
- **Automation script** that queries the Nexus REST API for the latest npm artifact, downloads it, extracts it, and runs the app.
## Architecture
 
```
                ┌───────────────────────┐
                │   Nexus 3 (server)    │
                │                       │
                │  npm-hosted-project1  │◄── team1 user (publish)
                │  maven-hosted-project2│◄── team2 user (publish)
                └───────────┬───────────┘
                            │ REST API (search/download)
                            ▼
                ┌───────────────────────┐
                │  DigitalOcean droplet │
                │  deploy-user (read)   │
                │  deploy.sh automation │
                └───────────────────────┘
```
 
## Prerequisites
 
- A Linux server for Nexus (2 vCPU / 4GB RAM minimum)
- Java (OpenJDK 8 or 11)
- Node.js and npm (for the Project 1 app)
- Maven (for the Project 2 app)
- A DigitalOcean droplet for deployment
- `curl` and `jq` on the droplet
## Setup
 
### 1. Install Nexus
 
```bash
sudo apt update && sudo apt install openjdk-11-jre-headless -y
wget https://download.sonatype.com/nexus/3/latest-unix.tar.gz
tar -xvzf latest-unix.tar.gz
sudo mv nexus-* /opt/nexus3
sudo mv sonatype-work /opt/sonatype-work
```
 
Set `run_as_user` in `/opt/nexus3/bin/nexus.rc`, then start Nexus:
 
```bash
sudo /opt/nexus3/bin/nexus start
```
 
Open port `8081` and log in at `http://<server-ip>:8081` using the password in `/opt/sonatype-work/nexus3/admin.password`.
 
### 2. Create repositories
 
- **npm-hosted-project1** — recipe `npm (hosted)`, own blob store
- **maven-hosted-project2** — recipe `maven2 (hosted)`, version policy `Mixed`, layout `Strict`
### 3. Create scoped users
 
| User | Role | Access |
|---|---|---|
| `team1-user` | `npm-project1-role` | browse/read/add/edit on `npm-hosted-project1` |
| `team2-user` | `maven-project2-role` | browse/read/add/edit on `maven-hosted-project2` |
| `deploy-user` | `deploy-role` | browse/read on both repos |
 
## Publishing artifacts
 
**Node.js (npm):**
 
```bash
npm login --registry=http://<nexus-ip>:8081/repository/npm-hosted-project1/
npm publish --registry=http://<nexus-ip>:8081/repository/npm-hosted-project1/
```
 
**Java (Maven)** — add a `distributionManagement` entry to `pom.xml` and matching credentials to `~/.m2/settings.xml`, then:
 
```bash
mvn clean package
mvn deploy
```
 
## Deploying to the droplet
 
Manual fetch via the Nexus REST Search API:
 
```bash
curl -u deploy-user:<password> \
  "http://<nexus-ip>:8081/service/rest/v1/search/assets?repository=npm-hosted-project1&sort=version&direction=desc" \
  | jq -r '.items[0].downloadUrl'
```
 
Automated fetch + run:
 
```bash
./deploy.sh
```
 
See [`deploy.sh`](./deploy.sh) — pulls the latest artifact from `npm-hosted-project1`, extracts it, installs dependencies, and starts the app.
 
## Repo contents
 
```
.
├── README.md
├── deploy.sh              # automated fetch-and-run script for the droplet
└── docs/
    └── nexus-exercises-guide.md   # full step-by-step exercise walkthrough
```
 
## Notes
 
- All Nexus users follow least-privilege access: publish rights are scoped per team/repo, and the deploy user is read-only.
- Replace `<nexus-ip>`, `<password>`, and repository names with your own values before running any commands.
 

