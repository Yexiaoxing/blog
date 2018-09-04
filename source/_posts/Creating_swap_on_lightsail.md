---
title: Creating SWAP File on AWS Lightsail Linux Machines
date: 2018-09-04 19:13:11
tags:
  - Tutorials
  - AWS
categories:
  - Tutorials
---

When I was building Vue files on a fresh AWS LightSail Machine, the SSH connection would freeze, and the building would result in a killed status. Why? It comes to me that, the memory is too small (1GB) and it gets killed for OOM. But why not swap?

1. Create a new swap file. By running the following commands, it would create a 8G swap file.

  ```bash
  sudo fallocate -l 8G /swapfile
  sudo chmod 600 /swapfile
  sudo mkswap /swapfile
  sudo swapon /swapfile
  ```

2. Add the line to fstab.

  ```bash
  sudo cp /etc/fstab /etc/fstab.bak
  echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
  ```

3. Ask the system to use swap less frequently.

  ```bash
  sudo sysctl vm.swappiness=10
  echo 'vm.swappiness=10' | sudo tee -a /etc/sysctl.conf
  sudo sysctl vm.vfs_cache_pressure=50
  echo 'vm.vfs_cache_pressure=50' | sudo tee -a /etc/sysctl.conf
  ```
