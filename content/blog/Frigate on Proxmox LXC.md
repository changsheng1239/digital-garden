---
title: Frigate on GMK G5 (Alder Lake N97)
tags:
  - frigate
  - home-assistant
draft: false
---
### TLDR

Frigate LXC unable to detect my N97 iGPU. Updated Proxmox kernel from 6.4.x to 6.12.x and it worked.

### Why GMK G5 ?

I usually runs my self-hosted services (docker/vm) in my Synology NAS directly, but the experience is not the greatest compared to Proxmox webui.

So I decided to have a separated instance for my home-controlled instance, e.g.: Home Assistant & Frigate.

After tons of shopping around, GMK G5 caught my eye because:
1. form factor (2.83inch * 2.83inch * 1.75inch)
2. Built in GPU (suitable for Frigate)
3. Low power consumption

## Frigate LXC

After installing Proxmox onto the G5, I decided to go with LXC for the new Frigate instance due to lesser resource overhead.

Thanks to [tteck proxmox scripts](https://github.com/tteck/Proxmox) (moved to new repo [ProxmoxVE](https://github.com/community-scripts/ProxmoxVE)), the LXC setup is just copy and paste into Proxmox shell.

Once the new container booted up, it managed to detect my intel iGPU and loaded the `openvino` model but it is using CPU by default:
```yaml
detectors:
  ov:
    type: openvino
    device: CPU
    model:
      path: /openvino-model/FP16/ssdlite_mobilenet_v2.xml
```


### Utilize the iGPU in N97

Everything is working at this stage but I wanted to utilize the iGPU for inferencing instead of CPU.

So I updated the config to `device: GPU` and restart but was greeted by errors and Frigate was constantly crashing with errors:
```
[GPU] Context was not initialized for 0 device
```

This error suggest it might be related to driver issue and I  tried a lot of suggestion online which all failed for my case.

This is what I tried:
1. Using other `preset`.
2. Set the env `LIBVA_DRIVER_NAME: i965`.
3. Install the latest Intel runtime manually in the container.
4. Boot a latest Debian 12 VM and install docker + frigate & passthrough iGPU. (still have the same error)

In the end, thanks to one of the comment in this [issue](https://github.com/blakeblackshear/frigate/issues/12266#issuecomment-2400003395), I upgraded my Proxmox kernel from 6.8.4 to 6.8.12 (latest at the time).

Then I reinstall the Frigate container, switch to GPU, and restart the container. Finally it booted successfully without errors.

The detection worked and inference speed is around 9ms, great!
