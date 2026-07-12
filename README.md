# Pi-hole with Docker

[Español](./README.es.md)

A [Docker Compose](https://docs.docker.com/compose/) setup to run [Pi-hole](https://pi-hole.net/) — a network-wide ad blocker and DNS sinkhole that blocks ads, trackers, and malicious domains at the DNS level for **every device on your network**.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/) (usually included with Docker Desktop)

## Getting Started

### 1. Configure environment variables

Copy the example environment file:

```bash
cp .env.example .env
```

Edit `.env` and set the required values:

| Variable | Description | Example |
|---|---|---|
| `TZ` | Timezone | `Europe/Madrid` |
| `DATA_ROOT` | Host path for persistent data | `C:\docker` (Windows) or `/srv/docker` (Linux) |
| `PIHOLE_PASSWORD` | Web admin password | `your_secure_password` |

> **Security:** The `.env` file is listed in `.gitignore` so it won't be committed.

### 2. Start Pi-hole

```bash
docker compose up -d
```

### 3. Access the web admin interface

Open your browser and go to:

```
http://<your-host-ip>:8081/admin
```

Log in with the password you set in `.env`.

## Usage

| Command | Description |
|---|---|
| `docker compose up -d` | Start Pi-hole in the background |
| `docker compose down` | Stop and remove the container |
| `docker compose logs -f` | Tail the container logs |
| `docker compose restart` | Restart the container |

## Pointing devices to Pi-hole

For Pi-hole to block ads on your network, devices must use it as their DNS server.

- **Router-level (recommended):** Set your router's DNS server to the IP of the machine running Pi-hole. This covers every device automatically.
- **Per-device:** Manually set the DNS server in your device's network settings to the IP of the Pi-hole host.

## Project Structure

```
├── .env               # Local environment variables (gitignored)
├── .env.example       # Template for .env
├── .gitignore
├── docker-compose.yml # Service definition
├── README.md          # Documentation (English)
├── README.es.md       # Documentation (Spanish)
└── etc-pihole/        # Persistent data (created at runtime)
```

## Ports

| Port | Protocol | Purpose |
|---|---|---|
| `53` | TCP/UDP | DNS queries |
| `8081` | TCP | Web admin dashboard |

> Port `53` requires elevated privileges on most systems. Docker handles this automatically.

## Data Persistence

Pi-hole data is stored on the host at `${DATA_ROOT}/pihole/` with two subdirectories:
- `etc-pihole/` → `/etc/pihole` in the container (Pi-hole config and databases)
- `etc-dnsmasq.d/` → `/etc/dnsmasq.d` in the container (dnsmasq configuration)

## Updating

```bash
docker compose pull && docker compose up -d
```

## Resources

- [Pi-hole Official Site](https://pi-hole.net/)
- [Pi-hole Documentation](https://docs.pi-hole.net/)
- [Pi-hole Docker Image](https://hub.docker.com/r/pihole/pihole)
- [Official Docker Setup Guide](https://github.com/pi-hole/docker-pi-hole)
