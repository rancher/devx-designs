---
id: intro
label: Introduction
sidebar_position: 0
slug: /
---

# SUSE DevX validated designs

This repository contains documentation, automation scripts, and other code
artifacts for validated designs enabling end-to-end software development on SUSE
Rancher Prime. These designs focus on developer ease of use, build automation,
continuous deployment, security, and maintainability.

Some designs will use SUSE Rancher Prime components, while others will
incorporate features available from SUSE certified partners.

## 1. Rancher GitOps 

### A composable, customizable, Git-centered workflow

[GitOps for Rancher](gitops-overview.md) is a highly customizable
workflow, but requires project-specific configuration for each application. It
makes the IDE and Git UI the center of the Application Developer's experience.
Build and deployment automation is managed by Platform Engineers.

```mermaid
flowchart LR
    Dev((Developer))
    --> VSC("VS Code &
    Dev Containers") 
    o--o RD("Rancher Desktop
            (docker + k3s)")
    Dev <--> SO
    Dev <--> SS
    RD o--o SPR
    VSC --> GT("Gitea or
            GitLab")
        Dev --> CD("Coder /
        Workspaces") 
    subgraph "Development"
        CD --> GT --> GHA(Git* Action Runner)
        --> SPR[("SUSE 
        Private Registry")] <--> FLT(Fleet)
        CD o--o SPR
    end
    SO["SUSE Observability"] <--> Deployment 
    SS["SUSE Security"] <--> Deployment 
    FLT --> Deployment 

    subgraph "Deployment"
        NODE1(RKE2)
        NODE2(RKE2)
        NODE3(RKE2)
    end

    AC[(SUSE Registry &
    Application Collection)] --> SPR
```

All container images / OCI artifacts can be stored centrally by the organization
in SUSE Private Registry, with base images and supporting applications coming
from SUSE Registry and SUSE Application Collection.

## 2. Epinio 

### Lightweight Kubernetes-native PaaS

[Epinio](https://epinio.io/) is now developed, maintained and supported by
[Krumware](https://www.krum.io) in partnership with SUSE. This design provides a UI
and API specifically for app developers (independent of the source code control
used).
