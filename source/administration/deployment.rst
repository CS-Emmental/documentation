Deployment
----------

.. highlight:: yaml

Requirements
^^^^^^^^^^^^
You must have a Kubernetes cluster, which must have:

* a namespace called *emmental-platform* (if you want to rename it, you must adapt all manifests)
* a namespace called *emmental-challenges* (cannot be changed for now)
* an Ingress controller enabled. For exemple, with Minikube, use ``minikube addons enable ingress``

Downloadable manifests
^^^^^^^^^^^^^^^^^^^^^^
Kubernetes manifests can be downloaded here:

:download:`namespaces.yaml <../files/namespaces.yaml>`
:download:`authorization.yaml <../files/authorization.yaml>`
:download:`mongo.yaml <../files/mongo.yaml>`
:download:`back.yaml <../files/back.yaml>`
:download:`front.yaml <../files/front.yaml>`

.. note:: Use these manifests by using ``kubectl apply -f <filename.yaml>``

Set up Namespaces
^^^^^^^^^^^^^^^^^
You need to have two namespaces, `emmental-platform` and `emmental-challenges`. You may use this manifest to create them:

    .. literalinclude:: ../files/namespaces.yaml


Set up Role Service Bindings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

We need to allow the platform to manage deployments and services.
This manifest defines a ServiceAccount, then a ClusterRole and then bind them with a ClusterRoleBinding:

    .. literalinclude:: ../files/authorization.yaml

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


.. note:: You should adapt the number of replicas to your needs.

Do the same for the front, which is exposed by an ingress:

    .. literalinclude:: ../files/front.yaml

To deploy a mongo database, add the next ressources, do not forget to customize the sotrage size and location on disk:

    .. literalinclude:: ../files/mongo.yaml

.. warning:: Using local storage will not work on multi-node clusters.