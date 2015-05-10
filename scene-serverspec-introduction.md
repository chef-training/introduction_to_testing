## ServerSpec - Intro

ServerSpec provides a framework for performing tests on the recipes that you write. ServerSpec tests are run when either the `kitchen verify` or `kitchen test` commands are executed. When these tests execute they are making assertions against the virtual OS to ensure that the system meets the defined expectations.

Similar to Chef being a Domain Specific Language (DSL) written on top of Ruby there are many other tools that use this same paradigm. ServerSpec is one such tool. ServerSpec is built on top of a DSL named RSpec. And RSpec is a testing framework that is built on top of Ruby.

ServerSpec and ChefSpec are similar in their syntax because they share the same roots with RSpec. However, ServerSpec has its own setup requirements and provides its own helper methods and matchers to assist with testing an OS.

ChefSpec and Test Kitchen are very different from each other. ChefSpec simulates the collection and execution of the resources defined in the tested recipe. Test Kitchen is actually provisioning instances of a specified OS and converging the recipes and executing the resources against the specified OS. This comes at the cost of speed. ChefSpec is much faster at validating the expectations of your recipe then using Test Kitchen.

These tests often do not take long to execute but the entire process of setting up the OS it is never as fast as ChefSpec. ServerSpec tests are often times refered to as integration tests because they are making assertions about the state of the OS after a recipe or multiple recipes have been executed. These tells ensure that you are writing valid recipes that have a higher chance of working when deployed to a similar OS.
