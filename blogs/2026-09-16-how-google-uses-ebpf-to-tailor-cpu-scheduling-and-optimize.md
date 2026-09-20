---
title: "How Google Uses eBPF to Tailor CPU Scheduling and Optimize Workload Performance"
url: "https://ebpf.foundation/how-google-uses-ebpf-to-tailor-cpu-scheduling-and-optimize-workload-performance-2/"
date: "2026-09-16"
author: "eBPF"
feed_url: "https://ebpf.foundation/feed/"
---
Download this case study in PDF version Overview Google faced significant infrastructure challenges due to the performance limitations of general-purpose kernel CPU schedulers, which could not optimize for the infrastructure’s highly specific application behaviors and hardware combinations. To resolve this, Google implemented application-tailored CPU scheduling policies using pluggable eBPF schedulers (originally ghOSt , and now via the sched_ext framework). This eBPF-driven architecture delivered a 5% queries per second (QPS) improvement in some of its core Remote Procedure Call (RPC) serving
