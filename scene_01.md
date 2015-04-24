## The Importance of Cookbook Style and Correctness

Welcome to Introduction to Testing with Chef. In this course we will be introducing you to the importance of Chef cookbook styling and correctness. We have divided up the learning sessions into discrete pieces so that you can learn at your own pace and focus on the areas that you feel you need additional help.

-

In this first session, we will introduce the topic and describe our main objectives. 

-

When thinking about cookbook development, of course it is important that the cookbook works. A common accepted development workflow could look like:

* Write some cookbook code
* Perform ad-hoc verification
* Upload the cookbook to the Chef Server

-

* chef-client applies the updated cookbook
* We login to one node to perform ad-hoc verification on the system
* We then promote the cookbook to the next environment

-

* Again chef-client applies the updated cookbook
* And adain we login to the node to perform ad-hoc verification on the sytem
* And, of course, set up monitoring on the deployed services

-

Generally the write, verify and deploy cycle involves small enough increments of work that can be easily verified. In isolation, cookbooks with small goals on new systems are easy to reason what the recipes will install and the effects it will have on the system. This style can lead to hidden technical debt that is obscured as a single individual works through getting the project done. 

It is seldom that our cookbooks maintain small goals on new systems or that we will be the only individual responsible for that cookbook in the long run. We are often confronted with the complexity that originally brought us to seek out configuration management in the first place. The additional cost of refactoring technical debt may lead to individuals throwing out work and restarting the process again leading to a lot of wasted work.

-

Every time we make changes to our cookbooks we are introducing risk. We can stop making changes to reduce the risk or we can adopt new practices, like linting and testing, to help us manage that risk.

-

An updated workflow, one able to manage risk better and describe the implicit assumptions made throughout the development of the cookbook involves these tools through the entire cookbook development workflow:

* Write some cookbook code
* Perform linting for code correctness
* Perform unit testing to ensure correctness
* Deploy the code to a local virtual environment
* Perform automated integration testing against that virtual environment

-

Additionally to local development, we also setup a continuous integration environment that performs the same series of steps of linting and testing the cookbooks.

This workflow gives us consistent, automated feedback at each stage of cookbook development. By employing linting and testing tools we are able to more confidently deliver code because these tools will help us better understand the cookbooks that you are building.

-

Linting tools provide automated ways to ensure that the code we write adheres to conventions that ensure code uniformity, portability, and uses best practices. This ensures everyone on the team writes similarly structured source code. It helps weave the expectations into the development of the code, and encourages collaboration over time. Ensuring the uniformity of source code helps set the expectations for fellow project contributors.

Testing tools including unit and integration testing provide automated ways to ensure that the code we write accomplishes its intended goal. It also helps us understand the intent of our code by providing executable documentation - as tests are able to run in virtualized environments.

During this course you will learn and demonstrate this new cookbook workflow employing the following tools:

* Code Correctness - Foodcritic and Rubocop
* Unit Tests - ChefSpec
* Integration Tests - Test Kitchen with ServerSpec
