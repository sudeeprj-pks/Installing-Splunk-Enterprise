 Install and Configure Splunk on Ubuntu

This repository contains a step-by-step guide (commands + explanation) to install, enable, and access Splunk Enterprise 9.3.0 on an Ubuntu machine/VM. Use this README to reproduce the setup and to troubleshoot common issues (firewall, VM networking, service status).



 Overview

These instructions cover:

* Downloading the Splunk .deb package
* Installing Splunk on Ubuntu
* Enabling Splunk to start at boot
* Starting and verifying Splunk
* Opening the Splunk Web UI
* Troubleshooting common issues (missing utilities, firewall, VM NAT/Bridged)

 Tested with Splunk Enterprise 9.3.0 (filename used in examples). Adjust package filename/URL when using a different version.

---

Prerequisites

* Ubuntu (desktop or server)
* `sudo` privileges
* Internet access to download Splunk
* If running in a VM, know whether network is NAT or Bridged

---

## Quick commands 

```bash
# 1. Update package index and install utilities
sudo apt update
sudo apt install -y curl net-tools

# 2. Create a working folder (optional)
mkdir -p ~/downloads/splunk && cd ~/downloads/splunk

# 3. Download Splunk (adjust the URL/filename if needed)
wget -O splunk-9.3.0-51ccf43db5bd-linux-2.6-amd64.deb "https://download.splunk.com/products/splunk/releases/9.3.0/linux/splunk-9.3.0-51ccf43db5bd-linux-2.6-amd64.deb"

# 4. Install the package
sudo dpkg -i splunk-9.3.0-51ccf43db5bd-linux-2.6-amd64.deb
# If dpkg reports missing dependencies, run:
sudo apt --fix-broken install -y
# and repeat the dpkg -i if needed.

# 5. Enable Splunk to start at boot
sudo /opt/splunk/bin/splunk enable boot-start --accept-license

# 6. Start Splunk (first run will prompt to create admin username/password)
sudo /opt/splunk/bin/splunk start

# 7. Check status
sudo /opt/splunk/bin/splunk status

# 8. Verify Splunk web is responding locally
curl -I http://localhost:8000
# Expect HTTP/1.1 303 See Other with Location: /en-US/

# 9. Allow port 8000 through UFW (if using UFW firewall)
sudo ufw allow OpenSSH
sudo ufw allow 8000/tcp
sudo ufw reload
sudo ufw status

# 10. Get server IP (use this IP in browser from your host machine or other device)
hostname -I

# 11. If using a VM with NAT and you need host access, use SSH port forwarding from host:
# On the host machine (replace user@VM-IP with your VM credentials):
ssh -L 8000:localhost:8000 user@VM-IP
# Then open http://localhost:8000 in your host browser.
```

