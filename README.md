# docker-container-with-config-files
Sometimes software isn't optimized to be used within a docker container. The most comfortable way would be to configure the instances with environment variables or cli parameters.

But very often software needs a config file. So how to deploy many containers with different configurations? You could create a docker image for every config-combination and push it to your registry. This can end in a lot of docker images.

Another less nice way is to mount the config path from the host-system into the container. But then you will mess up your host-system with config-files.

A practical way is to generate the config-files dynamically. So you can use one docker image for any number of docker containers with different configurations. This can be done with some steps in ansible.

**In this example ansible is used but the procedure can be equal in other tools.**

## Without config file
In "without-config-file.yml" you can see how to configure an instance of logstash that works with parameters and doesn't need a config-file.

## With config file
1. The example config-file is located in "templates". It contains a variable "port" that can be different for every host (see "hosts"-file), what will be injected for every current instance.
* In "with-config-file.yml" the command contains two "commands". The first one writes the config file into the container. The second one executes the program with the information, where the config-file is located. If it's not possible to set the path to the config file, the config-file must be written to the static path in the first command.
* That's it.

## !!! With config file and docker supported 'config' !!!
The most elegant way is to use "config" supported by docker in combination with a docker-compose.yml:

https://docs.docker.com/compose/compose-file/#configs

The current version of ansible has no module to manage configs in docker.
Unfortunately the configs in docker cannot be updated when they change.

## Execute
```
# Set the host user and password in hosts-file
ansible-playbook with-config-file.yml -i hosts
```
