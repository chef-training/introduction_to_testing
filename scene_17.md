## Test Kitchen - Tool

[Test Kitchen](http://kitchen.ci/) is a test harness tool to execute your configured code on one or more platforms in isolation. A driver plugin architecture is used which lets you run your code on various cloud providers and virtualization technologies.

"It is designed to execute isolated code run in a pristine environment ensuring that no prior state exists."

Kitchen gives us the ability to very quickly define a matrix of recipes we want to converge across multiple platforms.

This is a great tool for local development as it allows you to preview the effects of the recipes you have written in an environment that is easy to commision and then decomission.

Test Kitchen is able to work with a large number of drivers that allow you to provision local virtual machines or provision a machine from a cloud provider. With local development we often use drivers like vagrant or docker.

Test Kitchen provides a command-line application which allows you to control the test matrix that you have defined in your `.kitchen.yml`.

```bash
$ kitchen --help
```

Each cookbook is now commonly generated with a `.kitchen.yml` file. If the cookbook is missing this file you can generate it with a kitchen init command

```bash
$ kitchen help init
$ kitchen init
```

The `.kitchen.yml` file is a YAML file. It is white-space significant formatted file, like python. The kitchen YAML, as its often called, allows for us to configure the way that test kitchen will operate as well as define our test matrix.

```
---
driver:
  name: vagrant

provisioner:
  name: chef_zero

platforms:
  - name: ubuntu-12.04
  - name: centos-6.4


suites:
  - name: default
    run_list:
      - recipe[setup::default]
    attributes:

```

The driver is responsible for creating a machine that we'll use to test our cookbooks. The name of the driver is specified as a key-value pair under the driver key.

The provisioner tells Test Kitchen how to run Chef when applying the code in our cookbook to the machine under test. The default and simpliest approach is to use chef_zero. The name of the provisioner is specified as a key-value pair under the provisioner key.

> chef_zero can also be referred to as `chef-client --local-mode` as it will create an in-memory Chef Server.

The platforms is the list of operating systems on which we want to run our code. They are defined here in a list. The initial dash, '-', represents a new specified operating system. Each of the operating systems are key-value values of the name of the of the operating system.

> The list of available OS is dependent on the [bento](https://github.com/chef/bento) project which maintains the list of virtual machines that Test Kitchen will easily download.

The suites is a list of test suites to execute. They are defined here in a list. Each item has a number of key-value pairs of information. The name determines the name of the suite - this is usual in differeniating it from other suites you may define. The `run_list` attribute defines a list of recipes that will be executed when this suite is converged. The attributes define a list of custom attribute that you could define on the node object during execution.

The command `kitchen list` will generate the test matrix for you. The number of instances defined is the result of the number of platforms multiplied by the number of suites. Each instance will have a name generated from both of those fields.

Kitchen has a number of commands that represent the lifecycle of the virtual machine that you are about to administer.

* `kitchen create` is the first state. This will download the box file, if necessary, from the bento repo and store it away for you. It will creates and turns on a virtual machine through the specified driver for you. At this moment you would be able to login to the machine `kitchen login` to look around.

* `kitchen converge` is the second state. This will perform all create does and then it will install chef and perform a `chef-client` run with the run_list specified in the kitchen YAML file. This process will validate that your recipes successfully converges.

* `kitchen verify` is the third state. This will perform all of converge and then it will execute any test kitchen tests that you have defined in the the `test/integration/...` directory. Without tests this step is the same as converge.

* `kitchen destroy` destroys the instance. This is a useful clean up step if you want to converge the recipes on a brand new instance.

* `kitchen test` destroys the instance if present, runs through to the verify stage, and then destroys the instance. This is a useful step if you have defined a number of tests and you want to ensure that when they are run that the instance is clean.

* `kitchen login` allows you to login to the machine. This is useful if you want to perform some exploratory examination of the OS.

There are additional features that can be specified in the kitchen YAML file to make interacting with the specified platform. One particular setting is the ability to forward the ports from the guest system (the virtual machine) to the host system (the workstation).

```
platforms:
  - name: centos65
    driver_config:
      network:
      - ["forwarded_port", {guest: 80, host: 8880}]
      - ["forwarded_port", {guest: 81, host: 8881}]
```