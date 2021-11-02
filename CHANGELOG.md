# Changelog

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
