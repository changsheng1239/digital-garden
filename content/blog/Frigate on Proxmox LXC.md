---
title: Frigate on GMK G5 (Alder Lake N97)
tags:
  - frigate
  - home-assistant
draft: false
---
> [!tldr]- TL;DR
> I initially couldn't get my N97 iGPU working with Frigate in an LXC container on Proxmox. Updating the Proxmox kernel from 6.8.x to 6.12.x (latest at the time) solved the issue.
### Why the GMK G5?

I typically run my self-hosted services (like Docker and VMs) on my Synology NAS, but it doesn’t quite measure up to the Proxmox web UI experience. So, I decided to set up a dedicated instance for my home automation tools like Home Assistant and Frigate.

After a lot of research, the GMK G5 caught my eye for a few reasons:

1. **Form Factor:** It’s compact, measuring just 2.83” x 2.83” x 1.75”.
2. **Built-in GPU:** Ideal for offloading tasks in Frigate.
3. **Low Power Consumption:** Perfect for a 24/7 home server setup.

### Setting Up Frigate in an LXC Container

Once I had Proxmox installed on the G5, I opted to use an LXC container for Frigate to keep resource usage low. Setting up the LXC was a breeze thanks to [tteck’s Proxmox scripts](https://github.com/tteck/Proxmox) (now relocated to [ProxmoxVE](https://github.com/community-scripts/ProxmoxVE)). With just a few commands in the Proxmox shell, the LXC setup was good to go.

After booting up the container, Frigate detected my Intel iGPU and loaded the `openvino` model. However, by default, it was set to use the CPU:

```yaml
detectors:
  ov:
    type: openvino
    device: CPU
    model:
      path: /openvino-model/FP16/ssdlite_mobilenet_v2.xml
```

### Enabling the iGPU in the N97

While everything was technically working, I wanted to offload inferencing to the iGPU rather than the CPU. So, I updated the configuration to `device: GPU` and restarted. But instead of success, I was greeted with errors, and Frigate kept crashing with:

```log
[GPU] Context was not initialized for 0 device
```

This error hinted at a driver-related issue. After a lot of trial and error, including multiple solutions I found online, nothing seemed to work. Here’s what I tried:

1. Testing different `preset` options.
2. Setting the environment variable `LIBVA_DRIVER_NAME: i965`.
3. Manually installing the latest Intel runtime in the container.
4. Booting up a Debian 12 VM, installing Docker and Frigate, and passing through the iGPU — but I still ran into the same error.

Finally, thanks to a comment on this [GitHub issue](https://github.com/blakeblackshear/frigate/issues/12266#issuecomment-2400003395), I decided to upgrade my Proxmox kernel from 6.8.4 to 6.12.x (the latest version at the time).

After reinstalling the Frigate container, setting the device to GPU, and restarting, it booted up without errors! Inference was now working on the GPU, with detection speeds averaging around 9ms—perfect!