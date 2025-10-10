# 🏠 My Homelab Docker Stack

This repository contains the Docker Compose configuration for my personal Homelab setup. It includes a variety of self-hosted services for media management, file syncing, smart home automation, and utilities.

Feel free to explore and adapt these files for your own Homelab environment!

---

## 💡 Introduction

The `docker-compose.yml` file defines a stack of interconnected containers, orchestrated to provide various functionalities for my home network. The configuration is designed to be easily reproducible and maintainable using **Docker Compose**.

### Key Configurations
* **Common Environment Variables (`x-common-env`)**: Centralized definition for `PUID`, `PGID` (set to `1000`), and `TZ` (`Asia/Ho_Chi_Minh`).
* **External Network**: All containers utilize an external network named `docker_homelab` for easy integration with a reverse proxy (if one is used).

## ⚙️ Prerequisites

Before running this stack, you need to have:
1.  **Docker** installed on your host machine.
2.  **Docker Compose** (or the modern `docker compose` CLI).
3.  **Create the Docker Network**:

    ```bash
    docker network create docker_homelab
    ```

4.  **Host Directory Setup**: Ensure that the volume paths defined in the compose file (e.g., `/mnt/mmc1-4/docker/`, `/mnt/data_md0/`) exist on your host and have the correct permissions (matching the defined `PUID`/`PGID`).

## 🗃️ Directory Structure and Volumes

The configuration follows a common Homelab practice of separating configuration files from main media/data storage.

| Host Path | Purpose | Example Services |
| :--- | :--- | :--- |
| `/mnt/docker/config` | **Configuration Storage** (Application settings, SQLite DBs) | `jellyfin`, `radarr`, `sonarr`, `homeassistant` |
| `/mnt/data/` | **Main Data Storage** (Media, Downloads, Shared Files) | `jellyfin`, `navidrome`, `qbittorrent`, `filebrowser` |
| `/var/run/docker.sock` | **Docker Daemon Access** | `homepage`, `watchtower` |

## 🚀 Usage

1.  Place the `docker-compose.yml` file in your desired working directory.
2.  Review and adjust the **Volume Mappings** and **Ports** in `docker-compose.yml` to fit your specific host paths and port availability.
3.  Deploy the stack:

    ```bash
    docker compose up -d
    ```

4.  Check the status of your running containers:

    ```bash
    docker compose ps
    ```

## 🛠️ Services Overview

| Service | Category | Functionality | Host Port |
| :--- | :--- | :--- | :--- |
| **homepage** | Dashboard | Centralized hub for accessing all self-hosted services. | `8080` |
| **filebrowser** | File Management | Web interface for browsing and managing files. | `8088` |
| **syncthing** | Syncing | Decentralized file synchronization between devices. | `8384` |
| **jellyfin** | Media Server | Streaming server for movies and TV shows. | `8096` |
| **navidrome** | Music Streaming | Web-based music server and streamer. | `4533` |
| **qbittorrent** | Download Client | Torrent client with a powerful web UI. | `8089` |
| **prowlarr** | Media Automation | Indexer manager for Radarr/Sonarr. | `9696` |
| **radarr** | Media Automation | Automated movie management and downloading. | `7878` |
| **sonarr** | Media Automation | Automated TV show management and downloading. | `8989` |
| **bazarr** | Media Automation | Automatic subtitle downloading for media. | `6767` |
| **watchtower** | Maintenance | Automatically checks for and updates running Docker images. | N/A |
| **homeassistant**| Smart Home | Central hub for home automation (using `network_mode: host`). | N/A (Uses host ports) |
| **cups** | Network Service | CUPS print server with AirPrint support (`network_mode: host`). | N/A (Uses host ports) |
