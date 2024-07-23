# devopsfetch

## Overview

`devopsfetch` is a tool for DevOps that collects and displays system information, including active ports, user logins, Nginx configurations, Docker images, and container statuses. It also supports continuous monitoring with log rotation.

## Features

- Display all active ports and services.
- List Docker images and containers.
- Display Nginx domains and their ports.
- List all users and their last login times.
- Display activities within a specified time range.
- Log activities to a file with log rotation and management.

## Installation

### Prerequisites

- Docker
- Nginx

## Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/yourusername/HNG11-5.git
   cd HNG11-5
   ```

# File Structure
```
.
├── devopsfetch.service  # User as a pointer to the insatallion file
├── devopsfetch.sh       # Main script for fetching system information
├── install.sh           # Installation script for setting up dependencies and systemd service
└── README.md            # Project documentation

```
# Log Management
Logs generated by devOpsfetch are stored in ```/tmp/devopsfetch.log``` as well as ```/var/log/devopsfetch.log``` . You can view the log file using commands like ```cat```, ```less```, or ```tail```. For long-term storage and analysis, consider setting up log rotation using logrotate. with a maximum size of 10 MB. Log rotation is handled automatically by renaming the old log file to `devopsfetch.log.old`

## How to start
```
chmod +x install.sh
sudo ./install.sh
```
### The installation script will:

    1. Install Docker and Nginx.
    2. Copy the devopsfetch script to /usr/local/bin/.
    3. Set up a systemd service to monitor and log activities.
## Manual Setup
If you prefer to set up the tool manually:

### Install Dependencies:

```
sudo apt-get update
sudo apt-get install -y nginx curl
```
### Set Up Docker:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```
### Copy devopsfetch Script:
```
sudo cp devopsfetch.sh /usr/local/bin/devopsfetch
sudo chmod +x /usr/local/bin/devopsfetch
```
### Set Up Systemd Service:
```bash
cat << EOF | sudo tee /etc/systemd/system/devopsfetch.service
[Unit]
Description=DevOpsFetch Monitoring Service
After=network.target

[Service]
ExecStart=/usr/local/bin/devopsfetch -t "1 hour ago"
Restart=always
User=root

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable devopsfetch.service
sudo systemctl start devopsfetch.service
```
# Usage

## devopsfetch [OPTION] [ARGUMENT]

### Options
- **-p, --port [PORT]:**

Display all active ports or detailed info about a specific port.
```bash
devopsfetch -p
devopsfetch -p 80
```
- **-d, --docker [NAME]:**

List all Docker images and containers or detailed info about a specific container.
```bash
devopsfetch -d
devopsfetch -d <container_name>
```
- **-n, --nginx [DOMAIN]:**

Display all Nginx domains and their ports or detailed configuration information for a specific domain.
```bash
devopsfetch -n
devopsfetch -n <domain>
```
- **-u, --users [USER]:**

List all users and their last login times or detailed information about a specific user.
```bash
devopsfetch -u
devopsfetch -u <username>
```
- **-t, --time [START] [END]:**

Display activities within a specified time range.
```bash
devopsfetch -t 2024-07-18 2024-07-22
devopsfetch -t 2024-07-21
```
- **-h, --help:**

Display the help message.
```bash
devopsfetch -h
```


# Conclusion
DevOpsFetch is a powerful and flexible tool designed to streamline the monitoring and management of your DevOps environment. By consolidating essential system information, Docker details, Nginx configurations, and user activity logs into a single, easy-to-use script, DevOpsFetch empowers you to maintain and optimize your infrastructure efficiently.

With its comprehensive features and seamless integration with systemd for automated logging, DevOpsFetch ensures that you have all the critical data at your fingertips, enhancing your ability to respond swiftly to changes and issues. Whether you are a system administrator, developer, or DevOps engineer, DevOpsFetch is your go-to solution for maintaining system health and performance.

Feel free to contribute to the project, report any issues, or suggest enhancements. Your feedback is invaluable in making DevOpsFetch even better. Happy monitoring!