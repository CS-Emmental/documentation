Configuration
-------------

.. highlight:: yaml

Backend
^^^^^^^

In the code, there is a default configuration and a develpoment configuration.
In production, a conf file overwrites the development configuration.

Development
"""""""""""

The simpliest configuration for develpoment is::

    SECRET_KEY="dev"
    MONGO_URI="mongodb://mongo:27017/cs-emmental"


Production
""""""""""

For production, we use configMap of kubernetes. The app finds the prod configuration in a python file, for exemple in ``/etc/config/back.conf.py``

Here is an exemple of configMap definition:

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
