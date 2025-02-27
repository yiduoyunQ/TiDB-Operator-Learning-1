---
theme: academic
transition: fade
coverAuthor: Jirong Qiu
exportFilename: TiDB-Operator-Learning-1
hideInToc: true
lineNumbers: true
titleTemplate: '%s • Jirong Qiu'
---

# TiDB-Operator Learning 1

---

# Agenda

- Kubernetes Introduction
  - Historical context for Kubernetes
  - Kubernetes Components
  - Node and Pod
  - **Container Runtime**
  - Kube-Scheduler
  - Kube-Controller
  - **Logging Architecture**
- TiDB-Operator Introduction

---
layout: figure
figureCaption: 
figureUrl: https://kubernetes.io/images/docs/Container_Evolution.svg
---

# [Historical context for Kubernetes](https://kubernetes.io/docs/concepts/overview/#going-back-in-time)

---
layout: figure
figureCaption: An overview of the key components that make up a Kubernetes cluster
figureUrl: https://kubernetes.io/images/docs/components-of-kubernetes.svg
---

# [Kubernetes Components](https://kubernetes.io/docs/concepts/overview/components/)

---
layout: figure
figureCaption: Node (physical or virtual)
figureUrl: /node_and_pod.png
---

# [Node and Pod](https://kubernetes.feisky.xyz/introduction/concepts)

---

# [Container Runtime](https://www.ianlewis.org/en/container-runtimes-part-1-introduction-container-r)

- A container image format
- A method for building container images (Dockerfile/docker build)
- A way to manage container images (docker images, docker rm , etc.)
- A way to manage instances of containers (docker ps, docker rm , etc.)
- A way to share container images (docker push/pull)
- A way to run containers (docker run)

---
layout: figure
figureCaption: cgroups
figureUrl: /Linux_kernel_and_daemons_with_exclusive_access.svg.png
---

# [Container Runtime](https://en.wikipedia.org/wiki/Cgroups)

---

# [Container Runtime](https://www.ianlewis.org/en/container-runtimes-part-2-anatomy-low-level-contai)

```shell {1-3|5-6|8-11|13-18|20}
// sudo yum install libcgroup
// sudo yum install libcgroup-tools
// sudo yum install util-linux # unshare

UUID=$(uuidgen)
cgcreate -g cpu,memory:$UUID

cgset -r memory.limit_in_bytes=100000000 $UUID
cgset -r cpu.shares=512 $UUID
cgset -r cpu.cfs_period_us=1000000 $UUID
cgset -r cpu.cfs_quota_us=2000000 $UUID

$ cgexec -g cpu,memory:$UUID \
>     unshare --uts --ipc --net \
>     sh
sh-4.2# echo "Hello from in a container"
Hello from in a container
sh-4.2# exit

cgdelete -r -g cpu,memory:$UUID
```

<!-- Footer -->

Low-Level Container Runtime

---

# [Container Runtime](https://jvns.ca/blog/2016/10/10/what-even-is-a-container/)

```shell
# ls -l /sys/fs/cgroup/cpu/$UUID
总用量 0
-rw-rw-r-- 1 root root 0 2月  27 14:32 cgroup.clone_children
--w--w---- 1 root root 0 2月  27 14:32 cgroup.event_control
-rw-rw-r-- 1 root root 0 2月  27 14:32 cgroup.procs
-r--r--r-- 1 root root 0 2月  27 14:32 cpuacct.stat
-rw-rw-r-- 1 root root 0 2月  27 14:32 cpuacct.usage
-r--r--r-- 1 root root 0 2月  27 14:32 cpuacct.usage_percpu
-rw-rw-r-- 1 root root 0 2月  27 14:32 cpu.cfs_period_us
-rw-rw-r-- 1 root root 0 2月  27 14:32 cpu.cfs_quota_us
-rw-rw-r-- 1 root root 0 2月  27 14:32 cpu.rt_period_us
-rw-rw-r-- 1 root root 0 2月  27 14:32 cpu.rt_runtime_us
-rw-rw-r-- 1 root root 0 2月  27 14:32 cpu.shares
-r--r--r-- 1 root root 0 2月  27 14:32 cpu.stat
-rw-rw-r-- 1 root root 0 2月  27 14:32 notify_on_release
-rw-rw-r-- 1 root root 0 2月  27 14:32 tasks
```

---
layout: figure
figureCaption: cgroups level
figureUrl: /cgroups_level.png
---

# [Container Runtime](https://tech.meituan.com/2015/03/31/cgroups.html)

---
layout: figure
figureCaption: Low-Level and High-Level Container Runtimes
figureUrl: /Low-Level_and_High-Level_Container_Runtimes.png
---

# [Container Runtime](https://www.ianlewis.org/en/container-runtimes-part-1-introduction-container-r)

---
layout: figure
figureCaption: High-Level Container Runtimes
figureUrl: /high-level_runtime-architecture.png
---

# [Container Runtime](https://www.ianlewis.org/en/container-runtimes-part-3-high-level-runtimes)

---
layout: figure
figureCaption: Docker
figureUrl: /docker_runtime-architecture.png
---

# [Container Runtime](https://www.ianlewis.org/en/container-runtimes-part-3-high-level-runtimes)

---
layout: figure
figureCaption: Kubernetes Container Runtimes & CRI
figureUrl: /CRI.png
---

# [Container Runtime](https://www.ianlewis.org/en/container-runtimes-part-4-kubernetes-container-run)

---
layout: figure
figureCaption: The Docker Engine provides the following storage drivers on Linux
figureUrl: /docker_storage_driver.png
---

# [Container Runtime](https://docs.docker.com/engine/storage/drivers/select-storage-driver/)

---

# [Container Runtime](https://docs.docker.com/engine/storage/drivers/)

```dockerfile {all|3|4|5|6|7|8}
# syntax=docker/dockerfile:1

FROM ubuntu:22.04
LABEL org.opencontainers.image.authors="org@example.com"
COPY . /app
RUN make /app
RUN rm -r $HOME/.cache
CMD python /app/app.py
```


This Dockerfile contains four commands. Commands that modify the filesystem create a new layer. 

<v-clicks at="1">

- The `FROM` statement starts out by creating a layer from the ubuntu:22.04 image. 
- The LABEL command only modifies the image's metadata, and doesn't produce a new layer. 
- The COPY command adds some files from your Docker client's current directory. 
- The first RUN command builds your application using the make command, and writes the result to a new layer. 
- The second RUN command removes a cache directory, and writes the result to a new layer. 
- Finally, the CMD instruction specifies what command to run within the container, which only modifies the image's metadata, which doesn't produce an image layer.

</v-clicks>

<style>
p {
  font-size: 12px;
}
ul {
  font-size: 12px;
}
</style>

---
layout: figure
figureCaption: The diagram below shows a container based on an ubuntu:15.04 image
figureUrl: https://docs.docker.com/engine/storage/drivers/images/container-layers.webp
---

# [Container Runtime](https://docs.docker.com/engine/storage/drivers/)

---
layout: figure
figureCaption: overlay storage driver with Docker
figureUrl: https://docs.docker.com/engine/storage/drivers/images/overlay_constructs.webp
---

# [Container Runtime](https://docs.docker.com/engine/storage/drivers/overlayfs-driver/)

---
layout: figure
figureCaption: Scheduling in Kubernetes
figureUrl: https://kubernetes.io/blog/2023/01/12/protect-mission-critical-pods-priorityclass/kube-scheduler.svg
---

# [Kube-Scheduler](https://kubernetes.io/blog/2023/01/12/protect-mission-critical-pods-priorityclass/)

---
layout: figure
figureCaption: scheduling context of a Pod
figureUrl: https://kubernetes.io/images/docs/scheduling-framework-extensions.png
---

# [Kube-Scheduler](https://kubernetes.io/docs/concepts/scheduling-eviction/scheduling-framework/)

---
layout: figure
figureCaption: 
figureUrl: /kube-controller.png
---

# [Kube-Controller](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)

---
layout: figure
figureCaption: How nodes handle container logs
figureUrl: https://kubernetes.io/images/docs/user-guide/logging/logging-node-level.png
---

# [Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)

---
layout: figure
figureCaption: Using a node logging agent
figureUrl: https://kubernetes.io/images/docs/user-guide/logging/logging-with-node-agent.png
---

# [Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)

---
layout: figure
figureCaption: Streaming sidecar container
figureUrl: https://kubernetes.io/images/docs/user-guide/logging/logging-with-streaming-sidecar.png
---

# [Logging Architecture](https://kubernetes.io/docs/concepts/cluster-administration/logging/)

---
layout: figure
figureCaption: TiDB Architecture
figureUrl: https://download.pingcap.com/images/docs/tidb-architecture-v6.png
---

# [TiDB-Operator Introduction](https://docs.pingcap.com/tidb/stable/tidb-architecture)

---
layout: figure
figureCaption: TiDB Operator Architecture
figureUrl: /tidb-operator-overview.jpeg
---

# [TiDB-Operator Introduction](https://docs.pingcap.com/zh/tidb-in-kubernetes/stable/architecture)
