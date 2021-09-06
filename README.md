# elastic-logstash-pod

Logstash Podman Pod.


## Prerequisites

Build Sphinx image:
```
podman build -t extra2000/sphinx .
```


## How to build docs into HTML

**NOTE:** The `chcon` commands are for SELinux platforms only.

Allow `./docs` to be mounted into Podman container:
```
chcon -R -v -t container_file_t ./docs
```

Create `./output` to store rendered HTML output and allow the directory to be mounted into Podman container:
```
mkdir -pv output
chcon -R -v -t container_file_t ./output
```

Build docs:
```
podman run -it --rm --network none -v ./docs:/srv/docs:ro -v ./output:/srv/docs/build:rw extra2000/sphinx make clean html
```


## How to deploy docs

Import SELinux Security Policy for `httpd` container:
```
sudo semodule -i docs_elastic_logstash_pod.cil /usr/share/udica/templates/{base_container.cil,net_container.cil}
```

Create `no-internet` network:
```
podman network create no-internet --internal
```

Deploy docs:
```
podman run --rm --network no-internet -p 18082:80 -v ./output/html:/usr/local/apache2/htdocs:ro --security-opt label=type:docs_elastic_logstash_pod.process docker.io/library/httpd:2.4
```

Deploy docs using pod:
```
podman play kube --network no-internet docs-elastic-logstash-pod.yaml
```
