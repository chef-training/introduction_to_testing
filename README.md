# Test-Driven Development

## Introductions

The purpose of introductions is two-fold.

First to get to know the audience. Try to assess interests, skill level, and motivation for learning Chef so you can later maintain a consistent level of engagement.

Second to allow for the attendees to become more comfortable with the individuals that also participating in the workshop. Attendees in the workshop often share similar motivations and skill levels so it this is a great opportunity to start to foster a sense of a cohert. Allowing each of the attendees to feel comfortable asking each other questions and assistance.

## Course Objectives and Style

The attendee will leave this workshop with a basic understanding Chef's core testing and linting tools and ensure they have a workstation with all necessary tools installed.

The course is a mixture of lecture, exercise, and conversation. Each section the instructor will walk the attendees through a technical challenge by providing them the explanations, demonstration, and source code. The instructor will also present exercises, without walk-through, that the attendees will accomplish. The instructor will lead the attendees through various discussions that reinforce good practices and understanding the underlying technology.

## Cookbook Style and Correctness

It is important to validate that our cookbooks work. In an test-free environment feedback loop is rather long and often times pushes validation out to integration environments or worse production.

### Old Cookbook Workflow

* Write some cookbook code
* Perform ad-hoc verification
* Deploy the cookbook to an integration environment
* Perform ad-hoc verification
* Deploy the cookbook to production environment
* Perform ad-hoc verification

This workflow is sustainable for developing software if know for certain that you are never going to change the cookbook source code. In isolation, cookbooks with small goals on new systems are easy to reason what the recipes will install and the effects it will have on the system.

> This is often the crux of demonstrating testing tools and practices. The code or systems are reasonable. Asking you to write tests for a completely reasonable system feels often like wearing a belt and suspenders -- extra work without a lot of benefit. The benefit is often not felt until the system starts to become unreasonable and adding tests then often feels too difficult.

But it seldom that our cookbooks maintain small goals on new systems. We are often confronted with the complexity that originally brought us to use configuration management in the beginning.

Testing and a testing practice will help you better understand the system that you are building. It will make the code that you write more realible. And when something breaks (which it still will break) the errors will shift from your lack understanding of your objective and instead be related to the environment in which the code integrates with other code.

Testing is about understanding and verifying your intent of the code that you wrote. The tests will provide an automated way of ensure you don't lose sight of your goals. This becomes important as you invite new members to your team to assist you with development. Tests will help you understand code written by other team members. Tests will even help you when you return to code you have not seen in months or years.

In the same way that writing Chef recipes to capture the state of your infrastructure. Testing and linting tools will assist in ensuring that you have defined the correct infrastructure.

### New Cookbook Workflow

* Write some cookbook code
* Perform checks for code correctness
* Perform checks to ensure correctness with unit tests
* Deploy the cookbook to an integration environment
* Perform checks to ensure correctness with integration tests
* Deploy the cookbook to production environment

During this workshop the attendee will demonstrate this new cookbook workflow through the use of following tools:

* Code Correctness - Foodcritic and Rubocop
* Unit Tests - ChefSpec
* Integration Tests - Test Kitchen with ServerSpec

## Rubocop

The majority of the time you are working with Chef you are writing ruby code. Most every file within a cookbook is a ruby file - the metadata, recipes, attributes and any source that you have defined in the libraries folder.

Rubocop is a first tool to assist you with writing cleaner cookbooks. It is a style and linting tool that anaylyzes the ruby code you've defined against a number of 'cops'. Each of these cops examines the code with a different perspective and produces a list of of warnings, deviations from conventions, potentional errors, and fatal errors.

* Enforces style conventions
* Enforces best practices within Ruby
* Evaluate the code against metrics (e.g. line length, function size)

Style and linting tools such as provide automated ways to ensure that everyone on the team writes similarly structured source code. Ensuring the uniformity of source code helps set the expectations for new and old committers on the project. They are more easily able to reason about the overall goal of the source code if they are spending less time attempting to understand what is written. This is useful as well when you want to bring more individuals into the code review process.

This tool is built for Ruby developers and the conventions that it attempts to enforce are those defined by the community of Ruby developers that work with the project.  As cookbook authors we do not always have the same objectives as Ruby developers but there is enough of an overlap that the tool is beneficial.

Rubocop allows you to turn on, turn off, and create your own cops to assist you with enforcing the standards defined by your team.

Rubocop is a command-line application that is executed against a specified path (normally a cookbook).

```bash
$ rubocop webcookbook
```

While developing cookbooks with tests and using linting tools it is often the case you will find yourself executing this command inside the directory of the cookbook.

```bash
$ rubocop .
```

Rubocop will return to you, via standard out, the results of the evaluation. This is a list of all the warnings, violations of conventions, code needing refactoring, warnings, errors, and fatal issues.

```
Inspecting 8 files
CWCWCCCC

Offences:

cookbooks/apache/attributes/default.rb:1:1: C: Missing utf-8 encoding comment.
default["apache"]["indexfile"] = "index1.html"
^
cookbooks/apache/attributes/default.rb:1:9: C: Prefer single-quoted strings when you don't need string interpolation or special symbols.
default["apache"]["indexfile"] = "index1.html"
        ^^^^^^^^
cookbooks/apache/attributes/default.rb:1:19: C: Prefer single-quoted strings when you don't need string interpolation or special symbols.
default["apache"]["indexfile"] = "index1.html"
                  ^^^^^^^^^^^
```

The results start with a summary describing the number of ruby files that found and examined. Next it displays the results of each of those files as a series of symbols or letters.

* `.` means that the file contains no issues
* `C` means a issue with convention
* `W` means a warning
* `E` means an error
* `F` means an fatal error

After the summary each of the offences are displayed in the following format:

```
FILENAME:LINE_NUMBER:CHARACTER_NUMBER: TYPE_OF_ERROR: MESSAGE
SOURCE CODE
^ (<- that carrot indicates the location within the source where the issue was detected)
```

Rubocop is executed with the following default set of rules: https://github.com/bbatsov/rubocop/blob/master/config/enabled.yml. There are a number of rules that are disabled by default https://github.com/bbatsov/rubocop/blob/master/config/disabled.yml.

Rubocop feedback can sometimes feel not very helpful because it usually focuses on spaces, parenthesis, and other minutia within the language. Chef itself generates source code for you within cookbooks that will generate various issues to fix.

To assist you with maximizing the effectiveness of the tool you are able to configure the various cops to ensure that the tool focuses on the conventions that you feel are important. You also have the ability to disable the cops that you never want executed.

We define these rules and exceptions on a per cookbook basis, though they are often the same across all our cookbooks. Within a particular cookbook you are allowed to define a file named `.rubocop.yml` file. In this file you specify the name of the rule. You specify overriding parameters if necessary. You specify whether the rule should be enabled.

This is an example of a common `.rubocop.yml` for Chef cookbooks.

```
AlignParameters:
  Enabled: false

Encoding:
  Enabled: false

LineLength:
  Max: 200

StringLiterals:
  Enabled: false
```

When Rubocop is executed the next time it will evaluate the overrides within this file and ensures to enable, disable, and/or augument the rules that are defined.


## Foodcritic

Foodcritic is the second tool to assist you with writing cleaner cookbooks. It is a lint-like tool because it analyzes the code specified and returns a list of violations.

* Enforces specific behaviors
* Detects portability of your recipes
* Detects possible run-time failures
* Detects common anti-patterns
* Faster than a chef-client run as it performs static analysis

It is a static analysis tool which means that the code is not executed in the traditional sense (e.g. chef-client). The code itself is read, broken down, and then compared to the rules that ship with foodcritic. These rules are the guidelines that the community has decided are common across all cookbooks.

> Foodcritic is not going to validate the intention of your cookbook's recipes. It will evaluate the structure of that code and ensure that it meets the community guidelines.

These rules are not requirements. Employing these rules will help you, your team mates, open-source contributors, and future maintainers to more easily reason about the code because of the shared, enforced standard.

Foodcritic can provide feedback for each of your cookbooks against:

* a particular rule
* a classification of rules,
* all rules with the exclusion of some rules

It is command-line application that is executed against a single cookbook path.

```bash
$ foodcritic web
```

While developing cookbooks with tests and using linting tools it is often the case you will find yourself executing this command inside the directory of the cookbook.

```bash
$ foodcritic .
```

Foodcritic will return to you, via standard out, the results of its evaluation. This is a list of all the foodcritic violations.

```
FC003: Check whether you are running with chef server before using server-specific features: ./recipes/ip-logger.rb:1
FC008: Generated cookbook metadata needs updating: ./metadata.rb:2
FC008: Generated cookbook metadata needs updating: ./metadata.rb:3
```

Each line of the results adheres to the following format:

```
RULENUMBER: MESSAGE: FILEPATH:LINENUMBER
```

Selecting one of the violations in our results we see the following:

```
FC003: Check whether you are running with chef server before using server-specific features: ./recipes/ip-logger.rb:1
```

* Rule Number: FC003
* Message: Check whether you are running with chef server before using server-specific features
* Filepath: ./recipes/ip-logger.rb
* Line Number: 1

The message desribes the steps to remediation. However, this message is likely only useable to someone that has spent quite a bit of time with Foodcritic. Until then the message is at best a useful hint at how to proceed.

The [website](http://www.foodcritic.io/) itself describes each of the violations in more detail. Given a rule number the website provides additional details about the rule, the categories where the rule belongs, the reasoning behind it, example code that violates the rule and example code that adheres to the rule.

In our previous example, [FC003](http://www.foodcritic.io/#FC003), it explains the issue in more detail.

When you violate a rule it does not automatically mean that the offending code needs to be changed. It is important to understand the rule and its intentions.

For instance, rule [FC003](http://www.foodcritic.io/#FC003), describes a scenario where the recipe uses the method `search` to retreive data from the Chef Server. It suggests that this cookbook will raise an error when running in a situation without a Chef Server. To maintain portability it suggests you check if you happen to be in "solo" mode (e.g. Chef Server is absent).

You may not want to adopt this rule within your organization because there are no imagineable situations where you will be executing this cookbook without a Chef Server. In these situations it is possible to execute foodcritic in a manner which excludes a rule.

```bash
$ foodcritic . --tags ~FC003
```

> Be aware that your rules you reject for your cookbooks may make it hard to use your cookbook in scenarios that you did not imagine. This is important to remember if you decide to share your cookbook with another team or the open-source community.

You can also define your own rules or import them. An example of that are the rules defined by [Etsy](https://github.com/etsy/foodcritic-rules).

### [Foodcritic](http://www.foodcritic.io/)

> Foodcritic is a helpful lint tool you can use to check your Chef cookbooks for common problems.
>
> It comes with 47 built-in rules that identify problems ranging from simple style inconsistencies to difficult to diagnose issues that will hurt in production.
>
> Various nice people in the Chef community have also written extra rules for foodcritic that you can install and run. Or write your own!

### [Lint](http://en.wikipedia.org/wiki/Lint_%28software%29)

In computer programming, lint was the name originally given to a particular program that flagged some suspicious and non-portable constructs (likely to be bugs) in C language source code. The term is now applied generically to tools that flag suspicious usage in software written in any computer language. The term lint-like behavior is sometimes applied to the process of flagging suspicious language usage. Lint-like tools generally perform static analysis of source code.

## Test Kitchen

## ServerSpec

## ChefSpec

## Workstation Setup
