Host Preparations
=================

Preparing host for deploying Logstash.

Podman Installations
--------------------

Execute the following command to install Podman:

.. code-block:: bash

    sudo dnf install podman

SELinux Utilities
-----------------

Install ``udica`` which is required for simplifying SELinux for containers:

.. code-block:: bash

    sudo dnf install udica
