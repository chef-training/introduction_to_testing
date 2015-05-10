## Foodcritic - Introduction

Welcome to Introduction to Testing with Chef.

-
2
-

In this section we will be introducing and using Foodcritic, a linting tool.

-
3
-

Remember this is the first tool we may use on our workstation to analyze our code to verify the syntax and structure of our cookbooks.

-
4
-

As mentioned before, linting tools provide atutomated ways to ensure that the code we write adheres to conventions that ensure code unformity, portability, and uses best practices.

-
5
-

This ensures everyone on the team writes similarly structured source code. It helps weave the expectations into the development of the code, and encourages collaboration over time. Ensuring the uniformity of source code helps set the expectations for fellow project contributors.

-
6
-

When it comes to composing cookbook code you often times have many ways to solve your problems. There are also many ways to create problems.

-
7
-

Foodcritic is built for Chef developers by Chef developers to ensure all their cookbooks obey a commmunity defined set of conventions.

-
8
-

There are over 50 rules defined to address conventions for a variety of areas of a cookbook.

An example of one is if you defined a resource with an incorrectly defined action or attribute.

-
9
-

As in this instance where the execute resource only has two actions :run and :nothing but we have defined the action as :rum. A potential error!

-
10
-

Depending on your background, you may find that some conventions unhelpful. Foodcritic is configurable allowing you to modify which conventions are checked and which to ignore.

You can also create and share additional conventions that you create based on your organization's goals.

-
11
-

During this section you will learn and demonstrate how to use Foodcritic effectively:

* using Foodcritic to discover violations within cookbooks
* applying solutions to identified violations
* and using Foodcritic configuration to make the tool more effective and consistent in identifying violations
