# Create GitLab Runners

[back](../README.md)

We want to create a Runner to use it in our *CI/CD* process. To create the Runner we need to download and install these two tools and create a account on [GitLab](https://gitlab.com).

* [Vagrant](https://www.vagrantup.com/downloads)
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads)


> ## Vagrant
>
> Vagrant is a tool for building and managing virtual machine environments in a single workflow. It provides easy to configure, reproducible, and portable work environments built on top of industry. Machines are provisioned on top of Virtualization Tools (VirtualBox, VMware, AWS, etc.).

> ## VirtualBox
>
> VirtualBox is a general-purpose full virtualizer for x86 and AMD64/Intel64 hardware, targeted at server, desktop and embedded use.

We assume that the process must work in different operating systems (Windows, Linux, Mac OS). But in this case we will use Windows 10 to realize this example.

Let's create a work directory

``` bash
  $ mkdir gitlab-runner
  $ cd gitlab-runner
```

Create a `Vagrantfile` file with the follow data:

``` sh
# The "2" represent the version of the configuration object config.
Vagrant.configure("2") do |config|
  # This configures what box the machine will be brought up against.
  config.vm.box = "bento/ubuntu-16.04"
  # The version of the box to use.
  config.vm.box_version = "202008.16.0"
  # Configures networks on the machine.
  config.vm.network "private_network", ip: "192.168.60.10"
  # The hostname the machine should have.
  config.vm.hostname = "gitlab"
  # Configures provider-specific configuration.
  config.vm.provider "virtualbox" do |v|
    v.memory = 3072
    v.cpus = 2
  end
end
```

Bring up and SSH a virtual machine.

``` sh
# Bring up a virtual machine
$ vagrant up

# SSH into the machine
$ vagrant ssh


  # Update and install packages to the virtual machine.

  # Update
  $ sudo apt-get update

  # Install packages
  # apt-transport-https - https download transport for APT.
  # ca-certificates - contains certificates provided by the Certificate Authorities.
  # curl - command line tool for transferring data with URL syntax.
  # gnupg-agent - GNU privacy guard - cryptographic agent.
  # software-properties-common - manage the repositories that you install software from (common).
  $ sudo apt-get install -Y apt-transport-https ca-certificates curl gnupg-agent software-properties-common

  # Add Docker official GPG key.
  $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

  # Set up the stable repository.
  $ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

  # Install Docker Community Edition.
  $ sudo apt-get update
  $ sudo apt-get install docker-ce

  # Check for correct installation of docker
  $ sudo docker info

  # Add docker group to user vagrant.
  $ sudo usermod -a -G docker vagrant
  $ exit
$ vagrant ssh
  $ id # naw the user vagrant is into docker group

  # Add gitlab runner
  # -d, --detached, the process run in background.
  # --name [name] - Set a name to container.
  # --restart [policy] - defines a restart policy for a container.
  # -v, --volume [host-src:]container-dest[:<options>] - Bind mount a volume.
  # image[:tag] - define the image and the version of the image that the container will run.
  $ sudo docker run -d --name gitlab-runner --restart always -v /srv/gitlab-runner/config:/etc/gitlab-runner -v /var/run/docker.sock:/var/run/docker.sock gitlab/gitlab-runner:latest

  # Enter to the container shell
  $ docker exec -ti gitlab-runner bash

    # Check machine info
    $ cat /ect/*ease*

    # To exit from the container
    $ exit

  # To exit from the virtual machine
  $ exit

# To Graceful shutdown of the virtual machine
$ vagrant halt

```

To Synchronize GitLab with the virtual machine

1. On GitLab we must create a project
1. Go to `Project` > `Settings` > `CI/CD` > `Runners` > `Expand` > `Set up a specific Runner manually`
1. Go to the gitlab-runner and register the runner using the data that appear at `Set up a specific Runner manually`.

``` bash
# Up the virtual machine
$ vagrant up
  # Enter into container shell
  $ docker exec -ti gitlab-runner bash
    # Register GitLab Runner
    $ gitlab-runner register

    # Enter the gitlab-ci coordinator URL

    # Enter the gitlab-ci token

    # Enter a description for the runner (e.g. runner1)

    # Enter a tag for the runner (e.g. runner1)

    # Enter the executor (docker)

    # Enter the default docker image (gitlab-runner)

    # Runner Registered Successfully.
```
Create the `.gitlab-ci.yml` file at the root of the `Project`.

It is possible create the file from GitLab choosing `bash` template.

It is possible add **tags** like in `build1`, `test1`, `test2` and `deploy1` and also it is possible add **conditions** with the *only* statement like in `test2`.

``` yml
# This file is a template, and might need editing before it works on your project.
# see https://docs.gitlab.com/ee/ci/yaml/README.html for all available options

# you can delete this line if you're not using Docker
image: busybox:latest

before_script:
  - echo "Before script section"
  - echo "For example you might run an update here or install a build dependency"
  - echo "Or perhaps you might print out some debugging details"

after_script:
  - echo "After script section"
  - echo "For example you might do some cleanup here"

build1:
  tags:
    - runner1
  stage: build
  script:
    - echo "Do your build here"

test1:
  tags:
    - runner1
  stage: test
  script:
    - echo "Do a test here"
    - echo "For example run a test suite"

test2:
  tags:
    - runner1
  stage: test
  script:
    - echo "Do another parallel test here"
    - echo "For example run a lint test"
  only:
    variables:
      - $TEST_LINUX == "true"

deploy1:
  tags:
    - runner1
  stage: deploy
  script:
    - echo "Do your deploy here"
```

Now the runner works any time that there any commit in the `Project` repository.
