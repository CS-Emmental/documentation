Deployment
----------

.. highlight:: yaml

Requirements
^^^^^^^^^^^^
You must have a Kubernetes cluster.

Platform deployment
^^^^^^^^^^^^^^^^^^^

.. warning:: This will expose the platform as a HTTP service. In production, you must deploy it in HTTPS!

First you need to clone the platform repository, and build the back and front
images::

    git clone adresse
    cd back
    docker build
    TODO: Finish these steps

Then, create a Kubernetes Deployment and Service to deploy the back:

    .. literalinclude:: ../files/back.yaml
      :language: yaml


.. note:: You should adapt the number of replicas to your needs.

Do the same for the front:

    .. literalinclude:: ../files/front.yaml
      :language: yaml


Now you can add an Ingress to expose the web server on the
network:

    .. literalinclude:: ../files/ingress.yaml
      :language: yaml

To deploy a mongo database, add the next ressources:

    .. literalinclude:: ../files/mongo.yaml
      :language: yaml