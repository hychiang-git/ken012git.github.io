---
layout: post
title: "Solving Nsight Compute Permission Error"
date: 2024-08-20
description: "Solving Nsight Compute Permission Error"
tags: linux, ubuntu, nvidia, commands
comments: true
---

# The Permission Error
If you are running into this error message when using [Nsight-Compute](https://developer.nvidia.com/nsight-compute)

> The user does not have permission to access NVIDIA GPU Performance Counters on the target device 0. For instructions on enabling permissions and to get more information see https://developer.nvidia.com/ERR_NVGPUCTRPERM

# The Solution

1. (sudo required) Create `.conf` file (e.g. `profile.conf`) in folder `/etc/modprobe.d`
2. (sudo required) Open file `/etc/modprobe.d/profile.conf` in any editor
3. (sudo required) Add below line in profile.conf `options nvidia NVreg_RestrictProfilingToAdminUsers=0`, and close file `/etc/modprobe.d/profile.conf`
4. (Optional) On some systems (or when using a package manager to install), it may be necessary to rebuild the initrd after writing a configuration file to `/etc/modprobe.d`. For RedHat-based distributions, rebuild the initrd with `dracut -–regenerate-all -f`
5. (sudo required) Restart your machine
6. (Optional) Check that the value is correctly set to 0: `cat /proc/driver/nvidia/params | grep RmProfilingAdminOnly`, You should see: `RmProfilingAdminOnly: 0`


# Reference
- [https://developer.nvidia.com/nvidia-development-tools-solutions-err_nvgpuctrperm-permission-issue-performance-counters#AllUsersTag](https://developer.nvidia.com/nvidia-development-tools-solutions-err_nvgpuctrperm-permission-issue-performance-counters#AllUsersTag)
- [https://forums.developer.nvidia.com/t/nvprof-warning-the-user-does-not-have-permission-to-profile-on-the-target-device/72374/4](https://forums.developer.nvidia.com/t/nvprof-warning-the-user-does-not-have-permission-to-profile-on-the-target-device/72374/4)

