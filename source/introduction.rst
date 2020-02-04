Introduction
------------

Welcome to the CS-Emmental documentation. CS-Emmental is a self-hosted,
open-source platform to host cybersecurity challenges and CTF. It supports any
arbitrary challenge type, since challenges are Docker containers. As long as
you can containerize the challenge you xant to create in one or more
containers, which can be connected by network or not, then that challenge can
be hosted on CS-Emmental.

The only requirement is to have a functional Kubernetes cluster. There is no
other requirement - you can deploy manually a Kubernetes cluster, use a managed

CS-Emmental has two main components: the platform, which is the website used by
users to browse and start available challenges and by admins to manage the
challenges, and the Kubernetes controller which manages the containers running
the challenges.
