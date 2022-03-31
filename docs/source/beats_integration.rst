Beats Integration
=================

Create SSL Certs for Beats
--------------------------

.. code-block:: bash

    podman run -it --network none --rm -v ./secrets:/tmp/secrets:rw localhost/extra2000/elastic/elasticsearch ./bin/elasticsearch-certutil cert --ca /tmp/secrets/elastic-ca.p12 --pem --name beats

.. list-table:: Questions and answers for creating ``logstash``'s ``certificate-bundle.zip``
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

The certificates generated in ``./secrets/beats-certificate-bundle`` and also ``./secrets/elastic-ca.crt`` should be distributed to all Beats agents.

Create Metricbeat Elasticsearch Template
----------------------------------------

Create a temporary admin API key for managing Beats. Login your Kibana instance as user ``elastic`` and execute the following command using ``Dev Tools``:

.. code-block:: text

    POST /_security/api_key
    {
      "name": "tmp-metricbeat",
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
      "name" : "tmp-metricbeat",
      "expiration" : 1630578830630,
      "api_key" : "beUH7QK9SFGwNAWPjhSmMA"
    }

Create Elasticsearch template for Metricbeat using the following command:

.. code-block:: bash

    podman run --rm -v ./secrets/elastic-ca.crt:/tmp/elastic-ca.crt:ro docker.elastic.co/beats/metricbeat:8.1.0 metricbeat setup --index-management -E output.elasticsearch.ssl.verification_mode=full -E 'output.elasticsearch.ssl.certificate_authorities=["/tmp/elastic-ca.crt"]' -E 'output.elasticsearch.hosts=["https://es-coord-01.mydomain:9200"]' -E 'output.elasticsearch.api_key="aqvbpXsBFle_vVK8fjfJ:beUH7QK9SFGwNAWPjhSmMA"'

Delete the temporary API key:

.. code-block:: text

    DELETE /_security/api_key
    {
      "name" : "tmp-metricbeat"
    }

Fine Tune Metricbeat ILM Policy
-------------------------------

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
          },
          "delete": {
            "min_age": "3h",
            "actions": {
              "delete": {}
            }
          }
        }
      }
    }

.. note::

    This ILM Policy configuration is for testing purpose, you may need to change for production.

Create Filebeat Elasticsearch Template and Ingest Pipelines
-----------------------------------------------------------

Create a temporary admin API key for managing Filebeat. Login your Kibana instance as user ``elastic`` and execute the following command using ``Dev Tools``:

.. code-block:: text

    POST /_security/api_key
    {
      "name": "tmp-filebeat",
      "expiration": "1h",
      "role_descriptors": {
        "superuser": {
          "cluster": [
            "manage_ingest_pipelines",
            "manage_pipeline",
            "manage_index_templates",
            "monitor",
            "manage_ilm"
          ],
          "index": [
            {
              "names": [
                "filebeat-*"
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
      "id" : "xOL6In0BFNBv1FTCj6RH",
      "name" : "tmp-filebeat",
      "expiration" : 1636972985734,
      "api_key" : "x5UQUaftSjGfs8EAiw_MjA"
    }

Create Elasticsearch template for Filebeat using the following command:

.. code-block:: bash

    podman run --rm -v ./secrets/elastic-ca.crt:/tmp/elastic-ca.crt:ro docker.elastic.co/beats/filebeat:8.1.0 filebeat setup --index-management -E output.elasticsearch.ssl.verification_mode=full -E 'output.elasticsearch.ssl.certificate_authorities=["/tmp/elastic-ca.crt"]' -E 'output.elasticsearch.hosts=["https://es-coord-01.mydomain:9200"]' -E 'output.elasticsearch.api_key="xOL6In0BFNBv1FTCj6RH:x5UQUaftSjGfs8EAiw_MjA"'

Create ingest pipelines for processing Filebeat's Elasticsearch module (to be displayed on Kibana Stack Monitoring) using the following command:

.. code-block:: bash

    podman run --rm -v ./secrets/elastic-ca.crt:/tmp/elastic-ca.crt:ro docker.elastic.co/beats/filebeat:8.1.0 filebeat setup --pipelines --modules elasticsearch -M elasticsearch.server.enabled=true -M elasticsearch.gc.enabled=true -M elasticsearch.audit.enabled=true -M elasticsearch.slowlog.enabled=true -M elasticsearch.deprecation.enabled=true -E output.elasticsearch.ssl.verification_mode=full -E 'output.elasticsearch.ssl.certificate_authorities=["/tmp/elastic-ca.crt"]' -E 'output.elasticsearch.hosts=["https://es-coord-01.mydomain:9200"]' -E 'output.elasticsearch.api_key="xOL6In0BFNBv1FTCj6RH:x5UQUaftSjGfs8EAiw_MjA"'

Create ingest pipelines for processing Filebeat's NGINX module using the following command:

.. code-block:: bash

    podman run --rm -v ./secrets/elastic-ca.crt:/tmp/elastic-ca.crt:ro docker.elastic.co/beats/filebeat:8.1.0 filebeat setup --pipelines --modules nginx -M nginx.access.enabled=true -M nginx.error.enabled=true -E output.elasticsearch.ssl.verification_mode=full -E 'output.elasticsearch.ssl.certificate_authorities=["/tmp/elastic-ca.crt"]' -E 'output.elasticsearch.hosts=["https://es-coord-01.mydomain:9200"]' -E 'output.elasticsearch.api_key="xOL6In0BFNBv1FTCj6RH:x5UQUaftSjGfs8EAiw_MjA"'

Delete the temporary API key:

.. code-block:: text

    DELETE /_security/api_key
    {
      "name" : "tmp-filebeat"
    }

Fine Tune Filebeat ILM Policy
-----------------------------

Login your Kibana instance as user ``elastic`` and execute the following command using ``Dev Tools``:

.. code-block:: text

    PUT _ilm/policy/filebeat
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
          },
          "delete": {
            "min_age": "3h",
            "actions": {
              "delete": {}
            }
          }
        }
      }
    }

.. note::

    This ILM Policy configuration is for testing purpose, you may need to change for production.
