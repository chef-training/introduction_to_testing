## ChefSpec - Introduction

In this section we will be introducing and using ChefSpec, a testing tool. Testing provides us a way to define a set of specification for our recipes that we can easily execute from our workstation. This feedback allows us to make changes to our cookbooks ensuring we are able to upload our cookbooks with confidence that our recipes accomplish their defined work.

Ruby is a dynamically typed programming language. Dynamically typed languages do type checking at run-time as opposed to compile-time. This means that ruby files in our cookbook are not executed, thus not validated, until they are run.

Prior to ChefSpec the cookbook development workflows delayed the verification of your cookbook code until it was already released to the Chef Server and pulled down by a node during a chef-client run. Running a cookbook in this manner gives great feedback on its success at the cost of the time required for each cookbook to be pushed through this workflow and the complications of other dependencies.

ChefSpec addresses the speed of execution and removes many of the outside dependencies by allowing you to define expectations on what your chef-client run would accomplish if run solely in memory. Fast feedback allows you to build cookbooks confidently because it gives you a way to exercise these cookbooks in a virtualized way. This feedback is especially important when you add functionality or refactor your recipes as it will ensure your new changes don't break the previous functionality you wrote before and depend on working tomorrow.

Similar to Chef being a Domain Specific Language (DSL) written on top of Ruby there are many other tools that use this same paradigm. ChefSpec is one such tool. ChefSpec is built on top of a DSL named RSpec. And RSpec is a testing framework that is built on top of Ruby.