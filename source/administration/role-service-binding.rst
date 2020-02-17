Role Service Binding
--------------------

.. highlight:: yaml

We will create a service account named `api-emmental`, binded to the ClusterRole `challenge-instance-manager`.
The role binding resource is called `api-emmental:challenge-instance-manager`

.. note:: The name of this service account must be specified in the backend deployment manifest, at `spec.template.spec.serviceAccountName`

Role
^^^^

First, define a cluster role::

    kind: ClusterRole
    apiVersion: rbac.authorization.k8s.io/v1beta1
    metadata:
        name: challenge-instance-manager
    rules:
        - apiGroups: ["", "apps"] # "" indicates the core API group
        resources: ["services", "deployments"]
        verbs: ["create", "delete", "read", "list"]

Then apply it:: 

    kubectl apply -f "clusterRole.yml"

Service
^^^^^^^

Create a service account (here in the default namespace)::

    kubectl create serviceaccount api-emmental


Binding
^^^^^^^

To bind the service account to the role, execute::

    kubectl create rolebinding api-emmental:challenge-instance-manager --clusterrole challenge-instance-manager --serviceaccount default:api-emmental