# Raspberry Pi 5 Cybersecurity Home Lab

An isolated cybersecurity practice lab running on a Raspberry Pi 5. Kali Linux is the attacker machine, and Docker containers (Ubuntu 22.04 and OWASP Juice Shop) are the targets.

![Lab hardware](images/lab-hardware.jpg)

## Lab overview

| Role | System | Details |
|---|---|---|
| Host / attacker | Kali Linux Rolling | Runs on Raspberry Pi 5 |
| Target 1 | Ubuntu 22.04.5 LTS | Docker container `cyberlab-ubuntu` |
| Target 2 | OWASP Juice Shop | Docker container `juice-shop`, served at `http://localhost:3000` |

![Lab running](images/lab-running.png)

*Top: Kali Linux. Bottom left: Ubuntu 22.04 container. Right: Juice Shop started and open in the browser.*

## Hardware

- Raspberry Pi 5 with active cooler, in a 3D-printed case
- USB hub
- External storage drive
- USB Wi-Fi adapter

## Folder structure

```
Cyberlab/
├── README.md
├── docker/
│   └── docker-compose.yml
└── images/
```

## Setup steps

### 1. Install Docker on Kali
```bash
sudo apt update
sudo apt install -y docker.io docker-compose
sudo systemctl enable --now docker
```

### 2. Pull the images
```bash
docker pull ubuntu:22.04
docker pull bkimminich/juice-shop
```

### 3. Create and start the containers
```bash
docker run -dit --name cyberlab-ubuntu ubuntu:22.04
docker run -d --name juice-shop -p 127.0.0.1:3000:3000 bkimminich/juice-shop
```
Or use the compose file: `docker compose -f docker/docker-compose.yml up -d`

### 4. Open a shell in the Ubuntu target
```bash
docker exec -it cyberlab-ubuntu bash
```

### 5. Open Juice Shop
Browse to `http://localhost:3000`.

### 6. Verify the operating systems
```bash
cat /etc/os-release
```
Run this on Kali and inside the Ubuntu container.

## Safety

- All targets run locally in Docker and are exposed to this machine only (`127.0.0.1`).
- Only test systems you own or have written permission to test.
- No real data or accounts are used in the lab.

## Status

Lab environment is built and running. Security testing has not started yet.

## Planned next

- Network reconnaissance with Nmap against the lab targets
- Work through Juice Shop challenges
- Add notes for each completed challenge

## Tools

Raspberry Pi 5, Kali Linux, Docker, Ubuntu 22.04, OWASP Juice Shop
