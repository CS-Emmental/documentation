Configuration
-------------

.. highlight:: yaml

Downloadable manifests
^^^^^^^^^^^^^^^^^^^^^^
Kubernetes manifests can be downloaded here:

:download:`configmap-back.yaml <../files/configmap-back.yaml>`

Use these manifests one by one by using  ``kubectl apply -f <filename.yaml>``  
or apply an entire manifest folder with ``kubectl apply -f <path/to/yaml/folder>``

.. note:: To shut down what you just created, just replace ``apply`` by ``delete`` in above commands

Backend
^^^^^^^

The default configuration is the development one.
In production, a conf file overwrites the development configuration.

Development
"""""""""""

The simpliest configuration for develpoment is::

    SECRET_KEY="dev"
    MONGO_URI="mongodb://mongo:27017/cs-emmental"

Currently, this is hardcoded in ``app.py``. Check the code out.

Production
""""""""""

For production, we use configMap of kubernetes. The app finds the prod configuration in a python file, for exemple in ``/etc/config/back.conf.py``

Here is an exemple of configMap definition (:download:`configmap-back.yaml <../files/configmap-back.yaml>`):

    .. literalinclude:: ../files/configmap-back.yaml
        :language: yaml

.. warning:: SECRET_KEY must be some random bytes and kept secret. Read the (short) paragraph in the
    `official doc <https://flask.palletsprojects.com/en/1.1.x/quickstart/#sessions>`_ to learn how to properly set it


Then backend pods must use this configMap. To do so, in the backend manifest, set this key in ``spec.template.spec``: 

.. code-block:: yaml
    
    volumes:
      - name: config-volume
        configMap:
            name: configmap-back

And set this key in ``spec.template.spec.containers``:

.. code-block:: yaml
    
    volumeMounts:
      - name: config-volume
        mountPath: /etc/config


Finally, it should look like the file below:

    .. literalinclude:: ../files/back.yaml
        :language: yaml


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
