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

- [Ansible](devops/ansible.html) - Python based configration management

#### Docker
A Container in userspace, written in go and powered by awesome. Docker containers are designed to
isolate a process and allow it to speak to other processes via secure means.

- [Intro](devops/docker/docker.html)
- [update](devops/docker/docker2.html)
- [Swarm](devops/docker/docker-swarm-mode.html)
- [Dockerfile Details](devops/docker/building_dockerfile.html)

### Kubernetes
- [Intro](devops/docker/kubernetes.html)

#### Elasticsearch
- [intro](devops/elastic_search/elasticsearch_intro.html)
- [intro part 2](devops/elastic_search/elasticsearch_intro_2.html)
- [clusters](devops/elastic_search/clusters.html)
