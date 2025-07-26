# dnsproxy
# dnsproxy

A lightweight DNS proxy and HTTP reverse-proxy stack using dnsmasq and NGINX for caching, forwarding, and health-checked service delivery.

## Table of Contents

- [Features](#features)
- [Architecture](#architecture)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Health Checks](#health-checks)
- [Customization](#customization)
- [Docker Compose](#docker-compose)
- [Contributing](#contributing)
- [License](#license)

## Features

- **DNS Caching & Forwarding:** Fast DNS resolution using dnsmasq  
- **HTTP Reverse Proxy:** Flexible routing and TLS passthrough via NGINX  
- **Health Checks:** Built-in endpoints and container healthchecks  
- **Docker Support:** Deploy with Docker and Docker Compose  

## Architecture

This project runs two services:

1. **dnsmasq**  
   - Listens on UDP port 53 for DNS queries  
   - Caches upstream responses  
   - Forwarding rules defined in `dnsmasq.conf`

2. **nginx-proxy**  
   - Acts as a reverse proxy for HTTP/TLS  
   - Configurable via `nginx.conf`  
   - Exposes a `/health` endpoint for service monitoring  

## Prerequisites

- Docker Engine (v20.10+ recommended)  
- Docker Compose (v1.29+ recommended)  
- A Linux host or VM for UDP port binding  

## Installation

1. **Clone the repository**  
   ```bash
   git clone https://github.com/<your‑org>/dnsproxy.git
   cd dnsproxy
   ```

2. **Customize configuration**  
   - Edit `dnsmasq.conf` and `dnsmasq.d/` as needed  
   - Edit `nginx.conf` for proxy settings, timeouts, and headers  

3. **Launch services**  
   ```bash
   docker-compose up -d
   ```

## Configuration

- **dnsmasq.conf** – Primary dnsmasq settings (cache size, upstream servers, filtering)  
- **dnsmasq.d/** – Additional fragment files  
- **nginx.conf** – Worker, proxy, and health-check configuration  

## Usage

- Point your system DNS server to the host running dnsmasq (e.g., `sudo resolvectl dns lo 127.0.0.1`)  
- Access web services through the NGINX proxy on port 80 or 443  

## Health Checks

- **DNS Container Health:** UDP probe on port 53 (configured in Compose healthcheck)  
- **HTTP Health Endpoint:**  
  ```
  GET http://<host>/health
  ```  

## Customization

- Mount additional `dnsmasq.d/` fragments by editing the Compose file or volume mounts  
- Override environment variables or build a custom Docker image if needed  

## Docker Compose

The provided `docker-compose.yml` defines both services with:

- Network mode, restart policies, and ulimits  
- Healthcheck for dnsmasq  
- Host volume mounts for configuration files  

Start, stop, and update with:

```bash
docker-compose up -d
docker-compose pull
docker-compose up -d --build
```

## Contributing

1. Fork the repository  
2. Create a feature branch (`git checkout -b feature/awesome`)  
3. Commit your changes (`git commit -m 'Add awesome feature'`)  
4. Push and open a Pull Request  

## License

Distributed under the [MIT License](LICENSE).