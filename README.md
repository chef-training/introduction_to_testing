# Test-Driven Development

## Introductions

The purpose of introductions is two-fold.

First to get to know the audience. Try to assess interests, skill level, and motivation for learning Chef so you can later maintain a consistent level of engagement.

Second to allow for the attendees to become more comfortable with the individuals that also participating in the workshop. Attendees in the workshop often share similar motivations and skill levels so it this is a great opportunity to start to foster a sense of a cohert. Allowing each of the attendees to feel comfortable asking each other questions and assistance.

## [01](scene_01.md) The Importance of Cookbook Style and Correctness

## [02](scene_02.md) Rubocop - Intro

## [03](scene_03.md) Rubocop

## [04](scene_04.md) Rubocop - Exercise

## [05](scene_05.md) Rubocop - Summary

## [06](scene_06.md) Foodcritic - Intro

## [07](scene_07.md) Foodcritic

## [08](scene_08.md) Foodcritic - Exercise

## [09](scene_09.md) Foodcritic - Summary

## [10](scene_10.md) ChefSpec - Intro

## [11](scene_11.md) RSpec - Application

## [12](scene_12.md) RSpec - Language

## [13](scene_13.md) ChefSpec

## [14](scene_14.md) ChefSpec - Exercise

## [15](scene_15.md) ChefSpec - Summary

## Test Kitchen

[Test Kitchen](http://kitchen.ci/) is a test harness tool to execute your configured code on one or more platforms in isolation. A driver plugin architecture is used which lets you run your code on various cloud providers and virtualization technologies.

"It is designed to execute isolated code run in a pristine environment ensuring that no prior state exists."

Kitchen gives us the ability to very quickly define a matrix of recipes we want to converge across multiple platforms.

ChefSpec and Test Kitchen are very different from each other. ChefSpec simulates the collection and execution of the resources defined in the tested recipe. Test Kitchen is actually provisioning instances of a specified OS and converging the recipes and executing the resources against the specified OS. This comes at the cost of speed. ChefSpec is much faster at validating the expectations of your recipe then using Test Kitchen.

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

## ServerSpec

ServerSpec provides a framework for performing tests on the recipes that you write. ServerSpec tests are run when either the `kitchen verify` or `kitchen test` commands are executed. When these tests execute they are making assertions against the virtual OS to ensure that the system meets the defined expectations.

ServerSpec and ChefSpec are similar in their syntax because they share the same roots with RSpec. However, ServerSpec has its own setup requirements and provides its own helper methods and matchers to assist with testing an OS.

These tests often do not take long to execute but the entire process of setting up the OS it is never as fast as ChefSpec. ServerSpec tests are often times refered to as integration tests because they are making assertions about the state of the OS after a recipe or multiple recipes have been executed. These tells ensure that you are writing valid recipes that have a higher chance of working when deployed to a similar OS.

Similar to Chef being a Domain Specific Language (DSL) written on top of Ruby there are many other tools that use this same paradigm. ServerSpec is one such tool. ServerSpec is built on top of a DSL named RSpec. And RSpec is a testing framework that is built on top of Ruby.

> Refer to the RSpec introduction in the ChefSpec portion

Test Kitchen is more particular than RSpec with regard to where the test files are located. The file structure mirrors the following:

```
COOKBOOK_NAME/test/integration/SUITE_NAME/serverspec/RECIPE_NAME_spec.rb
```

* `test/integration` - Test Kitchen will look for tests to run under this directory. It allows you to put unit or other tests in test/unit, spec, acceptance, or wherever without mixing them up. This is configurable, if desired.

* `SUITE_NAME` - This corresponds exactly to the Suite name we set up in the .kitchen.yml file. If we had a suite called "server-only", then you would put tests for the server only suite under

* `serverspec` - This tells Test Kitchen (and Busser) which Busser runner plugin needs to be installed on the remote instance.

* `RECIPE_NAME_spec.rb` - All test files (or specs) are name after the recipe they test and end with the suffix "_spec.rb". A spec missing that will not be found when executing kitchen verify.

There are for more helpers and other things to learn about ServerSpec. The best thing is to read the project [website](http://serverspec.org/) and review the large set of testable [resource types](http://serverspec.org/resource_types.html).

## Workstation Setup

* Install the ChefDK

* Install VirtualBox

* Install Vagrant

* Install Sublime or Atom or Notepad++
