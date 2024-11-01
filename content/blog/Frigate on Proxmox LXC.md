---
title: Frigate on GMK G5 (Alder Lake N97)
tags:
  - frigate
  - home-assistant
draft: true
---
### 1. **Introduction: Why Frigate on Proxmox with GMK G5?**

- Explain your motivation for choosing Frigate and why you wanted to run it in a Proxmox LXC environment.
- A quick overview of the GMK G5 and why the Alder Lake N97 with iGPU is a good choice for a Frigate instance.

### 2. **Preparing the Proxmox LXC Environment**

- Start with the initial steps of setting up Proxmox, choosing the correct LXC template, and any configuration needed to support the GPU passthrough.
- Tips for anyone new to Proxmox LXC and what to watch out for in this setup.

### 3. **GPU Passthrough for the Alder Lake iGPU**

- A walk-through on enabling and configuring GPU passthrough specifically for the Alder Lake N97’s iGPU.
- Any kernel parameters or BIOS settings you had to tweak to get the iGPU working in the LXC container.

### 4. **Installing Frigate and Optimizing It for the iGPU**

- Step-by-step instructions for installing Frigate, configuring it to use the hardware acceleration, and any dependencies or drivers you needed.
- Share any hiccups or troubleshooting tips, especially for other Alder Lake-based setups.

### 5. **Performance Observations and Testing**

- How well is the iGPU handling video processing?
- Share your experiences with stream stability, detection speed, and any tweaks you made to get the best performance.

### 6. **Wrap-Up: Final Thoughts and Recommendations**

- Reflect on the setup, what worked well, what could be improved, and tips for others interested in trying a similar setup.