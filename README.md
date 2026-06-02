# Auto-Media-Server


🎬 Home Media Server
A fully automated self-hosted media server stack running on Proxmox, built for streaming movies and TV shows at home and remotely.
📋 Overview
This project sets up a complete media automation system that allows you and your family to request, download, and stream movies and TV shows — all managed from a single interface.


/media/
├── films/          # Movies
├── serier/         # TV Shows
├── musik/          # Music
├── downloads/      # Download staging
│   ├── complete/   # Completed downloads
│   └── incomplete/ # In progress downloads
└── undertekster/   # Subtitles

🛠️ Stack
Service            Purpose                  Port
qbittorrent        Downloads                8080
Jellyfin           Media streaming server   8096
Radarr             Movie automation         7878
Sonarr             TV show automation       8989  
Prowlarr           Indexer management       9696
Bazarr             Subtitle automation      6767
Jellyseerr         Media request            5055
