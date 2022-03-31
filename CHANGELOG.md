# Changelog

## [7.1.0](https://github.com/extra2000/elastic-logstash-pod/compare/v7.0.0...v7.1.0) (2022-03-31)


### Features

* **dockerfile:** update Logstash from `8.1.0` to `8.1.1` ([0da45f8](https://github.com/extra2000/elastic-logstash-pod/commit/0da45f8e7f225f9c2f4867b2581be6279585f28d))


### Styles

* **docs:** remove trailing spaces ([78003f8](https://github.com/extra2000/elastic-logstash-pod/commit/78003f82b5ea6f33d85e31b3debad98bbb1d6c3e))


### Documentations

* **deployment:** simplify `podman generate systemd` ([97d114e](https://github.com/extra2000/elastic-logstash-pod/commit/97d114e31f4c2294001fa8737ca5de739aefe0f1))
* **deployment:** update Beats from `8.1.0` to `8.1.1` ([0a9ae4e](https://github.com/extra2000/elastic-logstash-pod/commit/0a9ae4e865f11367d89c9b62d470c0238f3eb2fb))

## [7.0.0](https://github.com/extra2000/elastic-logstash-pod/compare/v6.0.0...v7.0.0) (2022-03-31)


### ⚠ BREAKING CHANGES

* **ssl:** path and mount point for SSL certs have changed

### Features

* **beats:** add pipelines for Nginx filebeat ([7916747](https://github.com/extra2000/elastic-logstash-pod/commit/79167476110774c9ef26f1810f4851d97d99159f))
* **dockerfiles:** upgrade Logstash from `8.0.1` to `8.1.0` ([3833c51](https://github.com/extra2000/elastic-logstash-pod/commit/3833c51fad4541bece2525f1ae9f67b79dc2af0f))


### Code Refactoring

* **ssl:** change to OpenSSL style ([aea5855](https://github.com/extra2000/elastic-logstash-pod/commit/aea5855599c0c60da23de93a4860a781a2c43972))


### Styles

* **docs:** remove trailing spaces ([526584c](https://github.com/extra2000/elastic-logstash-pod/commit/526584c12abf1fa1f63569edd6b27d26a5c115fe))


### Documentations

* **beats-integrations:** update Beats from `8.0.1` to `8.1.0` ([85c9172](https://github.com/extra2000/elastic-logstash-pod/commit/85c91728200198b0087d758e6c0a0d86623f3bf5))
* **deployment:** remove unnecessary API privileges ([5e97fb5](https://github.com/extra2000/elastic-logstash-pod/commit/5e97fb53b0ef3987b96b8863146f7ec8dcbc26a0))
* **deployments:** remove old SSL instructions ([aa22265](https://github.com/extra2000/elastic-logstash-pod/commit/aa222656c8c9b4639c0c839fc1f260f26452a651))

## [6.0.0](https://github.com/extra2000/elastic-logstash-pod/compare/v5.0.0...v6.0.0) (2022-03-10)


### ⚠ BREAKING CHANGES

* **deployments:** deployment structure has been reorganized

### Features

* **dockerfiles:** update Logstash from `8.0.0` to `8.0.1` ([d6c7b04](https://github.com/extra2000/elastic-logstash-pod/commit/d6c7b04c735c9a9fb54d2a52e0291cece6902a7e))


### Documentations

* **deployments:** update Beats from `8.0.0` to `8.0.1` ([5964a0c](https://github.com/extra2000/elastic-logstash-pod/commit/5964a0c43d8b45745c3aff455bf8bdc18166c5ad))


### Code Refactoring

* **deployments:** reorganize ([7520ce9](https://github.com/extra2000/elastic-logstash-pod/commit/7520ce943050dd1c797e5ca3dbcf888cce851660))
* **docs:** reorganize documentations ([ffcf9ed](https://github.com/extra2000/elastic-logstash-pod/commit/ffcf9ed5d634e02054c8b568bbdcfcee4609e5c6))


### Continuous Integrations

* **AppVeyor:** update workdir path ([e990f38](https://github.com/extra2000/elastic-logstash-pod/commit/e990f382e8e032f685fafdf4d9453151766d912b))

## [5.0.0](https://github.com/extra2000/elastic-logstash-pod/compare/v4.3.0...v5.0.0) (2022-02-20)


### ⚠ BREAKING CHANGES

* **beats:** beats pipeline no longer works for Elastic Stack 7.x

### Features

* **dockerfiles:** upgrade Logstash from `7.17.0` to `8.0.0` ([b0f3084](https://github.com/extra2000/elastic-logstash-pod/commit/b0f3084b5314f8e276adb0db915d62510d6750b5))


### Documentations

* **deployments:** rename `tmp-beats-admin` to `tmp-metricbeat` API key name ([2eaa6ce](https://github.com/extra2000/elastic-logstash-pod/commit/2eaa6ceeedcf64f112d11bc288961958f1f81ed5))
* **deployments:** update beats command prior to v8.0.0 ([0aae8b9](https://github.com/extra2000/elastic-logstash-pod/commit/0aae8b90f17b278e8bc9fedbfd019e5f89e6163b))
* **deployments:** update Beats from `7.17.0` to `8.0.0` ([9ea451b](https://github.com/extra2000/elastic-logstash-pod/commit/9ea451be840fe36f6a50f127f75c824b082a0642))


### Code Refactoring

* **beats:** change `.monitoring-` pipeline required for Elastic Stack 8.0 ([17b7a94](https://github.com/extra2000/elastic-logstash-pod/commit/17b7a94d6849aa1c4abbe4b1b9f5f54dabeef520))
* **deployments:** remove `podman-elk-rpi` deployment ([cdfe6db](https://github.com/extra2000/elastic-logstash-pod/commit/cdfe6dbcdb7401ece703e04c32159c73f68ec1e0))
* **deployments:** remove `podman-elk` deployment ([f459d89](https://github.com/extra2000/elastic-logstash-pod/commit/f459d89bf4778b9801d8fdd72d2e3764729cda5d))

## [4.3.0](https://github.com/extra2000/elastic-logstash-pod/compare/v4.2.0...v4.3.0) (2022-02-17)


### Features

* **beats:** add pipelines for NGINX Filebeat module ([eab29e8](https://github.com/extra2000/elastic-logstash-pod/commit/eab29e87b096fac179f787b8bdba6793f5d35d53))
* **dockerfile:** upgrade Logstash from `7.16.3` to `7.17.0` ([11cae55](https://github.com/extra2000/elastic-logstash-pod/commit/11cae55fdbd8c5fee817fa4cd5b801d6138da267))


### Styles

* **docs:** remove trailing spaces ([0d05bd8](https://github.com/extra2000/elastic-logstash-pod/commit/0d05bd88a305bf1ed059f33840372f1848844399))


### Documentations

* **deployments:** update Beats from `7.16.3` to `7.17.0` ([047a65d](https://github.com/extra2000/elastic-logstash-pod/commit/047a65d428a626d40a05c317d5b8eaaddee2a9de))
* **podman-general:** add instructions to create NGINX ingest pipeline ([db181da](https://github.com/extra2000/elastic-logstash-pod/commit/db181da4217d0863deb19d64dbf95d9ae8597fac))

## [4.2.0](https://github.com/extra2000/elastic-logstash-pod/compare/v4.1.0...v4.2.0) (2022-01-29)


### Features

* **dockerfile:** upgrade Logstash from `7.16.2` to `7.16.3` ([789cf17](https://github.com/extra2000/elastic-logstash-pod/commit/789cf17eccbf4952558f03ff34d54da6d92b1844))


### Documentations

* **deployments:** update Beats from `7.16.2` to `7.16.3` ([5e6f860](https://github.com/extra2000/elastic-logstash-pod/commit/5e6f860a8ca569bf8050be3f348bc8165dcc6cf5))
* **logo:** remove logo ([197e7f5](https://github.com/extra2000/elastic-logstash-pod/commit/197e7f502380898d99dffe2d15c73678a74eefcf))

## [4.1.0](https://github.com/extra2000/elastic-logstash-pod/compare/v4.0.1...v4.1.0) (2022-01-04)


### Features

* **pipelines/beats:** add Filebeat pipelines for processing Elasticsearch module to be displayed on Stack Monitoring ([c129f99](https://github.com/extra2000/elastic-logstash-pod/commit/c129f991d510d78402141e74c62cc52a4f65b98e))


### Documentations

* **deployments-general:** add instructions to create Filebeat template and ingest pipelines for Filebeat's Elasticsearch module ([6dee685](https://github.com/extra2000/elastic-logstash-pod/commit/6dee685f9fafb09d7f53e6b9dc36f948491747ba))

### [4.0.1](https://github.com/extra2000/elastic-logstash-pod/compare/v4.0.0...v4.0.1) (2022-01-02)


### Fixes

* **pipeline:** fix incorrect shard counting caused by Metricbeat ([7521d1f](https://github.com/extra2000/elastic-logstash-pod/commit/7521d1f6fbe72cd1417d85b071ad90a908195e75))

## [4.0.0](https://github.com/extra2000/elastic-logstash-pod/compare/v3.0.1...v4.0.0) (2021-12-31)


### ⚠ BREAKING CHANGES

* **configs:** Stack Monitoring with self-monitoring no longer works. Instead, use Metricbeat to ship metrics for Stack Monitoring.
* **xpack:** Legacy stack monitoring will no longer works
* **pipelines/beats:** Beats pipelines are now default to datastream

### Features

* **dockerfile:** upgrade Logstash from `7.15.2` to `7.16.2` ([10fd2d5](https://github.com/extra2000/elastic-logstash-pod/commit/10fd2d5fd83cfc8bffdd305a873a0a898ac850d9))
* **pipeline:** add Stack Monitoring index `.monitoring-` into Beats pipeline ([b834994](https://github.com/extra2000/elastic-logstash-pod/commit/b834994482d0951bb9cf588250da7c1f492136f2))


### Code Refactoring

* **pipelines/beats:** using datastream instead of indices ([ffe35b7](https://github.com/extra2000/elastic-logstash-pod/commit/ffe35b7c3baa516fae1a9ea683f0e60a857436a0))
* **xpack:** remove deprecated `xpack.monitoring` ([acfb085](https://github.com/extra2000/elastic-logstash-pod/commit/acfb08512aff7d19fbd87e23195e9041685b624b))


### Fixes

* **configs:** add `monitoring.cluster_uuid` ([e1708ce](https://github.com/extra2000/elastic-logstash-pod/commit/e1708ce9e937f79e2f35286795ceea0968aea770))
* **configs:** add `monitoring.enabled: false` ([8d6d3b4](https://github.com/extra2000/elastic-logstash-pod/commit/8d6d3b45501e6f3262f0fd1ba8222d565b3825a9))
* **configs:** add `pipeline.ecs_compatibility: disabled` to surpress ECS compatibility warnings ([a07dd74](https://github.com/extra2000/elastic-logstash-pod/commit/a07dd7460aa8913681b4665cc6a26d9128a2b73d))


### Documentations

* **deployments-general:** need to distribute `./secrets/elastic-ca.pem` to Beats ([8516150](https://github.com/extra2000/elastic-logstash-pod/commit/8516150a322b73bb07713780ff737de8f44115d2))
* **deployments:** add `.monitoring-` index privilege ([b915a2b](https://github.com/extra2000/elastic-logstash-pod/commit/b915a2b3e5e403fa292ddf701134c499c90e5cda))
* **deployments:** add datastream indices privileges ([3b5e86a](https://github.com/extra2000/elastic-logstash-pod/commit/3b5e86ab46348cc8a943dfa0bc774e6fcd884785))
* **deployments:** update Beats from `7.15.2` to `7.16.2` ([25e542b](https://github.com/extra2000/elastic-logstash-pod/commit/25e542b14bd5a6bd42f716834c7576662786b0c5))

### [3.0.1](https://github.com/extra2000/elastic-logstash-pod/compare/v3.0.0...v3.0.1) (2021-12-04)


### Documentations

* **deployments:** fix `chmod` command ([3a32812](https://github.com/extra2000/elastic-logstash-pod/commit/3a3281242b9e95752722349edf799327fb4d0ca9))

## [3.0.0](https://github.com/extra2000/elastic-logstash-pod/compare/v2.0.0...v3.0.0) (2021-11-29)


### ⚠ BREAKING CHANGES

* **deployments:** we no longer support rootful Podman deployments

### Code Refactoring

* **pods:** remove `runAsGroup` and `runAsUser` because they are not needed by rootless Podman ([3d267c6](https://github.com/extra2000/elastic-logstash-pod/commit/3d267c6631880ef2540a1898a48d6621b9510ed2))
* **selinux:** remove unnecessary `container_file_t` and `user_home_t` ([514359f](https://github.com/extra2000/elastic-logstash-pod/commit/514359f983d2b7839aa52070dadb20a9fbed7b79))
* **sphinx:** change layout to full width ([faedc99](https://github.com/extra2000/elastic-logstash-pod/commit/faedc99fa9c837815e8f771e4ca6391194c43a31))


### Documentations

* **deployments:** add instructions to clone repository ([94a0400](https://github.com/extra2000/elastic-logstash-pod/commit/94a04001f9ca0f52917a86c827e5f36778f87f3d))
* **deployments:** add instructions to verify certs and upload secrets ([e7ce5b6](https://github.com/extra2000/elastic-logstash-pod/commit/e7ce5b6242f9034ac63153f3add6e365c95b22b8))
* **deployments:** change rootful Podman to rootless Podman instructions ([9fd90f5](https://github.com/extra2000/elastic-logstash-pod/commit/9fd90f57ba88f48d471415d7ee816938188f0afa))
* **deployments:** don't distribute `elastic-ca.p12` ([1487cbd](https://github.com/extra2000/elastic-logstash-pod/commit/1487cbd3f813218e466eb56f9ed3ca0cdceeb0e2))
* **deployments:** ensures `configs/`, `pipelines/`, and `secrets/` readable by others ([596440f](https://github.com/extra2000/elastic-logstash-pod/commit/596440f9d8989203590abd3a623af58bb5c264c5))
* **deployments:** improve `cd` instructions ([c28e8fc](https://github.com/extra2000/elastic-logstash-pod/commit/c28e8fc755de08fac6168e0cba0a72f8c1f36968))
* **host-preparations:** update instructions for rootless Podman ([d01d79b](https://github.com/extra2000/elastic-logstash-pod/commit/d01d79bda9d7b4d8c1a7c8f28ef997f1f5fa57f7))

## [2.0.0](https://github.com/extra2000/elastic-logstash-pod/compare/v1.3.1...v2.0.0) (2021-11-22)


### ⚠ BREAKING CHANGES

* **pods:** pod files have been removed and become example file

### Code Refactoring

* **pods:** remove pod file and make it as example file ([beb0226](https://github.com/extra2000/elastic-logstash-pod/commit/beb02268d658491a368793769f9e010a078ba929))


### Documentations

* **deployments:** add instructions to create pod file ([df25630](https://github.com/extra2000/elastic-logstash-pod/commit/df25630a49cc2632e0a62eee1a9ce6e68b53c9cf))
* **README:** add badges ([a5c6dc8](https://github.com/extra2000/elastic-logstash-pod/commit/a5c6dc8d3010b5eb6d3b32f4fedeb9e1daeca75f))

### [1.3.1](https://github.com/extra2000/elastic-logstash-pod/compare/v1.3.0...v1.3.1) (2021-11-15)


### Documentations

* **deployment:** add instructions to create Filebeat template ([b060307](https://github.com/extra2000/elastic-logstash-pod/commit/b060307974f4b634d208e07a8e5771541f8f5004))
* **logstash:** add missing output for Winlogbeat ([8cb9b7b](https://github.com/extra2000/elastic-logstash-pod/commit/8cb9b7b6478aac719b63c7d29233f806d8c0cb21))
* **logstash:** add output for Filebeat ([eafee44](https://github.com/extra2000/elastic-logstash-pod/commit/eafee447eb00144259078e04511c850d45781e7c))

## [1.3.0](https://github.com/extra2000/elastic-logstash-pod/compare/v1.2.1...v1.3.0) (2021-11-12)


### Features

* **logstash:** update version `7.15.1` to `7.15.2` ([fe83809](https://github.com/extra2000/elastic-logstash-pod/commit/fe838093f58e7ff3974864666e1bbeedaba195a3))


### Documentations

* **deployment:** add instructions for creating Winlogbeat template ([adf7a39](https://github.com/extra2000/elastic-logstash-pod/commit/adf7a39267400b788f0ca7acc2c2a26a8ead3ead))
* **README:** change "beats" to Metricbeat ([f91e5e9](https://github.com/extra2000/elastic-logstash-pod/commit/f91e5e99ef4067775c8b43fecce891509c891d33))

### [1.2.1](https://github.com/extra2000/elastic-logstash-pod/compare/v1.2.0...v1.2.1) (2021-11-02)


### Documentations

* **deployment:** fix typo `ecs-logstash` index name ([37d16f2](https://github.com/extra2000/elastic-logstash-pod/commit/37d16f2234e6b676ab0539b21defc1898a312c17))
* **examples:** fix path for `elastic-ca.pem` ([3de11b6](https://github.com/extra2000/elastic-logstash-pod/commit/3de11b6ed15049ba5aea858bccc3ed8900b1fe12))
* **metricbeat:** add delete phase ([0de0640](https://github.com/extra2000/elastic-logstash-pod/commit/0de0640e6590965097e15eb214556c46ebe32594))
* **pipeline:** change metricbeat from version 7.15.0 to 7.15.1 ([5ffbf13](https://github.com/extra2000/elastic-logstash-pod/commit/5ffbf133e4c10e612aac123ceb15704feed6b29c))

## [1.2.0](https://github.com/extra2000/elastic-logstash-pod/compare/v1.1.0...v1.2.0) (2021-10-25)


### Features

* **logstash:** upgrade from version `7.15.0` to `7.15.1` ([dbebacd](https://github.com/extra2000/elastic-logstash-pod/commit/dbebacd26fe5c781654a5f4f90a361e20c512e35))

## [1.1.0](https://github.com/extra2000/elastic-logstash-pod/compare/v1.0.0...v1.1.0) (2021-10-23)


### Features

* **deployment:** add Podman example for RPi ([1714dd8](https://github.com/extra2000/elastic-logstash-pod/commit/1714dd8fa95fbf647c6d61a3274091190122069f))
* **dockerfile:** upgrade Logstash from `7.14.1` to `7.15.0` ([58a9db7](https://github.com/extra2000/elastic-logstash-pod/commit/58a9db780f118f30b6517f0a3fc052edc4236a4f))


### Fixes

* **memory:** set heap limit and increase memory limit ([77badc0](https://github.com/extra2000/elastic-logstash-pod/commit/77badc085870ca9407bd141689a47353bb0d1a6d))


### Documentations

* **pipelines:** update Metricbeat from `7.14.x` to `7.15.0` ([7f9b9b2](https://github.com/extra2000/elastic-logstash-pod/commit/7f9b9b2dbbebd3b909b376ff877beda1985251c6))
* update Metricbeats from `7.14.1` to `7.15.0` ([910c02e](https://github.com/extra2000/elastic-logstash-pod/commit/910c02e55bb4a24f1ef1332f12464ac7b394a98d))


### Continuous Integrations

* **appveyor:** ensure CA certs always latest ([ecdc307](https://github.com/extra2000/elastic-logstash-pod/commit/ecdc307762aeebd0fc51e9ab83a88f452564158c))
* **appveyor:** upgrade Ubuntu from `18.04` to `20.04` ([4c977e1](https://github.com/extra2000/elastic-logstash-pod/commit/4c977e17827d91ac4dbf1276f3c2183ab17d94e6))

## 1.0.0 (2021-09-06)


### Features

* initial commit ([93861cd](https://github.com/extra2000/elastic-logstash-pod/commit/93861cda57ff00bc8423140a54b09eda7b1572ed))


### Documentations

* **README:** update `README.md` ([8d94c26](https://github.com/extra2000/elastic-logstash-pod/commit/8d94c26f361901e2fb63c5de3b73e623d3d1d648))


### Continuous Integrations

* add AppVeyor with `semantic-release` ([a0f8334](https://github.com/extra2000/elastic-logstash-pod/commit/a0f833415ba9034aa71a163d4869b5515484e39f))
