---
title: Frigate on GMK G5 (Alder Lake N97)
tags:
  - frigate
  - home-assistant
draft: false
---
### TL;DR

I initially couldn't get my N97 iGPU working with Frigate in an LXC container on Proxmox. Updating the Proxmox kernel from 6.4.x to 6.12.x solved the issue.

### Why the GMK G5?

I typically run my self-hosted services (like Docker and VMs) on my Synology NAS, but it doesn’t quite measure up to the Proxmox web UI experience. So, I decided to set up a dedicated instance for my home automation tools like Home Assistant and Frigate.

After a lot of research, the GMK G5 caught my eye for a few reasons:

1. **Form Factor:** It’s compact, measuring just 2.83” x 2.83” x 1.75”.
2. **Built-in GPU:** Ideal for offloading tasks in Frigate.
3. **Low Power Consumption:** Perfect for a 24/7 home server setup.

### Setting Up Frigate in an LXC Container

Once I had Proxmox installed on the G5, I opted to use an LXC container for Frigate to keep resource usage low. Setting up the LXC was a breeze thanks to [tteck’s Proxmox scripts](https://github.com/tteck/Proxmox) (now relocated to [ProxmoxVE](https://github.com/community-scripts/ProxmoxVE)). With just a few commands in the Proxmox shell, the LXC setup was good to go.

After booting up the container, Frigate detected my Intel iGPU and loaded the `openvino` model. However, by default, it was set to use the CPU:

yaml