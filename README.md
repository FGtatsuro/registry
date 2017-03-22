registry
========

Private Docker repository for other services.

We assume that:

- This service works on Docker container.
- This service uses Amazon S3 as storage backend for container images.

Requirements
------------

The dependencies on other softwares/libraries for this service are as follows.

- Docker (>= 1.12.x)

For utility tasks, it's better to install following softwares/libraries.

- [Ansible](http://docs.ansible.com/ansible/index.html) (>= 2.2.x)

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
# These inventory file is for testing. Please use your inventory files.
# Start registry
$ ansible-playbook deploy/start_registry.yml -i tests/inventory/hosts -l test_host \
  --extra-vars="registry_storage_s3_accesskey=AKIAIOSFODNN7EXAMPLE \
  registry_storage_s3_secretkey=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY \
  registry_storage_s3_bucket=registry-bucket"

# Stop registry
$ ansible-playbook deploy/stop_registry.yml -i tests/inventory/hosts -l test_host
```

### Variables

There are variables related to the playbook to start service(`start_registry.yml`).

|name|description|default value|
|---|---|---|
|registry_storage_s3_region|`region` parameter of [s3 storage driver](https://docs.docker.com/registry/storage-drivers/s3/#parameters).|'ap-northeast-1'|
|registry_storage_s3_bucket|`bucket` parameter of [s3 storage driver](https://docs.docker.com/registry/storage-drivers/s3/#parameters). This is a required variable.|It isn't defined in default.|
|registry_storage_s3_rootdirectory|`rootdirectory` parameter of [s3 storage driver](https://docs.docker.com/registry/storage-drivers/s3/#parameters).|'/'|
|registry_storage_s3_accesskey|`accesskey` parameter of [s3 storage driver](https://docs.docker.com/registry/storage-drivers/s3/#parameters).|''(empty string)|
|registry_storage_s3_secretkey|`secretkey` parameter of [s3 storage driver](https://docs.docker.com/registry/storage-drivers/s3/#parameters).|''(empty string)|

Following variable related to both playbooks to start/stop service(`start_registry.yml` and `stop_registry.yml`)

|name|description|default value|
|---|---|---|
|registry_docker_command_become|If yes(true), docker command is executed with sudo.|false|

Test
----

Now, no test exists for this service.

Note/Limitation
---------------

- We must create S3 bucket(=`registry_storage_s3_bucket`) in specified region(=`registry_storage_s3_region`) before this service starts,
  and access the bucket from hosts where service runs with the permission `registry_storage_s3_accesskey`/`registry_storage_s3_secretkey` pair provides.
- Hosts which pull/push images via this service must accept insecure HTTP connection.
  Please check [Docker official document](https://docs.docker.com/registry/insecure/#/deploying-a-plain-http-registry).
