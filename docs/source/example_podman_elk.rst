Example Deployment for ELK using Podman
=======================================

Example how to deploy a small Logstash instance for ELK using Podman.

Getting Started
---------------

``cd`` into ``elastic-logstash-pod/deployment/examples/``:

.. code-block:: bash

    cd elastic-logstash-pod/deployment/examples/

Build our Logstash image:

.. code-block:: bash

    sudo podman build -t extra2000/elastic/logstash -f Dockerfile.amd64 .
    podman build -t extra2000/elastic/logstash -f Dockerfile.amd64 .

Create ``elknet`` podman network from ``elastic-elasticsearch-pod`` project.

Get CA from ``elastic-elasticsearch-pod`` Project
-------------------------------------------------

``cd`` into ``elastic-logstash-pod/deployment/examples/podman-elk``:

.. code-block:: bash

    cd elastic-logstash-pod/deployment/examples/podman-elk

Then, get CA certificate from ``_global_secrets_`` and ``es-coord-01`` instance:

.. code-block:: bash

    cp -v /path/to/elastic-elasticsearch-pod/deployment/_global_secrets_/elastic-ca.p12 ./secrets/
    cp -v /path/to/elastic-elasticsearch-pod/deployment/examples/cluster/es-coord-01/secrets/elasticsearch-ssl-http/logstash/elasticsearch-ca.pem ./secrets/elastic-ca.pem

Prerequisites for ``logstash-01``
---------------------------------

Creating SSL Certificate
~~~~~~~~~~~~~~~~~~~~~~~~

Ensure the ``./secrets`` directory is labeled as ``container_file_t``:

.. code-block:: bash

    chcon -R -v -t container_file_t ./secrets

Create SSL certificate:

.. code-block:: bash

    podman run -it --network none --rm -v ./secrets:/tmp/secrets:rw localhost/extra2000/elastic/elasticsearch ./bin/elasticsearch-certutil cert --ca /tmp/secrets/elastic-ca.p12 --pem --multiple

.. list-table:: Questions and answers for creating ``logstash-01``'s ``certificate-bundle.zip``
   :widths: 50 50
   :header-rows: 1

   * - Question
     - Answer
   * - Enter password for CA (``/tmp/secrets/elastic-ca.p12``)
     - ``abcde12345``
   * - Enter instance name
     - ``logstash-01``
   * - Enter name for directories and files of ``logstash-01``
     - ``logstash-01``
   * - Enter IP Addresses for instance
     - ``127.0.0.1``
   * - Enter DNS names for instance
     - ``elk-logstash-01.elknet``, ``logstash-01.yourhostname.lan``, ``localhost``
   * - Would you like to specify another instance?
     - ``n``
   * - Please enter the desired output file
     - ``/tmp/secrets/certificate-bundle.zip``

Extract the certificate archive:

.. code-block:: bash

    unzip ./secrets/certificate-bundle.zip -d ./secrets/certificate-bundle

For unknown reason, `the generated certificate key needed to be converted to PKCS8`_:

.. _the generated certificate key needed to be converted to PKCS8: https://discuss.elastic.co/t/logstash-ssl-file-does-not-contain-a-valid-private-key-with-beats/173229/2

.. code-block:: bash

    openssl pkcs8 -in ./secrets/certificate-bundle/logstash-01/logstash-01.key -topk8 -out ./secrets/certificate-bundle/logstash-01/logstash-01-pkcs8.key -nocrypt

Load SELinux Security Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    sudo semodule -i selinux/elk_logstash_01_pod_logstash_01.cil /usr/share/udica/templates/{base_container.cil,net_container.cil}

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "elk_logstash_01_pod_logstash_01"

Create configs and pipelines based on examples
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    cp -v ./configmaps/logstash-01.yaml{.example,}
    cp -v ./configs/logstash-01.yml{.example,}
    cp -v ./configs/logstash-01-pipelines.yml{.example,}
    cp -v ./pipelines/beats.conf{.example,}

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
            "manage_index_templates",
            "monitor",
            "manage_ilm"
          ],
          "index": [
            {
              "names": [
                "ecs-logstash-*",
                "filebeat-*",
                "winlogbeat-*",
                "metricbeat-*",
                "packetbeat-*",
                "heartbeat-*",
                "journalbeat-*"
              ],
              "privileges": [
                "write",
                "create",
                "create_index",
                "manage",
                "manage_ilm"
              ]
            }
          ]
        }
      },
      "metadata": {
        "application": "my-application",
        "environment": {
          "level": 1,
          "trusted": true,
          "tags": ["dev", "staging", "example"]
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

In ``configmaps/logstash-01.yaml``, replace ``ES_API_KEY`` value with your ``id:api_key`` for example ``hWSkl3sBCxVUyk5KV2rl:T-MHkne7Sb2GvhDGyu0OgA``.

Deployment
----------

Deploy ``logstash-01``
~~~~~~~~~~~~~~~~~~~~~~

Ensure all mount directories are labeled as ``container_file_t``:

.. code-block:: bash

    chcon -R -v -t container_file_t ./configs ./pipelines ./secrets

Then, deploy:

.. code-block:: bash

    sudo podman play kube --network elknet --configmap configmaps/logstash-01.yaml --seccomp-profile-root ./seccomp elk-logstash-01-pod.yaml

Beats Integrations
------------------

Create SSL Certs for Beats
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

    podman run -it --network none --rm -v ./secrets:/tmp/secrets:rw localhost/extra2000/elastic/elasticsearch ./bin/elasticsearch-certutil cert --ca /tmp/secrets/elastic-ca.p12 --pem --name beats

.. list-table:: Questions and answers for creating ``logstash-01``'s ``certificate-bundle.zip``
   :widths: 50 50
   :header-rows: 1

   * - Question
     - Answer
   * - Enter password for CA (``/tmp/secrets/elastic-ca.p12``)
     - ``abcde12345``
   * - Please enter the desired output file
     - ``/tmp/secrets/beats-certificate-bundle.zip``

Extract the certificate archive:

.. code-block:: bash

    unzip ./secrets/beats-certificate-bundle.zip -d ./secrets/beats-certificate-bundle

The certificates generated in ``./secrets/beats-certificate-bundle`` should be distributed to all Beats agents.

Get Elasticsearc Cluster UUID
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Login your Kibana instance as user ``elastic`` and execute the following command using ``Dev Tools``:

.. code-block:: bash

    GET /_cluster/state/cluster_uuid

If success, it will produce the following output:

.. code-block:: json

    {
      "cluster_name" : "elk-cluster-01",
      "cluster_uuid" : "4zWrMvXLQ1KKtApnZ7JIjw"
    }

Metricbeats Configurations Example
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Distribute certificates into device's directory:

* ``/opt/beats-certificate-bundle``
* ``/opt/elastic-ca.pem``

Set the following values in ``/etc/metricbeat/metricbeat.yml``:

.. code-block:: bash

    output.logstash:
      hosts: ["127.0.0.1:5044"]
      ssl.verification_mode: "full"
      ssl.certificate_authorities: ["/opt/elastic-ca.pem"]
      ssl.certificate: "/opt/beats-certificate-bundle/beats/beats.crt"
      ssl.key: "/opt/beats-certificate-bundle/beats/beats.key"

    monitoring:
      enabled: true
      cluster_uuid: "4zWrMvXLQ1KKtApnZ7JIjw"
      elasticsearch:
        hosts: ["https://127.0.0.1:9200"]
        username: beats_system
        password: abcde12345
        ssl.certificate_authorities: ["/opt/elastic-ca.pem"]

.. note::

    Comment ``cloud.id``, ``cloud.auth``, and all ``output.elasticsearch``.

Create Metricbeat Elasticsearch Template
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Create a temporary admin API key for managing Beats. Login your Kibana instance as user ``elastic`` and execute the following command using ``Dev Tools``:

.. code-block:: text

    POST /_security/api_key
    {
      "name": "tmp-beats-admin",
      "expiration": "1h",   
      "role_descriptors": { 
        "superuser": {
          "cluster": [
            "manage_index_templates",
            "monitor",
            "manage_ilm"
          ],
          "index": [
            {
              "names": [
                "metricbeat-*"
              ],
              "privileges": [
                "write",
                "create",
                "create_index",
                "manage",
                "manage_ilm"
              ]
            }
          ]
        }
      }
    }

If success, it will produce the following output:

.. code-block:: json

    {
      "id" : "aqvbpXsBFle_vVK8fjfJ",
      "name" : "tmp-beats-admin",
      "expiration" : 1630578830630,
      "api_key" : "beUH7QK9SFGwNAWPjhSmMA"
    }

Create Elasticsearch template for Metricbeat using the following command:

.. code-block:: bash

    sudo podman run --rm --network elknet -v ./secrets/elastic-ca.pem:/tmp/elastic-ca.pem:ro docker.elastic.co/beats/metricbeat:7.14.1 setup --index-management -E output.elasticsearch.ssl.verification_mode=full -E 'output.elasticsearch.ssl.certificate_authorities=["/tmp/elastic-ca.pem"]' -E 'output.elasticsearch.hosts=["https://elk-es-coord-01-pod.elknet:9200"]' -E 'output.elasticsearch.api_key="aqvbpXsBFle_vVK8fjfJ:beUH7QK9SFGwNAWPjhSmMA"'

Then, delete the temporary API key:

.. code-block:: text

    DELETE /_security/api_key
    {
      "name" : "tmp-beats-admin"
    }

Fine Tune Metricbeat ILM Policy
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Login your Kibana instance as user ``elastic`` and execute the following command using ``Dev Tools``:

.. code-block:: text

    PUT _ilm/policy/metricbeat
    {
      "policy": {
        "phases": {
          "hot": {
            "min_age": "0ms",
            "actions": {
              "rollover": {
                "max_size": "50gb",
                "max_age": "1h"
              },
              "forcemerge": {
                "max_num_segments": 1,
                "index_codec": "best_compression"
              },
              "shrink": {
                "number_of_shards": 1
              },
              "readonly": {}
            }
          },
          "warm": {
            "min_age": "1h",
            "actions": {
              "set_priority": {
                "priority": 50
              },
              "shrink": {
                "number_of_shards": 1
              },
              "forcemerge": {
                "max_num_segments": 1
              },
              "allocate": {
                "number_of_replicas": 1
              },
              "readonly": {}
            }
          },
          "cold": {
            "min_age": "2h",
            "actions": {
              "set_priority": {
                "priority": 0
              },
              "allocate": {
                "number_of_replicas": 1
              },
              "freeze": {},
              "readonly": {}
            }
          }
        }
      }
    }

.. note::

    This ILM Policy configuration is for testing purpose, you may need to change for production.
