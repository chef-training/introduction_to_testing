## The Importance of Cookbook Style and Correctness

It is important that the cookbooks you develop work! Developing cookbooks in an environment without testing pushes the moment of cookbook validation into our integration environments or into our production environments.

Let us talk about a common, accepted cookbook development workflow:

* Write some cookbook code
* Perform ad-hoc verification
* Deploy the cookbook to an integration environment
* Perform ad-hoc verification
* Deploy the cookbook to production environment
* Perform ad-hoc verification

This workflow is sustainable for developing software if you know for certain that you are never going to change the cookbook source code. In isolation, cookbooks with small goals on new systems are easy to reason what the recipes will install and the effects it will have on the system.

But it is seldom that our cookbooks maintain small goals on new systems. We are often confronted with the complexity that originally brought us to seek out configuration management in the first place.

Every time we make changes to our cookbooks we are introducing risk. We can stop making changes to reduce the risk or we can adopt new practices, like linting and testing, to help us manage that risk.

An updated workflow, one able to manage risk better, involves these tools through the entire cookbook development workflow:

* Write some cookbook code
* Perform linting for code correctness
* Perform unit testing to ensure correctness
* Deploy the cookbook to an integration environment
* Perform integration testing to ensure correctness
* Deploy the cookbook to production environment

This workflow gives us consistent, automated feedback at each stage of cookbook development. By employing linting and testing tools we are able to more confidently deliver code because these tools will help us better understand the cookbooks that you are building.

Linting tools provide automated ways to ensure that the code we write adheres to conventions that ensure code uniformity, portability, and uses best practices. This ensures everyone on the team writes similarly structured source code. Ensuring the uniformity of source code helps set the expectations for fellow project contributors.


Testing tools provide automated ways to ensure that the code we write accomplishes its intended goal. It also helps us understand the intent of our code by providing executable documentation - as tests are able to run in virtualized environments.

During this course you will learn and demonstrate this new cookbook workflow employing the following tools:

* Code Correctness - Foodcritic and Rubocop
* Unit Tests - ChefSpec
* Integration Tests - Test Kitchen with ServerSpec
