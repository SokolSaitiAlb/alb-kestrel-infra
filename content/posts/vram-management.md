---
title: "Hard Lessons in VRAM Allocation: SIGKILL and the OOM Killer"
date: 2026-05-08
draft: false
---

### The Incident
While testing the limits of my RX 6800 16GB on CachyOS, I attempted to load the Qwen2.5-Coder-32B model using llama-server.

### The Data
As shown in my terminal logs, the ROCm0 model buffer size requested was 16123.35 MiB. With only 319.04 MiB mapped to the CPU, the GPU was being pushed to its absolute physical limit.

### The Crash
Because the model consumed nearly the entire 16GB of VRAM, the Linux kernel faced a critical memory shortage. To prevent a total system lockup, the Out-Of-Memory (OOM) Killer intervened:

* Google Chrome was terminated.
* `plasmashell.service` was terminated, effectively crashing my desktop environment.
* The `llama-server` process was ultimately terminated by signal **SIGKILL**.

### The Pivot
Stability is a prerequisite for a DevOps workflow. I have since pivoted to the **Qwen2.5-Coder-14B-Instruct.Q6_K.GGUF** model. This provides a high-performance coding assistant that fits comfortably within the RX 6800’s VRAM.
