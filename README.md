# MAID (Miniature Async Internet Device)

This project sets up a media server environment using Docker Compose, and Some networking tools. Main focus of this tool is to extent the SpyderNet project Created by Advait Pandharpurkar in FE.   
It includes Jellyfin for media streaming, Pi-hole for network-wide ad blocking, Heimdall for a dashboard interface, and Prometheus and Grafana for network monitoring.

---

## Members who Contributed to this Project
1. [Advait Pandharpurkar](https://www.linkedin.com/in/lordofwizard/)
2. [Priyal Kalal](https://www.linkedin.com/in/priyal-kalal-307900250/)
3. [Kedar Nanaware](https://www.linkedin.com/in/kedar-nanaware-7264942b6/)
4. Nitin Pawar
5. Nikhil Pawar

## Table of Contents
1. [Project Structure](#project-structure)
2. [Services](#services)
    - [Jellyfin](#jellyfin)
    - [Pi-hole](#pi-hole)
    - [Heimdall](#heimdall)
    - [Prometheus](#prometheus)
    - [Grafana](#grafana)
3. [Setup](#setup)
4. [Running the Project](#running-the-project)
5. [Configuration](#configuration)
6. [Troubleshooting](#troubleshooting)

## Project Structure

The directory structure for the project is as follows:
```
docker-media-server/
├── docker-compose.yml
├── jellyfin/
│   ├── config/
│   ├── cache/
├── media/
│   ├── tv_shows/
│   ├── movies/
├── pihole/
│   ├── etc-pihole/
│   ├── etc-dnsmasq.d/
├── heimdall/
│   ├── config/
├── prometheus/
├── grafana/
```

## Services

### Jellyfin
Jellyfin is a free software media system that puts you in control of managing and streaming your media.

- **Image**: `jellyfin/jellyfin`
- **Container Name**: `jellyfin`
- **Volumes**:
  - `./jellyfin/config:/config` - Configuration files
  - `./jellyfin/cache:/cache` - Cache files
  - `./media/tv_shows:/media/tv_shows` - TV shows library
  - `./media/movies:/media/movies` - Movies library
- **Ports**:
  - `8096:8096` - Web interface and API

### Pi-hole
Pi-hole is a network-wide ad blocker.

- **Image**: `pihole/pihole`
- **Container Name**: `pihole`
- **Environment Variables**:
  - `TZ: 'America/Chicago'` - Timezone setting
  - `WEBPASSWORD: 'yourpassword'` - Password for the web interface
- **Volumes**:
  - `./pihole/etc-pihole:/etc/pihole` - Configuration files
  - `./pihole/etc-dnsmasq.d:/etc/dnsmasq.d` - DNS configuration files
- **Ports**:
  - `53:53/tcp` - DNS TCP
  - `53:53/udp` - DNS UDP
  - `67:67/udp` - DHCP UDP
  - `80:80/tcp` - Web interface
  - `443:443/tcp` - Web interface (SSL)

### Heimdall
Heimdall is an application dashboard.

- **Image**: `linuxserver/heimdall`
- **Container Name**: `heimdall`
- **Environment Variables**:
  - `PUID: 1000` - User ID for permissions
  - `PGID: 1000` - Group ID for permissions
  - `TZ: America/Chicago` - Timezone setting
- **Volumes**:
  - `./heimdall/config:/config` - Configuration files
- **Ports**:
  - `8000:80` - Web interface (HTTP)
  - `443:443` - Web interface (HTTPS)

### Prometheus
Prometheus is a monitoring and alerting toolkit.

- **Image**: `prom/prometheus`
- **Container Name**: `prometheus`
- **Volumes**:
  - `./prometheus:/etc/prometheus` - Configuration files
- **Ports**:
  - `9090:9090` - Web interface

### Grafana
Grafana is an open-source platform for monitoring and observability.

- **Image**: `grafana/grafana`
- **Container Name**: `grafana`
- **Environment Variables**:
  - `GF_SECURITY_ADMIN_PASSWORD=yourpassword` - Admin password
- **Volumes**:
  - `./grafana:/var/lib/grafana` - Data storage
- **Ports**:
  - `3000:3000` - Web interface

## Setup

1. **Install Docker and Docker Compose**:
   - Follow the instructions for installing Docker [here](https://docs.docker.com/get-docker/).
   - Install Docker Compose by following the instructions [here](https://docs.docker.com/compose/install/).

2. **Clone the Repository**:
   ```sh
   git clone https://github.com/lordofwizard/maid
   cd maid
   ```

3. **Create Directory Structure**:
   Make sure the directory structure as shown above is created. This can be done manually or using the following commands:
   ```sh
   mkdir -p jellyfin/config jellyfin/cache media/tv_shows media/movies pihole/etc-pihole pihole/etc-dnsmasq.d heimdall/config prometheus grafana
   ```

4. **Set Permissions**:
   Ensure that the directories have the correct permissions for Docker to read/write.
   ```sh
   sudo chown -R 1000:1000 jellyfin heimdall prometheus grafana
   ```

## Running the Project

To start all the services, run the following command in the directory containing the `docker-compose.yml` file:
```sh
docker-compose up -d
```
This will start all services in detached mode.

## Configuration

### Jellyfin
- **Access**: Open a web browser and go to `http://<your-ip>:8096`
- **Initial Setup**: Follow the on-screen instructions to complete the setup and add your media libraries.

### Pi-hole
- **Access**: Open a web browser and go to `http://<your-ip>/admin`
- **Login**: Use the password set in the `WEBPASSWORD` environment variable.

### Heimdall
- **Access**: Open a web browser and go to `http://<your-ip>:8000`
- **Setup**: Follow the on-screen instructions to add your applications and bookmarks.

### Prometheus
- **Access**: Open a web browser and go to `http://<your-ip>:9090`
- **Configuration**: Edit the Prometheus configuration file located in `./prometheus/prometheus.yml`.

### Grafana
- **Access**: Open a web browser and go to `http://<your-ip>:3000`
- **Login**: Use the default username `admin` and the password set in the `GF_SECURITY_ADMIN_PASSWORD` environment variable.
- **Setup**: Add Prometheus as a data source and create your dashboards.

## Troubleshooting

- **Checking Logs**: If you encounter issues, you can check the logs of any service using:
  ```sh
  docker-compose logs <service-name>
  ```
  Replace `<service-name>` with `jellyfin`, `pihole`, `heimdall`, `prometheus`, or `grafana` as needed.

- **Restarting Services**: To restart a specific service, use:
  ```sh
  docker-compose restart <service-name>
  ```

- **Stopping Services**: To stop all services, use:
  ```sh
  docker-compose down
  ```

## Conclusion

This setup provides a robust and scalable media server environment with additional functionalities for network monitoring and application management. Customize and expand as needed to suit your requirements. Enjoy your new media server!
