## The Importance of Cookbook Style and Correctness

Welcome to Introduction to Testing with Chef. [Introduce Yourself]. 

In this course we will be introducing you to the importance of Chef cookbook styling and correctness. We have divided up the learning sessions into discrete pieces so that you can learn at your own pace and focus on the areas that you feel you need additional help.

-

In this first session, we will introduce the topic and describe our main objectives. 

-

When thinking about cookbook development it is important that the cookbook works.

How do you verify the cookbooks you wrote work before they reach a production environment?

Let's examine a common development workflow:

New features are added to our existing cookbook. At this point we may perform some ad-hoc verification by eye-balling the code for the right amount of dos and ends. We may walk a few logic branches in our head to ensure that our platforms are correctly configured. Maybe the change is so simple we don't even worry about it. 

We then upload the cookbook to the Chef Server.

-

We login to a test node that we place into a special environment where there are no cookbook restrictions. We run chef-client to synchronize and apply the latest changes in our cookbook. If everything converges without error we poke around the system -- running a few commands to see if ports and services are running, configuration files are in place and our logs don't show any errors. Then we log out and feel pretty comfortable promoting the cookbook to the next integration environment.

-

We may log in to this system and update the cookbook and poke around. We may not. At this point having seen it work on one machine already means that it is likely work in the next environment. Right? We also may not have the time to verify everything again that's what monitoring will help us verify.

-

Generally the write, verify and deploy cycle involves small enough increments of work that can be easily verified. In isolation, cookbooks with small goals on new systems are easy to understand. They are relatively easy to verify as well. However, it is seldom that our cookbooks maintain small goals on new systems, that we continue to focus on that cookbook against other needs of the business, or that we will be the only individual responsible for that cookbook in the long run.

This approach of using ad-hoc verification can lead to hidden technical debt that is obscured as a single individual works through getting the work done. Eventually this debt becomes overwhelming and we are often confronted with the complexity that originally brought us to seek out configuration management in the first place.

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

-

Testing tools including unit and integration testing provide automated ways to ensure that the code we write accomplishes its intended goal. It also helps us understand the intent of our code by providing executable documentation - as tests are able to run in virtualized environments.

During this course you will learn and demonstrate this new cookbook workflow employing the following tools:

* Code Correctness - Foodcritic and Rubocop
* Unit Tests - ChefSpec
* Integration Tests - Test Kitchen with ServerSpec
