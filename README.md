# **Docker Web Server Deployment Lab**

[![Linux](https://img.shields.io/badge/OS-Ubuntu-blue?logo=ubuntu)](https://ubuntu.com/)
[![Docker](https://img.shields.io/badge/Tool-Docker-blue?logo=docker)](https://www.docker.com/)
[![Web Server](https://img.shields.io/badge/Application-Nginx-green?logo=nginx)](https://nginx.org/)
[![System Administration](https://img.shields.io/badge/Focus-System%20Administration-orange)](https://en.wikipedia.org/wiki/System_administrator)
[![Containerization](https://img.shields.io/badge/Skill-Containerization-lightgrey)](https://en.wikipedia.org/wiki/OS-level_virtualisation)
[![Port Binding](https://img.shields.io/badge/Concept-Port%20Binding-red)](https://en.wikipedia.org/wiki/Port_(computer_networking))

---

## **Project Overview**

This project demonstrates how to **pull, run, verify, inspect, and clean up** a containerized web application using **Docker** on **Ubuntu**.  
The exercise aligns with an on-the-job scenario for a **junior security analyst** testing containerized environments for sandbox analysis and documenting configurations for future vulnerability scanning labs.

---

## **Learning Objectives**

1. Understand how to **pull and run Docker containers** from Docker Hub.  
2. Deploy a **basic web application** and test accessibility.  
3. **List, inspect, stop, and remove** containers using core Docker CLI commands.  
4. Reinforce foundational ops skills: **directory organization, file management, text stream processing, and permissions** (supporting docs and logging).

---

## **Scenario**

You are a **junior security analyst** tasked with quickly deploying a **sample Nginx web server** in Docker, validating access via `localhost`, capturing key container attributes for documentation, and then performing a clean teardown to leave the host in a known-good state. These steps simulate standing up ephemeral services for **sandboxing and analysis**.

---

## **Step 1: Launch Ubuntu VM & Open Terminal**

Open your Ubuntu VM and start a terminal session (Ctrl+Alt+T).

---

## **Step 2: Verify Docker Installation & Service Status**

```bash
docker --version
```

Expected: prints the installed Docker version.

Check the daemon status; enable and start if needed:

```bash
sudo systemctl status docker
sudo systemctl start docker
sudo systemctl enable docker
```

**Outcome:** Docker service is **active (running)**.

---

## **Step 3: Pull the Official Nginx Image**

```bash
sudo docker pull nginx
```

**Result:** Downloads `library/nginx:latest` from Docker Hub.  
*Evidence:* Include screenshot of successful layer pulls and final digest.

[Insert Image Here – *Screenshot: docker pull nginx shows layers complete and repo digest*]

---

## **Step 4: Run the Container (Detached) and Map Port 80**

```bash
sudo docker run -d -p 80:80 --name webserver nginx
```

- `-d` runs in the background.  
- `-p 80:80` binds **host 80 → container 80** (HTTP).  
- `--name webserver` sets a readable name.

Confirm it’s up:

```bash
sudo docker ps
```

[Insert Image Here – *Screenshot: docker ps shows webserver Up with 0.0.0.0:80->80/tcp*]

---

## **Step 5: Verify the Web Server via localhost**

Use a browser (http://localhost) **or** curl:

```bash
curl localhost
```

Expected: the **default Nginx landing page HTML** (`Welcome to nginx!`).

[Insert Image Here – *Screenshot: curl localhost outputs Nginx HTML*]

---

## **Step 6: Inspect the Running Container**

Capture full attributes (IDs, image, mounts, network, ports):

```bash
sudo docker inspect webserver
```

Tip: For a concise view, filter select fields (optional):

```bash
sudo docker inspect -f 'Name={{.Name}} | Image={{.Image}} | IP={{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' webserver
```

Take a screenshot of the key attributes for documentation.

---

## **Step 7: Stop, Remove Container, and Clean the Image**

Graceful stop and removal:

```bash
sudo docker stop webserver
sudo docker rm webserver
```

(Optional) Remove the image to reclaim space:

```bash
sudo docker rmi nginx
```

Verify a clean slate:

```bash
sudo docker ps -a
```

[Insert Image Here – *Screenshot: docker ps -a shows no containers*]  
[Insert Image Here – *Screenshot: stop/rm/rmi deletions printed to terminal*]

---

## **Evidence (Screenshots to Include)**

1. **Pull Completed:** `sudo docker pull nginx` with layers and final digest.  
2. **Container Running:** `sudo docker ps` shows `webserver` **Up** with `0.0.0.0:80->80/tcp`.  
3. **Accessibility Verified:** `curl localhost` returns Nginx default HTML.  
4. **Cleanup Confirmed:** `sudo docker stop/rm/rmi` output and `sudo docker ps -a` empty list.

---

## **Key Achievements**

- **Containerized Web App Deployed:** Pulled and launched **Nginx** with correct **port binding** on host TCP/80.  
- **Service Reachability Validated:** Confirmed HTTP response from `localhost` using CLI.  
- **Operational Insight Captured:** Inspected container metadata for future infra and security analysis.  
- **Environment Hygiene Practiced:** Performed **stop → remove → image prune** workflow to maintain a clean host.

---

## **Skills Demonstrated**

- **Docker Fundamentals:** image lifecycle, detached runs, port publishing, basic inspection.  
- **Linux Ops:** systemd service checks, CLI diagnostics, output interpretation.  
- **Security-Aware Workflow:** ephemeral deployment for sandboxing and controlled teardown.  
- **Documentation Discipline:** stepwise evidence with screenshots for reproducibility.

---

## **Summary**

This lab validates the core workflow for **rapid container deployment and validation** on Ubuntu using Docker.  
By completing the steps, I demonstrated the ability to **stand up a service**, **verify network exposure**, **extract container attributes**, and **tear down cleanly**—all essential for **SOC labs**, **sandbox testing**, and **DevOps-adjacent** operations.

---

## **Recommendations for Future Enhancements**

1. **Bind a Host Content Volume:**  
   Map a local directory to `/usr/share/nginx/html` to serve custom pages.  
   ```bash
   sudo docker run -d -p 80:80 -v $(pwd)/site:/usr/share/nginx/html:ro --name webserver nginx
   ```

2. **Add Basic Logging/Artifacts:**  
   Redirect `docker inspect` and `docker ps` outputs to a `logs/` folder for audit trails.

3. **Container Security Baseline:**  
   Run as a non-root user where feasible, set read-only FS, and add resource limits (`--cpus`, `--memory`).

4. **Compose File:**  
   Convert commands into a minimal `docker-compose.yml` for repeatable spin-ups.

5. **Healthcheck & Monitoring:**  
   Add an HTTP healthcheck and collect basic metrics (e.g., `docker stats`, cAdvisor) during runtime.

---
