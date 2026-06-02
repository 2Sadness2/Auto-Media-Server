# 🎬 Auto-Media-Server

> Build your own automated home media server — like Netflix, but self-hosted.

![GitHub repo size](https://img.shields.io/github/repo-size/2Sadness2/Auto-Media-Server)
![GitHub last commit](https://img.shields.io/github/last-commit/2Sadness2/Auto-Media-Server)
![License](https://img.shields.io/badge/license-MIT-blue)

---

## 📋 Overview

A fully automated self-hosted media server stack running on **Proxmox**, built for streaming movies and TV shows at home and remotely.

This project allows you and your family to **request, download, and stream** movies and TV shows — all managed from a single interface.

---

## 🏗️ Infrastructure

| Component | Details |
|-----------|---------|
| **Hypervisor** | Proxmox VE |
| **Container** | Ubuntu LXC (CT100) |
| **Container Engine** | Docker |
| **Management UI** | Portainer |

---

## 🛠️ Services

| Service | Purpose | Port |
|---------|---------|------|
| 🎬 **Jellyfin** | Media streaming server | `8096` |
| 🎬 **Plex** | Media streaming server | `32400` |
| 🎥 **Radarr** | Movie automation | `7878` |
| 📺 **Sonarr** | TV show automation | `8989` |
| 🔍 **Prowlarr** | Indexer management | `9696` |
| 💬 **Bazarr** | Subtitle automation | `6767` |
| 🙋 **Jellyseerr** | Media request portal | `5055` |
| ⬇️ **qBittorrent** | Torrent download client | `8080` |

---

## ⚙️ How It Works

```
👨‍👩‍👧 Family requests movie/show via Jellyseerr
                    ↓
        🎥 Radarr / 📺 Sonarr finds it
                    ↓
          ⬇️ qBittorrent downloads it
                    ↓
      📁 Organized automatically in /media
                    ↓
      🎬 Jellyfin picks it up automatically
                    ↓
       📱 Family watches on any device 🎉
```

---

## 📁 Directory Structure

```
/media/
├── films/              # 🎥 Movies
├── serier/             # 📺 TV Shows
├── musik/              # 🎵 Music
├── downloads/          # ⬇️ Download staging
│   ├── complete/       # ✅ Completed downloads
│   └── incomplete/     # 🔄 In progress downloads
└── undertekster/       # 💬 Subtitles
```

---

## 🐳 Docker Compose

```yaml
services:
  qbittorrent:
    image: lscr.io/linuxserver/qbittorrent:4.6.7
    container_name: qbittorrent
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Copenhagen
      - WEBUI_PORT=8080
    volumes:
      - /etc/qbittorrent:/config
      - /media:/media
    ports:
      - 8080:8080
      - 6881:6881
      - 6881:6881/udp

  radarr:
    image: lscr.io/linuxserver/radarr
    container_name: radarr
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Copenhagen
    volumes:
      - /etc/radarr:/config
      - /media:/media
    ports:
      - 7878:7878

  sonarr:
    image: lscr.io/linuxserver/sonarr
    container_name: sonarr
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Copenhagen
    volumes:
      - /etc/sonarr:/config
      - /media:/media
    ports:
      - 8989:8989

  prowlarr:
    image: lscr.io/linuxserver/prowlarr
    container_name: prowlarr
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Copenhagen
    volumes:
      - /etc/prowlarr:/config
    ports:
      - 9696:9696

  bazarr:
    image: lscr.io/linuxserver/bazarr
    container_name: bazarr
    restart: unless-stopped
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Copenhagen
    volumes:
      - /etc/bazarr:/config
      - /media:/media
    ports:
      - 6767:6767

  jellyfin:
    image: jellyfin/jellyfin
    container_name: jellyfin
    restart: unless-stopped
    environment:
      - TZ=Europe/Copenhagen
    volumes:
      - /etc/jellyfin:/config
      - /var/cache/jellyfin:/cache
      - /media:/media
    ports:
      - 8096:8096

  jellyseerr:
    image: ghcr.io/seerr-team/seerr:latest
    container_name: jellyseerr
    restart: unless-stopped
    environment:
      - LOG_LEVEL=info
      - TZ=Europe/Copenhagen
    volumes:
      - /etc/jellyseerr:/app/config
    ports:
      - 5055:5055
```

---

## 🚀 Getting Started

### Prerequisites
- Proxmox VE installed
- Ubuntu 24.04 LXC container
- Docker + Docker Compose installed

### Installation

**1. Clone this repository**
```bash
git clone https://github.com/2Sadness2/Auto-Media-Server
cd Auto-Media-Server
```

**2. Create media directories**
```bash
mkdir -p /media/{films,serier,musik,downloads/complete,downloads/incomplete,undertekster}
```

**3. Deploy the stack**
```bash
docker compose up -d
```

**4. Access your services and configure each one**

---

## 🌐 Remote Access

| Method | Purpose |
|--------|---------|
| ☁️ **Cloudflare Tunnel** | Secure remote access via custom domain |

---

## 📱 Service URLs

| Service | URL |
|---------|-----|
| Jellyfin | `http://YOUR-IP:8096` |
| Jellyseerr | `http://YOUR-IP:5055` |
| qBittorrent | `http://YOUR-IP:8080` |
| Radarr | `http://YOUR-IP:7878` |
| Sonarr | `http://YOUR-IP:8989` |
| Prowlarr | `http://YOUR-IP:9696` |
| Bazarr | `http://YOUR-IP:6767` |

---

## ⚠️ Notes

> **qBittorrent:** Use version `4.6.7` — newer versions (5.x) have WebUI port issues with Docker

---

## 📝 License

MIT License — feel free to use and modify.
