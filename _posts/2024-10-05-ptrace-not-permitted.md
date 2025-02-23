---
layout: post
title: "Solving `ptrace: Operation not permitted.` for GDB"
date: 2024-10-05
description: "Solving `ptrace: Operation not permitted.` for GDB"
tags: linux ubuntu commands
comments: true
---

# The Permission Error

If  you see this error when attaching the GDB to a process:
> ptrace: Operation not permitted.

# The Solution

1. (Temporarily, sudo required) run `echo "0"|sudo tee /proc/sys/kernel/yama/ptrace_scope`
2. (Permanently, sudo required) editing the file `/etc/sysctl.d/10-ptrace.conf` and change the line: `kernel.yama.ptrace_scope = 1` to `kernel.yama.ptrace_scope = 0`

# References
- [https://stackoverflow.com/questions/19215177/how-to-solve-ptrace-operation-not-permitted-when-trying-to-attach-gdb-to-a-pro](https://stackoverflow.com/questions/19215177/how-to-solve-ptrace-operation-not-permitted-when-trying-to-attach-gdb-to-a-pro)
- [https://unix.stackexchange.com/questions/329504/proc-sys-kernel-yama-ptrace-scope-keeps-resetting-to-1](https://unix.stackexchange.com/questions/329504/proc-sys-kernel-yama-ptrace-scope-keeps-resetting-to-1)

