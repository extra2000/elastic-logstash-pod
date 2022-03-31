Deployment for Production
=========================

Example how to deploy Logstash instance for production environment using Podman.

Getting Started
---------------

Clone repository and then ``cd`` into the project's root:

.. code-block:: bash

    mkdir ~/extra2000
    cd ~/extra2000
    git clone https://github.com/extra2000/elastic-logstash-pod.git
    cd elastic-logstash-pod

From the project's root directory, ``cd`` into ``deployment/production/logstash/``:

.. code-block:: bash

    cd deployment/production/logstash/

Build our Logstash image:

.. code-block:: bash

    podman build -t extra2000/elastic/logstash -f Dockerfile.amd64 .

Get CA from ``elastic-elasticsearch-pod`` Project
-------------------------------------------------

From the project's root directory, ``cd`` into ``deployment/production/logstash/``:

.. code-block:: bash

    cd deployment/production/logstash/

Then, get CA certificate from ``_global_secrets_`` and ``es-coord-01`` instance:

.. code-block:: bash

    cp -v /path/to/elastic-elasticsearch-pod/deployment/_global_secrets_/elastic-ca.p12 ./secrets/
    cp -v /path/to/elastic-elasticsearch-pod/deployment/cluster-multi-servers/es-coord-01/secrets/elasticsearch-ssl-http/kibana/elasticsearch-ca.pem ./secrets/elastic-ca.crt

.. warning::

    The ``elastic-ca.p12`` file is only used for signing certificates and should not be distributed.

Prerequisites for ``logstash``
------------------------------

Load SELinux Security Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create SELinux Security Policy:

.. code-block:: bash

    cp -v selinux/logstash_podman.cil{.example,}

Then load the security policy:

.. code-block:: bash

    sudo semodule -i selinux/logstash_podman.cil /usr/share/udica/templates/{base_container.cil,net_container.cil}

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "logstash_podman"

Create configs and pipelines based on examples
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    cp -v logstash-pod.yaml{.example,}
    cp -v ./configmaps/logstash.yaml{.example,}
    cp -v ./configs/logstash.yml{.example,}
    cp -v ./configs/logstash-pipelines.yml{.example,}
    cp -v ./pipelines/beats.conf{.example,}

Ensure configs, pipelines, and secrets readable by others:

.. code-block:: bash

    chmod -R o+r ./configs/* ./pipelines/* ./secrets/*
    chmod o+rx ./pipelines

Create Elasticsearch API key for Logstash
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Login your Kibana instance as user ``elastic`` and execute the following command using ``Dev Tools``:

.. code-block:: bash

    POST /_security/api_key
    {
      "name": "logstash",
      "expiration": "30d",
      "role_descriptors": {
        "superuser": {
          "cluster": [
            "monitor"
          ],
          "index": [
            {
              "names": [
                "ecs-logstash*",
                "filebeat-*",
                "winlogbeat-*",
                "metricbeat-*",
                "packetbeat-*",
                "heartbeat-*",
                "journalbeat-*",
                ".monitoring-*",
                "logs-*",
                "metrics-*",
                "synthetics-*"
              ],
              "privileges": [
                "write",
                "create_index"
              ]
            }
          ]
        }
      }
    }

If success, the command above will produce the following output for example:

.. code-block:: json

    {
      "id" : "hWSkl3sBCxVUyk5KV2rl",
      "name" : "logstash",
      "expiration" : 1632928735165,
      "api_key" : "T-MHkne7Sb2GvhDGyu0OgA"
    }

In ``configmaps/logstash.yaml``, replace ``ES_API_KEY`` value with your ``id:api_key`` for example ``hWSkl3sBCxVUyk5KV2rl:T-MHkne7Sb2GvhDGyu0OgA``.

Deployment
----------

Ensure all mount directories are labeled as ``container_file_t``:

.. code-block:: bash

    chcon -R -v -t container_file_t ./configs ./pipelines ./secrets

Then, deploy:

.. code-block:: bash

    podman play kube --configmap configmaps/logstash.yaml --seccomp-profile-root ./seccomp logstash-pod.yaml

Generate ``systemd`` files and enable on ``boot``:

.. code-block:: bash

    mkdir -pv ~/.config/systemd/user
    cd ~/.config/systemd/user
    podman generate systemd --files --name logstash-pod-srv01
    systemctl --user enable container-logstash-pod-srv01.service
