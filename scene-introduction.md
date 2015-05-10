## The Importance of Cookbook Style and Correctness

Welcome to Introduction to Testing with Chef. [Introduce Yourself].

In this course we will be introducing you to the importance of Chef cookbook styling and correctness. We have divided up the learning sessions into discrete pieces so that you can learn at your own pace and focus on the areas that you feel you need additional help.

-

In this first session, we will explain the value of testing your cookbooks and then layout the main objectives of this course.

-

When thinking about cookbook development it is important that the cookbook works.

How do you verify your cookbooks before you send them to a production environment?

Let's examine a common development workflow:

Imagine we add new features to an existing cookbook. Before uploading it to the Chef Server we perform an ad-hoc examination of the recipes -- eye-balling the code for matching braces, parenthesis, and dos and ends. If we wrote code with logic, we may walk a few logic branches in our head or on a white board to ensure that our platforms are correctly configured. Or perhaps the change is so simple we don't even worry about it -- like a small change to a path inside a string or the addition of a new node attribute.

After enough examination we feel comfortable to upload the cookbook to the Chef Server.

-

We login to a test node that we patiently bootstrap into a union environment. This is an environment we setup with no cookbook restrictions allowing chef-client to synchronize and apply the latest changes in the recently completed cookbook. Here we see if we got the right package names, spelled all our cookbook attributes correctly, and didn't typo any of the configuration in the templates. If everything converges without error we poke around the system -- running a few commands to see if ports are blocked, services are running, and the logs don't show any errors.

Logging out of the working system we feel pretty comfortable promoting the cookbook to the rehearsal environment.

-

Here in this new environment we may log into another system. Manually perform a chef-client run and then poke around again if everything works. We also may not. It was such a small change and everything worked on the other machine -- so it's likely to work here. Right?

Instead of running through a series of ad-hoc verifications again on a new system in this environment - we start to think of the backlog of things that need to get done.

-

In isolation, cookbooks with small goals on new systems are easy to understand. They are relatively easy to verify as well. However, it is seldom that our cookbooks maintain small goals or will only be deployed to new systems. As time goes on and cookbook development continues it's also hard maintaining extensive knowledge of the cookbook in the face of many other competing business needs.

This approach of using ad-hoc verification can lead to hidden technical debt. Eventually this debt becomes overwhelming and we are often confronted with the complexity that originally brought us to seek out configuration management in the first place.

-

Every time we make changes to our cookbooks we are introducing risk. We can stop making changes to reduce the risk OR we can adopt new practices, like linting and testing, to help us manage that risk.

-

Lets talk about an updated workflow, one able to manage risk better. One that involves these tools throughout the entire cookbook development workflow:

It all starts with adding new features to the cookbook. But now before we upload the cookbook to the Chef Server we employ linting tool to automatically verify the syntax and structure of our cookbook. We use unit testing to verify that the cookbook performs its intended operations catching typos and exhaustively exploring all of our logic branches for each platform.

On top of that we use additional tools that allow us to deploy and test our cookbook code against virtualized environments that mirror our production systems.

All of these examinations are done through automation that we've defined and gives us more confidence when we commit the changes to our cookbook to source control.

-

We also setup a continuous integration environment that performs the same series of steps of linting and testing the cookbooks. To ensure an impartial source examines the cookbooks to prevent issues of cookbooks only working on "my machine".

This workflow gives us consistent, automated feedback at each stage of cookbook development by monitoring every commit. By employing these tools in an continuous integration environment your team is able to see immediately where and when bugs happen instead of weeks later when it comes time to deploy the updated cookbook.

Alright, so what are linting tools? What are testing tools?

-

Linting tools provide automated ways to ensure that the code we write adheres to conventions that ensure code uniformity, portability, and uses best practices. This ensures everyone on the team writes similarly structured source code. It helps weave the expectations into the development of the code, and encourages collaboration over time. Ensuring the uniformity of source code helps set the expectations for fellow project contributors.

-

Testing tools provide automated ways to ensure that the code we write accomplishes its intended goal. It also helps us understand the intent of our code by providing executable documentation. We add new cookbook features and write tests to preserve this functionality so that we - or anyone else on the team - can return to the cookbook tomorrow tomorrow to make additional changes without the fear of accidently breaking something.

-

During this course you will learn and demonstrate this new cookbook workflow employing the following tools:

* Code Correctness with Foodcritic and Rubocop
* Unit Tests and testing with ChefSpec
* And Integration Tests through Test Kitchen and ServerSpec
