Challenges
----------

.. highlight:: yaml

A challenge is made of one or several Docker images. The images must either be available on Docker Hub, or locally on every Kubernetes node.

In the web interface, when creating a challenge, the "infrastructure" field must contain a YAML based on this exemple:

.. literalinclude:: ../files/exemple-challenge.yaml
