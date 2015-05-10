-

## ChefSpec - Introduction

Welcome to Introduction to Testing with Chef.

-

In this section we will be introducing and using ChefSpec, a testing tool.

-

Remember this is the second tool we may use on our workstation to analyze the code to ensure what we write accomplishes its intended goal.

-

As mentioned before, testing tools ensure code correctness and provide executable documentation. Importantly they increase the speed at which we work by giving us consistent feedback earlier in the cookbook development process. This feedback allows us to make changes to our cookbooks with the confidence that they will accomplish their defined work.

-
5
-

One reason for testing is important is because: Ruby is a dynamically typed programming language. Dynamically typed languages do type checking at run-time as opposed to compile-time. This means that ruby files in our cookbook are not executed, thus not validated, until they are run.

-

Prior to ChefSpec the cookbook development workflows delayed the verification of your cookbook code until it was already released to the Chef Server and pulled down by a node during a chef-client run.

-

Testing a cookbook in this manner gives great feedback on its success at the cost of the time required for each cookbook to be pushed through this workflow.

-
8
-

ChefSpec addresses the speed of execution and removes many of the outside dependencies by allowing you to ...

-
9
-

...define expectations on what your chef-client run would accomplish if run solely in memory.

This example expectation says that when we apply the apache cookbook's server recipe on Ubuntu 14.04 it should install the correct packages. The correct package name is apache2.

-
10
-

This feedback is especially important when you add new features ...

-
11
-

... to your recipes as it will ensure your new changes meet the defined requirements ...

-
12
-

... without breaking the previously defined features.

-
13
-

Similar to Chef being a Domain Specific Language (DSL) written on top of Ruby there are many other tools that use this same paradigm. ChefSpec is one such tool.

-
14
-

ChefSpec is built on top of a DSL named RSpec. And RSpec is a testing framework that is built on top of Ruby. To illustrate how Chefspec works, we will start by looking at the basics of RSpec.

-
15
-

During this section you will learn and demonstrate how to test cookbooks effectively:

* using ChefSpec and RSpec to define tests for recipes
* executing those tests with the rspec command
* and experiencing the process of test-driven development.
