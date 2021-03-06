Deployment
----------

.. highlight:: yaml

Requirements
^^^^^^^^^^^^
You must have a Kubernetes cluster, which must have:

* a namespace called *emmental-platform* (if you want to rename it, you must adapt all manifests)
* a namespace called *emmental-challenges* (cannot be changed for now)
* an Ingress controller enabled. For exemple, with Minikube, use ``minikube addons enable ingress``
* a networking plugin supporting Network Policies, like `Calico <https://www.projectcalico.org/>`_ (if using Minikube, it may be necessary to use ``--network-plugin=cni --extra-config=kubeadm.pod-network-cidr=${POD_CIDR} \``
with ``minikube start``, see `this issue <https://github.com/kubernetes/minikube/issues/6673>`_).

Downloadable manifests
^^^^^^^^^^^^^^^^^^^^^^
Kubernetes manifests can be downloaded here:

:download:`namespaces.yaml <../files/manifests/namespaces.yaml>`
:download:`authorization.yaml <../files/manifests/authorization.yaml>`
:download:`back.yaml <../files/manifests/back.yaml>`
:download:`front.yaml <../files/manifests/front.yaml>`
:download:`mongo.yaml <../files/manifests/mongo.yaml>`

Use these manifests one by one by using  ``kubectl apply -f <filename.yaml>``
or apply an entire manifest folder with ``kubectl apply -f <path/to/yaml/folder>``

.. note:: To shut down what you just created, just replace ``apply`` by ``delete`` in above commands

Set up Namespaces
^^^^^^^^^^^^^^^^^
:download:`namespaces.yaml <../files/manifests/namespaces.yaml>`

You need to have two namespaces, `emmental-platform` and `emmental-challenges`. You may use this manifest to create them:

    .. literalinclude:: ../files/manifests/namespaces.yaml

Set up Network Policies
^^^^^^^^^^^^^^^^^^^^^^^

.. warning:: These manifests assume you use calico as a networking plugin.


Set up Role Service Bindings
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
:download:`authorization.yaml <../files/manifests/authorization.yaml>`

We need to allow the platform to manage deployments and services.
This manifest defines a ServiceAccount, then a ClusterRole and then bind them with a ClusterRoleBinding:

    .. literalinclude:: ../files/manifests/authorization.yaml

Platform deployment
^^^^^^^^^^^^^^^^^^^
:download:`back.yaml <../files/manifests/back.yaml>`
:download:`front.yaml <../files/manifests/front.yaml>`
:download:`mongo.yaml <../files/manifests/mongo.yaml>`

.. warning:: This will expose the platform as a HTTP service. In production, you must deploy it in HTTPS!

First you need to clone the platform repository, and build the back and front images::

    git clone <plateform adress>
    cd plateform
    make build-prod

.. note:: This creates two images named ``front`` and ``back``.

Then, create a Kubernetes Deployment and Service to deploy the back:

    .. literalinclude:: ../files/manifests/back.yaml


.. note:: You should adapt the number of replicas to your needs.

Do the same for the front, which is exposed by an ingress:

    .. literalinclude:: ../files/manifests/front.yaml

To deploy a mongo database, add the next ressources, do not forget to customize the storage size and location on disk:

    .. literalinclude:: ../files/manifests/mongo.yaml

.. warning:: Using local storage will not work on multi-node clusters.
