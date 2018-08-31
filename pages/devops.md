---
layout: page
title: Devops
tagline: Tools for building and deploying other tools.
description: Build and deploy stuff faster and more reliably
---

There is a lot to keep track of in this space.Here are some tools which I use:

* ansible
* docker
* docker-compose
* vagrant
* swarm
* git
* elk
* cmake
* make

And here are tools I _should_ be more familiar with:

* jenkins
* kubernetes

### Devops Docs Daze
Yet more links. Click click click.

#### Configuration Management
Once upon a time, folks wrote ad hoc scripts to handle configuration management. Now the field is crowded with
the likes of Puppet, Chef, Salt, and of course, Ansible. I happen to use Ansible, not because it is necessarily
the best, but because I know python well, and thus feel comfortable extending it. The others are mostly Ruby,
which I only have a passing familiarity with.

- [Ansible](devops/ansible.md) - Python based configration management

#### Docker
A Container in userspace, written in go and powered by awesome. Docker containers are designed to
isolate a process and allow it to speak to other processes via secure means.

- [Intro](devops/docker/docker.md)
- [update] (devops/docker/docker2.md)
- [Swarm](devops/docker/docker-swarm-mode.md)

#### Elasticsearch
- [intro](devops/elastic_search/elasticsearch_intro.md)
- [clusters](devops/elastic_search/clusters.md)
