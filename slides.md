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
  - Kube-Scheduler
  - Kube-Controller
  - Logging Architecture
- TiDB-Operator Introduction

---
layout: figure
figureCaption: 
figureUrl: https://kubernetes.io/images/docs/Container_Evolution.svg
---

# Historical context for Kubernetes

<!-- https://kubernetes.io/docs/concepts/overview/#going-back-in-time -->

---
layout: figure
figureCaption: An overview of the key components that make up a Kubernetes cluster
figureUrl: https://kubernetes.io/images/docs/components-of-kubernetes.svg
---

# Kubernetes Components

<!-- https://kubernetes.io/docs/concepts/overview/components/ -->

---
layout: figure
figureCaption: Node (physical or virtual)
figureUrl: /node_and_pod.png
---

# Node and Pod

<!-- https://kubernetes.feisky.xyz/introduction/concepts -->

---
layout: figure
figureCaption: Scheduling in Kubernetes
figureUrl: https://kubernetes.io/blog/2023/01/12/protect-mission-critical-pods-priorityclass/kube-scheduler.svg
---

# Kube-Scheduler

<!-- https://kubernetes.io/blog/2023/01/12/protect-mission-critical-pods-priorityclass/ -->

---
layout: figure
figureCaption: scheduling context of a Pod
figureUrl: https://kubernetes.io/images/docs/scheduling-framework-extensions.png
---

# Kube-Scheduler

<!-- https://kubernetes.io/docs/concepts/scheduling-eviction/scheduling-framework/ -->

---
layout: figure
figureCaption: 
figureUrl: /kube-controller.png
---

# Kube-Controller

<!-- https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/ -->

---
layout: figure
figureCaption: How nodes handle container logs
figureUrl: https://kubernetes.io/images/docs/user-guide/logging/logging-node-level.png
---

# Logging Architecture

<!-- https://kubernetes.io/docs/concepts/cluster-administration/logging/ -->

---
layout: figure
figureCaption: Using a node logging agent
figureUrl: https://kubernetes.io/images/docs/user-guide/logging/logging-with-node-agent.png
---

# Logging Architecture

---
layout: figure
figureCaption: Streaming sidecar container
figureUrl: https://kubernetes.io/images/docs/user-guide/logging/logging-with-streaming-sidecar.png
---

# Logging Architecture

---
layout: figure
figureCaption: TiDB Architecture
figureUrl: https://download.pingcap.com/images/docs/tidb-architecture-v6.png
---

# TiDB-Operator Introduction

<!-- https://docs.pingcap.com/tidb/stable/tidb-architecture -->

---
layout: figure
figureCaption: TiDB Operator Architecture
figureUrl: /tidb-operator-overview.jpeg
---

# TiDB-Operator Introduction

<!-- https://docs.pingcap.com/zh/tidb-in-kubernetes/stable/architecture -->
