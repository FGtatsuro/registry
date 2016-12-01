registry
========

Private Docker repository for other services.

We assume that:

- This service works on Docker container.
- This service uses Amazon S3 as storage backend for container images.

Requirements
------------

The dependencies on other softwares/libraries for this service are as follows.

- Docker (1.12.x)
- Docker Compose (1.9.x)

For tests/utility tasks, it's better to install following softwares/libraries.

- Ruby (>= 2.2.x)
- Rake (>= 11.x)

Build
-----

This service uses [pre-build image](https://hub.docker.com/_/registry/).

Config
------

This service has no config file.

Deploy
------

We can start/stop this service as follows.

```bash
# You must set specified environment variables before server starts. (Describe later)
# Start
$ (cd deploy/docker/ && docker-compose up -d)

# Stop
$ (cd deploy/docker/ && docker-compose down)
```

And helper tasks are defined to start/stop this service with environment variables.

```bash
# NOTE: Environment variables `AWS_ACCESS_KEY_ID`/`AWS_SECRET_ACCESS_KEY` should be set with secure way.
# Start
$ rake registry:start[ap-northeast-1,my-registry]

# Stop
$ rake registry:stop
```

### Variables

These are the environment variables to control this service. These must be set before this service starts.

|name|description|example value|
|---|---|---|
|REGISTRY_STORAGE_S3_ACCESSKEY|AWS access key id to access S3 bucket `REGISTRY_STORAGE_S3_BUCKET`. In default, the value of `AWS_ACCESS_KEY_ID` environment variable is used.|AKIAIOSFODNN7EXAMPLE|
|REGISTRY_STORAGE_S3_SECRETKEY|AWS secret access key to access S3 bucket `REGISTRY_STORAGE_S3_BUCKET`. In default, the value of `AWS_SECRET_ACCESS_KEY` environment variable is used.|wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY|
|REGISTRY_STORAGE_S3_REGION|Region where S3 bucket `REGISTRY_STORAGE_S3_BUCKET` exists|ap-northeast-1|
|REGISTRY_STORAGE_S3_BUCKET|S3 bucket which container images are saved in.|my-registry|

Test
----

Now, no test exists for this service.

Task
----

As we use tasks to deploy service, several utility tasks are defined. You can check them with `rake -D`.

Note/Limitation
---------------

- We must create S3 bucket `REGISTRY_STORAGE_S3_BUCKET` in region `REGISTRY_STORAGE_S3_REGION` before this service starts, and access the bucket from host service runs with the permission `REGISTRY_STORAGE_S3_ACCESSKEY`/`REGISTRY_STORAGE_S3_SECRETKEY` provides.
- Hosts which pull/push images via this service must accept insecure HTTP connection. Please check [Docker official document](https://docs.docker.com/registry/insecure/#/deploying-a-plain-http-registry).
