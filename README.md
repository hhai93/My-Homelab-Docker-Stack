# 🏠 My Homelab Docker Stack (Media & Automation Focus)

This repository contains the Docker Compose configuration for my personal Homelab setup. It includes a variety of self-hosted services for media management, photo backup, file syncing, smart home, workflow automation and useful utilities.

This setup uses **layered Docker Compose files** and a **centralized `.env` file** for easy management and deployment of specific service groups.

---

## 💡 Introduction

The configuration is split into four files:
1.  **`docker-compose.base.yml`**: Defines the shared network, common environment variables`.
2.  **`docker-compose.immich.yml`**: Contains the full Immich stack.
3.  **`docker-compose.media.yml`**: Contains media servers (`Jellyfin`, `Navidrome`) and the automated 'Arr' stack.
4.  **`docker-compose.utility.yml`**: Contains dashboard (`Homepage`), file management, syncing, smart home, workflow automation and useful utilities .

### Key Configurations
* **Centralized Environment Variables**: All configurable options (ports, PUID, PGID, volume paths, passwords) are managed in the **`.env`** file.
* **External Network**: All containers utilize an external network named `${HOMELAB_NETWORK}` for easy integration with a reverse proxy.

---

## ⚙️ Prerequisites

Before running this stack, you need to have:
1.  **Docker** installed on your host machine.
2.  **Docker Compose** (or the modern `docker compose` CLI).
3.  **Create the Docker Network**:

    ```bash
    docker network create docker_homelab
    ```

4.  **Configure .env**: **Crucially**, update the necessary variables in the `.env` file, especially `PUID`, `PGID`, `DOCKER_CONFIG_BASE`, `DATA_STORAGE_BASE`, and the **Immich database passwords**.

## 🗃️ Directory Structure and Volumes

Volume paths are managed via the `.env` file:
* **`${DOCKER_CONFIG_BASE}`** (e.g., `/mnt/docker/`) for Configuration/Databases.
* **`${DATA_STORAGE_BASE}`** (e.g., `/mnt/data/`) for Media/Downloads/Shared Files.

---

## 🚀 Usage

You must always include **`docker-compose.base.yml`** to ensure the common network and environment variables are loaded.

1.  **Deploy the FULL Stack**:

    ```bash
    docker compose -f docker-compose.base.yml -f docker-compose.immich.yml -f docker-compose.media.yml -f docker-compose.utility.yml up -d
    ```

2.  **Deploy only the Media stack and Utilities**:

    ```bash
    docker compose -f docker-compose.base.yml -f docker-compose.media.yml -f docker-compose.utility.yml up -d
    ```

3.  **Stop/Remove only the Immich stack**:

    ```bash
    docker compose -f docker-compose.base.yml -f docker-compose.immich.yml down
    ```

4.  **Check the status**:

    ```bash
    docker compose -f docker-compose.base.yml -f docker-compose.media.yml ps
    ```
---

## 🛠️ Services Overview

| Service | Category | Functionality | Host Port (Default) |
| :--- | :--- | :--- | :--- |
| **homepage** | Dashboard | Centralized hub for accessing services. | `${HOMEPAGE_HOST_PORT}` (8080) |
| **immich** | Photo Backup | Self-hosted Google Photos alternative. | `${IMMICH_HOST_PORT}` (2283) |
| **filebrowser** | File Management | Web interface for browsing and managing files. | `${FILEBROWSER_HOST_PORT}` (8088) |
| **syncthing** | Syncing | Decentralized file synchronization. | `${SYNCTHING_HOST_PORT}` (8384) |
| **jellyfin** | Media Server | Streaming server for movies and TV shows. | `${JELLYFIN_HOST_PORT}` (8096) |
| **navidrome** | Music Streaming | Web-based music server and streamer. | `${NAVIDROME_HOST_PORT}` (4533) |
| **qbittorrent** | Download Client | Torrent client with a powerful web UI. | `${QBITTORRENT_HOST_PORT_WEBUI}` (8083) |
| **prowlarr** | Media Automation | Indexer manager for Radarr/Sonarr. | `${PROWLARR_HOST_PORT}` (9696) |
| **radarr** | Media Automation | Automated movie management. | `${RADARR_HOST_PORT}` (7878) |
| **sonarr** | Media Automation | Automated TV show management. | `${SONARR_HOST_PORT}` (8989) |
| **n8n** | Automation | Workflow automation and integration tool. | `${N8N_HOST_PORT}` (5678) |
| **homeassistant**| Smart Home | Central hub for home automation. | `8123` |
| **cups**| SNetwork Service | CUPS print server with AirPrint support. | `631` |
| **watchtower** | Maintenance | Automatically updates running Docker images. | N/A |
