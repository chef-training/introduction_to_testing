-
1
-

Welcome to Introduction to Testing with Chef.

In this section we will be introducing Foodcritic, the command-line tool, that allows us to lint our cookbooks.

-
2
-

Foodcritic is part of the standard set of utilities included with the Chef Development Kit.

-
3
-

Foodcritic is a command-line application that analyzes cookbooks to verify that they adhere to a set of conventions.


-
4
-

Foodcritic is executed against a specified path. The path provided is the path to a specific cookbook.

```bash
$ foodcritic cookbooks/iis
```

Here is an example of us executing foodcritic on the iis cookbook found within the cookbooks directory.

-
5
-

While developing cookbooks you will find it easier or necessary to work within the directory of the cookbook. In those instances you may execute the command with a period to represent the current cookbook.

Here we have moved into the the redis cookbook and are asking foodcritic to evaluate it.

-
6
-

Foodcritic will return to you, via standard out or standard error, the results of its evaluation.  This evaluation is against a large body of enabled rules that examine the code from a number of different perspectives that yield a list of violations.

-

By running foodcritic early and often in your development process you gain two advantages:

* identify simple errors early
* improve the review process by eliminating issues that violate the teams conventions

-
8
-

Now you know how to use the tool lets examine the results of a foodcritic execution:

-
9
-

Each line of the output that starts with the sequence FC followed by three numbers is an individual violation.

-
10
-

Each violation can be broken down into the following format.

First is the violation number. FC is short for Foodcritic. The number that follows is the unique, numeric identifier tied to the community convention.

The colon character here acts a separator for our fields.

After the first separator is the message associated with the violation.

Another colon separates the message from the partial path to the file within the cookbook that contains the violation.

The last value after the filepath is the line number within the file that contained the violation. This line number makes it easier to find the issue within the code more quickly when you are trying to address the violations.

-

Lets break down one of these violations as an example:

```
FC003: Check whether you are running with chef server before using server-specific features: ./recipes/ip-logger.rb:1
```

* Rule Number: FC003
* Message: Check whether you are running with chef server before using server-specific features
* Filepath: ./recipes/ip-logger.rb
* Line Number: 1

-

The message describes the steps required for remediation. However, this message is likely only useable to someone that has spent quite a bit of time with Foodcritic. Until then the message is at best a useful hint at how to proceed.

-

The [foodcritic website](http://www.foodcritic.io/) describes each of the violations in more detail. Given a rule number the website provides additional details about the rule, the categories where the rule belongs, the reasoning behind it, example code that violates the rule and example code that provides a remedy to the rule.

-

Foodcritic allows you to execute specific rules. This is defined with the -t flag followed by the food critic violation.

```
$ foodcritic -t FC003 cookbooks/mysql
```

In this example we want to check the mysql cookbook in the cookbooks directory if it violates the food critic rule 003 or FC003.

-

Foodcritic also allows you to run all checks and exclude only specific rules. Again with the -t flag and followed by the food critic violation. Except, you will notice exclusions are specified with a proceeding tilde character.

```
$ foodcritic -t ~FC003 cookbooks/mongodb
```

In this example we want to check the mongodb cookbook in the cookbooks directory if it violates every food critic rule except FC003.

-

Foodcritic violations have tags associated with each of them. The tags can be used in similar way when wanting to filter its execution.

```
$ foodcritic -t services .
$ foodcritic -t ~style .
```

The first example demonstrates running only the violations tagged with "services". The second example demonstrates all violations except the ones tagged with "style".

-

Foodcritic allows you to use multiple copies of the '-t' flag so that you can compose what rules you want to run.

```
$ foodcritic -t services -t ~style -t ~FC002
```

In this example we are asking foodcritic to evaluate the current cookbook against all rules tagged as services that are not also tagged as style conventions or happen to be Foodcritic violation 002 or FC002.

-

When you have defined your complete set of tag options you are able to save them to a file named '.foodcritic'. Foodcritic will read this file upon execution and use those as the default options.

```
$ echo "style,services" > my_cookbook/.foodcritic
```

-

## Foodcritic