# Contributing Guide

This project welcomes contributions from platform engineers and architects
who would like to help expand and improve DevX Validated Designs by SUSE. 

The initial intent of this repository is to facilitate collaboration between
SUSE platform architects and SUSE partners, but contributions from the wider
community are also reviewed and considered. 

If you would like to work with SUSE on developer platforms for your users or
customers, consider joining the [SUSE One partner
program](https://www.suse.com/partners/become-partner/).

## What makes a (good) DevX Validated Design? 

A DevX Validated Design should:
* describe in detail how to construct a repeatable software delivery workflow
* include configuration, templates, or automation code to assist in setting up the platform
* be built around software supported by SUSE, primarily [SUSE Rancher
  Prime](https://www.rancher.com/products/rancher-platform)
* optionally include software from [SUSE
  partners](https://www.suse.com/pcsc/home#search?platforms=1027) that adds
  features to the developer workflow or improves usability
* ideally, use open source software components so that these models can be used
  by the wider community

## Supporting your stack

System Integrator and consulting partners should submit designs that they are
prepared to support commercially for customers. 

ISV partners submitting designs which include their software should be able to
support the design for their customers and keep all integrations in the design
up-to-date and tested.

SUSE supports the SUSE software components included in these designs for
customers with valid support subscriptions. 

## Licensing

Contributions to this repo fall under two licenses depending on whether they are
documentation or code:

* Documentation in this repository is licensed under the [Creative Commons
  Attribution 4.0 International License](docs/LICENSE).
* Code in this repository outside of the `docs` directory is licensed under the
  [Apache-2.0](LICENSE).

Add the appropriate [SPDX license identifier](https://spdx.dev/learn/handling-license-info/) in a comment at the top of each file you contribute. For instance:
* `SPDX-License-Identifier: CC-BY-4.0` for Creative Commons-licensed content
* `SPDX-License-Identifier: Apache-2.0` for Apache-licensed code

## Issues & Pull requests

Prior to creating a pull request it is a good idea to [create an issue].
This is especially true if the change request is something large.
The bug, feature request, or other type of issue can be discussed prior to
creating the pull request. This can reduce rework.

[create an issue]: https://github.com/rancher/devx-designs/issues/new

## DCO

At the moment we require that contributors sign off on commits submitted to this
repository. The [Developer Certificate of Origin
(DCO)](https://probot.github.io/apps/dco/) is a way to certify that you wrote
and have the right to contribute the code you are submitting to the project.

You sign-off by adding the following to your commit messages. Your sign-off must
match the git user and email associated with the commit.

    This is my commit message

    Signed-off-by: Your Name <your.name@example.com>

Git has a `-s` command line option to do this automatically:

    git commit -s -m 'This is my commit message'

If you forgot to do this and have not yet pushed your changes to the remote
repository, you can amend your commit with the sign-off by running 

    git commit --amend -s 

