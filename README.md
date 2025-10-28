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

## **Objectives**

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

---

## **Step 4: Run the Container (Detached) and Map Port 80**

```bash
sudo docker run -d -p 80:80 --name webserver nginx
```

<img width="991" height="269" alt="image" src="https://github.com/user-attachments/assets/0ad5459c-3ffe-4e1a-ab34-72dac54146eb" />

- `-d` runs in the background.  
- `-p 80:80` binds **host 80 → container 80** (HTTP).  
- `--name webserver` sets a readable name.

Confirm it’s up:

```bash
sudo docker ps
```

<img width="1289" height="92" alt="image" src="https://github.com/user-attachments/assets/2a0ff2c6-fbb6-4ecf-8a53-7030658e14d6" />

---

## **Step 5: Verify the Web Server via localhost**

Use a browser (http://localhost) **or** curl:

```bash
curl localhost
```

<img width="761" height="515" alt="image" src="https://github.com/user-attachments/assets/94d3f2a3-7317-4ca4-8fa0-575506df903c" />

---

## **Step 6: Inspect the Running Container**

Capture full attributes (IDs, image, mounts, network, ports):

```bash
sudo docker inspect webserver
```

<img width="1843" height="937" alt="image" src="https://github.com/user-attachments/assets/b2ae289f-fea4-48b8-aeff-cb483db72570" />
*Theres a lot more than this but that would be too many screenshots to upload here)

For a concise view, you can filter select fields:

```bash
sudo docker inspect -f 'Name={{.Name}} | Image={{.Image}} | IP={{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' webserver
```

---

## **Step 7: Stop, Remove Container, and Clean the Image**

Graceful stop and removal:

```bash
sudo docker stop webserver
sudo docker rm webserver
```

**Bonus** Remove the image to reclaim space:

```bash
sudo docker rmi nginx
```

<img width="878" height="229" alt="image" src="https://github.com/user-attachments/assets/c53897b1-6d64-4ff8-b7d0-532f64f4fdbb" />

Verify a clean slate:

```bash
sudo docker ps -a
```

<img width="715" height="69" alt="image" src="https://github.com/user-attachments/assets/7a7f3541-2381-4c0e-b0af-34b564f52f83" />

---

## **Summary**

This lab validates the core workflow for **rapid container deployment and validation** on Ubuntu using Docker.  
By completing the steps, I demonstrated the ability to **stand up a service**, **verify network exposure**, **extract container attributes**, and **tear down cleanly**—all essential for **SOC labs**, **sandbox testing**, and **DevOps-adjacent** operations.

---
