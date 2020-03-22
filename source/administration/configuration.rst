Configuration
-------------

.. highlight:: yaml

Downloadable manifests
^^^^^^^^^^^^^^^^^^^^^^
Kubernetes manifests can be downloaded here:

:download:`configmap-back.yaml <../files/manifests/configmap-back.yaml>`

Use these manifests one by one by using  ``kubectl apply -f <filename.yaml>``
or apply an entire manifest folder with ``kubectl apply -f <path/to/yaml/folder>``

.. note:: To shut down what you just created, just replace ``apply`` by ``delete`` in above commands.

Backend
^^^^^^^

The configuration is loaded depending on the mode of the app (development or production).

Development
"""""""""""

Currently, the configuration for development contains these keys::

    SECRET_KEY = "secretfordev"
    MONGO_URI = "mongodb://mongo:27017/cs-emmental"

    K8S_API_KEY = "k8s_api_key"
    K8S_HOST = "https://ip:port"

This configuration must be placed in ``back/instance/back.conf.py``.


Kubernetes API key
''''''''''''''''''

.. warning:: This section is only relevant in development.

:download:`namespaces.yaml <../files/manifests/namespaces.yaml>`
:download:`authorization.yaml <../files/manifests/authorization.yaml>`

Here are few steps to generate and get your kubernetes API key:

First, create the namespaces and the authorizations (service account, cluster role and role binding)::

    kubectl apply -f namespaces.yaml
    kubectl apply -f authorization.yaml

Then, store in an environment variable the name of the service account defined in authorization.yaml::

    SERVICE_ACCOUNT=api-emmental

Finally, run these commands to get the api key related to this service account::

    SECRET=$(kubectl get -n emmental-platform serviceaccount ${SERVICE_ACCOUNT} -o json | jq -Mr '.secrets[].name | select(contains("token"))')
    TOKEN=$(kubectl get -n emmental-platform secret ${SECRET} -o json | jq -Mr '.data.token' | base64 -d)

    echo $TOKEN

.. note:: ``kubectl get`` are namespaced with the flag ``-n emmental-platform``. Adapt the namespace according to your needs.

Now copy paste it in the conf file.

Testing
"""""""

Testing is very similar to development. However, please note this difference.
The configuration must be placed in ``back/instance/test.conf.py``.

Production
""""""""""

For production, we use a Kubernetes configMap. The app finds the prod configuration in a Python file, for exemple in ``/etc/config/back.conf.py``.

Kubernetes configMap
''''''''''''''''''''

Here is an exemple of configMap definition (:download:`configmap-back.yaml <../files/manifests/configmap-back.yaml>`):

    .. literalinclude:: ../files/manifests/configmap-back.yaml

.. warning:: SECRET_KEY must be some random bytes and kept secret. Read the (short) paragraph in the
    `official doc <https://flask.palletsprojects.com/en/1.1.x/quickstart/#sessions>`_ to learn how to properly set it


Then backend pods must use this configMap. To do so, in the backend manifest, set this key in ``spec.template.spec``:

.. code-block::

    volumes:
      - name: config-volume
        configMap:
            name: configmap-back

And set this key in ``spec.template.spec.containers``:

.. code-block::

    volumeMounts:
      - name: config-volume
        mountPath: /etc/config


Finally, it should look like the file below:

    .. literalinclude:: ../files/manifests/back.yaml


.. note:: In production, there is no need to define any Kubernetes configuration configuration file in the app. This is set up automatically.
            In details, containers find their configuration via the cluster.

Frontend
^^^^^^^^

The frontend configuration is managed via dotenv files. These files are used at webpack compilation time, in other words, when the frontend image is built.
The webpack only reads ``VUE_APP`` prefixed variable.

This is the actual configuration file we use:

.. code-block::

    VUE_APP_KUBERNETES_HOST=172.17.7.77

Development
"""""""""""

Development conf files are ``.env.development`` and ``.env.development.local``

Production
""""""""""

Production conf files are ``.env`` and ``.env.local``
