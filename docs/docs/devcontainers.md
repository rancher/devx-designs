---
id: gitops-devcontainers
sidebar_position: 20
# slug: /1-Rancher-GitOps/DevContainers
---

# SUSE Dev Containers

[Development Containers](https://containers.dev/) (AKA Dev Containers) are
container images (and a spec for supporting tools and editors) that provide a
compartmentalized workspace for developing and testing software. They provide an
incredibly quick and straightforward way to configure a development environment
in IDEs that support them, and do away with the need to manually configure a
local system before starting on a software project.

The dev container can be built and configured by a project lead, architect, or
platform engineer. Once this is set up in the `.devcontainer` directory of the
project, all contributors benefit from the fast and consistent setup.

Dev containers should be:
* **updated frequently** so that the latest patches for all supporting software are made available to all contributors
* **maintained by platform engineers** or a persona with the responsibility for maintaining the security for the dependency chain
* **served from a local private registry** to increase speed of container image downloads
* **pinned to a version number** so that application builds are reproduceable
* **built with the same base as the deployment image** so that the application can be expected to behave the same in development and in production

SUSE Linux Base Container Images (BCI) are used as the secure starting point for
dev containers and the base containers for the applications as they are
ultimately deployed on the platform.

## Get a SUSE runtime image

One of the most important parts of securing the software delivery supply chain
is starting with a secure, up to date base image. 

[SUSE Linux Base Container Images (BCI)](https://www.suse.com/products/base-container-images/) 
are the basis of SUSE SDK and language runtime images. These are available from:

* [SUSE Application Collection](https://apps.rancher.io/applications?category=category%3Asdk)
* [SUSE Registry](https://registry.suse.com/repositories?categories%5B%5D=bci-devel)

Each image will have multiple branches and versions available. As a rule, you
should choose the most recent branch and version that will work with your
software. The terminology "branch" and "version" may apply to major and minor
version numbers, depending on the language you choose.    

Images from SUSE Application Collection intentionally do _not_ have the `latest`
tag. This is used as a forcing function to make sure development and deployment
images are pinned to a specific version, so builds will be reproduceable.
Techniques for automatically incrementing the version numbers will be addressed
later in the guide. 

## Build your Dev Container

You can build your own runtime images starting with a SUSE Linux BCI, but the
runtime images give you a head start with recent builds of the most commonly
used language runtimes. 

In SUSE Application Collection, the 
[SDK & Runtime images](https://apps.rancher.io/applications?category=category%3Asdk) 
have two variants:
* base images without any package manager or additional helper utilities, and
* dev images (with a `-dev` suffix) with the `zypper` package manager and some
  other helpful utilities (`curl`, `openssl`, `grep`, etc.) 

:::note

Pulling images from SUSE Application Collection requires 
[authentication](https://docs.apps.rancher.io/get-started/authentication/).

You will need to use the `-dev` images to create a dev container suitable for
use with an IDE or workspace. For example, a `Dockerfile` for a node.js web app
might look like this:

```docker
FROM dp.apps.rancher.io/containers/nodejs:22-dev

RUN zypper --non-interactive install -y git openssh make awk
RUN useradd -ms /bin/bash node

USER node 
WORKDIR /home/node
EXPOSE 3000
```

### Manual build

When developing the dev container configuration for a project, you will likely
need to iterate a few times to get all the dependencies required for your
software. In this phase, you can work in your IDE with a `.devcontainers`
directory in your project base. In this directory create your `Dockerfile` like
the one above and an adjacent `devcontainer.json`:

```json
{
  "name": "Node.js",
  "build": {
    "dockerfile": "Dockerfile"
  },

  // optional for VS Code
  "customizations": {
    "vscode": {
      "extensions": [
        // VS Code extensions to install when the container is created.
      ]
    }
  },

  // a list of ports inside the container forwarded to localhost
  "forwardPorts": [3000],

  // commands to run after the container is created
  "postCreateCommand": "npm install",

  // run as the non-root user defined in your Dockerfile
  "remoteUser": "node"
}
```

Images can be built locally by a Platform Engineer, then pushed to an OCI
registry residing on the target cluster (e.g. SUSE Private Registry or Harbor).
This has a few benefits:

1. images are under the control of the Platform Engineering team
2. images are kept local to hosted workspaces, actions, and build systems that rely on them
3. developers do not need to build the dev containers themselves

The version of `.devcontainers/devcontainers.json` committed to the platfom
would reference the local OCI registry:

```json
{
  "name": "Node.js",
  image": "intreg.example.com/project/workspace-nodejs:22-dev"
  },
  ...
}
```




### Automating updates

Container images should always be pinned to a specific version or build. SDK and
runtime images from SUSE Application Collection do not expose a `latest` tag to
enforce this practice.  

Package updates are published in the SUSE Application Collection [RSS feed](https://apps.rancher.io/feed), and can be checked for individual artifacts via the [REST API](https://api.apps.rancher.io/swagger-ui/index.html). 

