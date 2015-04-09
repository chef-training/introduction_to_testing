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

This workflow is sustainable for developing software if you know for certain that you are never going to change the cookbook source code. In isolation, cookbooks with small goals on new systems are easy to reason what the recipes will install and the effects it will have on the system.

> This is often the crux of demonstrating testing tools and practices. The code or systems are reasonable. Asking you to write tests for a completely reasonable system feels often like wearing a belt and suspenders -- extra work without a lot of benefit. The benefit is often not felt until the system starts to become unreasonable and adding tests then often feels too difficult.

But it is seldom that our cookbooks maintain small goals on new systems. We are often confronted with the complexity that originally brought us to seek out configuration management in the beginning.

Testing and a testing practice will help you better understand the system that you are building. It will make the code that you write more realible. And when something breaks (which it still will break) the errors will shift from your lack understanding of your objective and instead be related to the environment in which the code integrates with other code.

Testing is about understanding and verifying the intent of the code that you wrote. The tests will provide an automated way of ensure you don't lose sight of your goals. This becomes important as you invite new members to your team to assist you with development. Tests will help you understand code written by other team members. Tests will even help you when you return to code you have not seen in months or years.

In the same way that writing Chef recipes capture the state of your infrastructure. Testing and linting tools will assist in ensuring that you have defined the correct infrastructure.

### New Cookbook Workflow

* Write some cookbook code
* Perform checks for code correctness
* Perform checks to ensure correctness with unit tests
* Deploy the cookbook to an integration environment
* Perform checks to ensure correctness with integration tests
* Deploy the cookbook to production environment

During this workshop we will demonstrate this new cookbook workflow through the use of following tools:

* Code Correctness - Foodcritic and Rubocop
* Unit Tests - ChefSpec
* Integration Tests - Test Kitchen with ServerSpec

## Rubocop

The majority of the time you are working with Chef you are writing ruby code. Most every file within a cookbook is a ruby file - the metadata, recipes, attributes and any source that you have defined in the libraries folder.

Rubocop is a first tool to assist you with writing cleaner cookbooks. It is a style and linting tool that anaylyzes the ruby code you've defined against a number of 'cops'. Each of these cops examines the code with a different perspective and produces a list of of warnings, deviations from conventions, potentional errors, and fatal errors.

* Enforces style conventions
* Enforces best practices within Ruby
* Evaluate the code against metrics (e.g. line length, function size)

Style and linting tools provide automated ways to ensure that everyone on the team writes similarly structured source code. Ensuring the uniformity of source code helps set the expectations for fellow project contributors.

New and existing contributors are more easily able to reason about the overall goal of the source code if they are spending less time attempting to understand what is written. This is useful when you want to bring more individuals into the code review process as you will be able to spend more time on the objective of the source and less time on the look of the code.

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

Rubocop will return to you, via standard out, the results of the evaluation. This is a list of all the warnings, violations of conventions, code needing refactoring, errors, and fatal issues.

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

Rubocop feedback can sometimes feel not very helpful. This is because the majority of the raised issues will be related to style and formatting (e.g. spaces, parenthesis, and other minutia within the language).

> Even some of the files (e.g. metadata.rb) the Chef tools generate will immediately raise issues when reviewed by Rubocop.

Rubocop is executed with the following default set of enabled [rules](https://github.com/bbatsov/rubocop/blob/master/config/enabled.yml). There are also a number of [disabled rules](https://github.com/bbatsov/rubocop/blob/master/config/disabled.yml).

To assist you with maximizing the effectiveness of the tool you are able to configure the various cops to ensure that the tool focuses on the conventions that you feel are important. This may mean completely disabling a cop that you feel is not important to your cookbook and not a standard you would like to adopt as a team.

Eech cookbook you create can have its own custom set of enabled, disabled, and configured cops.

> While you are able to define different rubocop rules per cookbook often times all your cookbooks within your organization will share the same rules.

Within a particular cookbook you define a YAML (YAML Ain't Markup Language) file named `.rubocop.yml`. YAML files are white-space significant formatted file, like python.

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

Within the example file we disabled a number of cops that are enabled by default. We also overrode the value of the LineLength to allow for us to write source code lines up to 200 characters without raising an error (defaults to 80).

Each entry within the file adheres to the following format:

```
NAME_OF_COP:
  Enabled : (true or false)
  PARAMETER_KEY: PARAMETER_VALUE
```

* NAME_OF_COP - The name of the cop that is being described.

* Enabled: (true or false) - This is where you can set the cop enabled or disabled. The default enabled setting is set to true for cops defined in the default [enabled](https://github.com/bbatsov/rubocop/blob/master/config/enabled.yml) file and false for cops defined in the default [disabled](https://github.com/bbatsov/rubocop/blob/master/config/disabled.yml) file.

* PARAMETER_KEY: PARAMETER_VALUE - define the parameters specific to the cop that is being applied. These values are dependent on the cops and not all cops have additional parameters to set.

When Rubocop is executed against a cookbook the default enabled and disabled cops are loaded and then the Rubocop YAML file is evaluated. This allows you to customize the cops that are defined.

There may be more "issues" that Rubocop is alerting to you that you would like to ignore initially or completely. Rubocop allows you to capture the current state of all the offences and write them to a file that you can accept as your core rules.

```bash
$ rubocop --auto-gen-config
```

This generates a file named `.rubocop_todo.yml`. You can simply rename this file as `.rubocop.yml` if you want to accept it as the rubocop standard for the cookbook.

Often times this file is generated as a list of items you want to review one-at-a-time. This is why the file is named `.rubocop_todo.yml`. Instead of renaming this file you can define a `.rubocop.yml` that includes this file as well.

```yaml
inherit_from: .rubocop_todo.yml
```

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
$ foodcritic cookbooks/web
```

While developing cookbooks with tests and using linting tools it is often the case you will find yourself executing this command inside the directory of the cookbook.

```bash
$ cd cookbooks/web
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

The [website](http://www.foodcritic.io/) itself describes each of the violations in more detail. Given a rule number the website provides additional details about the rule, the categories where the rule belongs, the reasoning behind it, example code that violates the rule and example code that provides a remedy to the rule.

When you violate a rule it does not automatically mean that the offending code needs to be changed. It is important to understand the rule and its intentions.

For instance, rule [FC003](http://www.foodcritic.io/#FC003), describes a scenario where the recipe uses the method `search` to retreive data from the Chef Server. It suggests that this cookbook will raise an error when running in a situation without a Chef Server. To maintain portability it suggests you check if you happen to be in "solo" mode (e.g. Chef Server is absent).

You may not want to adopt this rule within your organization because there are no imagineable situations where you will be executing this cookbook without a Chef Server. In these situations it is possible to execute foodcritic in a manner which excludes a rule.

```bash
$ foodcritic . --tags ~FC003
```

> Be aware that your rules you reject for your cookbooks may make it hard to use your cookbook in scenarios that you did not imagine. This is important to remember if you decide to share your cookbook with another team or the open-source community.

You can also define your own rules or import them. An example of that are the rules defined by [Etsy](https://github.com/etsy/foodcritic-rules).

### [Lint](http://en.wikipedia.org/wiki/Lint_%28software%29)

In computer programming, lint was the name originally given to a particular program that flagged some suspicious and non-portable constructs (likely to be bugs) in C language source code. The term is now applied generically to tools that flag suspicious usage in software written in any computer language. The term lint-like behavior is sometimes applied to the process of flagging suspicious language usage. Lint-like tools generally perform static analysis of source code.

## ChefSpec

ChefSpec provides a framework for performing tests on the recipes that you write. These tests execute fast and give you feedback fast when there are problems that need to be fixed in your cookbook. Fast feedback allows you to build cookbooks confidently because it gives you a way to exercise these cookbooks in a virtualized way. This feedback is especially important when you add functionality or refactor your recipes as it will ensure your new changes don't break the previous functionality you wrote before and depend on working tomorrow.

Similar to Chef being a Domain Specific Language (DSL) written on top of Ruby there are many other tools that use this same paradigm. ChefSpec is one such tool. ChefSpec is built on top of a DSL named RSpec. And RSpec is a testing framework that is built on top of Ruby.

> RSpec is a popular testing framework that was born out of the Behavior Driven Development (BDD) approach that became popular with Agile and everything else that has turned software development on its head.

RSpec, as a BDD framework, attempts to use more natural language to help you quickly describe scenarios in which systems are being tested.

```ruby
describe "Math" do

  context "when adding 1 + 1" do
    it "equals 2" do
      expect(1 + 1).to eq(2)
    end
  end

end
```

This is a test, or specification, that is written in pure RSpec. RSpec provides several important keywords that allow you describe the scenarios you want to examine in your tests.

* `describe` - allows us to describe the subject or the title of the thing that we are testing. For our test the name 'Math' is not important other than to give us a parent heading to give us guidance on the examples we are writing.

* `context` - this is an alias of the `describe`. It cannot however be used as a top-level item. The reason for nesting a `context` or `describe` is to further refine your tests by creating a group of specific tests for a given scenario. The rule to use when considering if you should create is a context: If you are thinking to yourself ... when I am on this platform ... and when I'm on that platform ... -- then consider using a context.


```ruby
describe "cookbook_name::recipe_name" do

  context "when on Debian" do
    it "installs the correct packages"
  end

  context "when on Ubuntu" do
    it "installs the correct packages"
  end

  context "when on Windows" do
    it "installs the correct packages"
  end
end
```

* `it` - defines the examples or tests that we are writing. Inside the body of the `it` block is where we setup the state of our test, take action by executing methods on our objects, and then making assertions to verify that the system is in the state we believe it to be in.

```ruby

it "equals 2" do
  # Assign
  a = 1
  b = 1
  # Act
  sum = a + b
  # Assert
  expect(sum).to eq(2)
end

it "equals 2" do
  expect(1 + 1).to eq(2)
end
```

* `let` - defines a helper method that is executed the first time it is invoked and then the value is stored for all subsequent requests within the same test. You may think about this similar to a cache-miss, load and then a cache-hit. The value is reset for each example to ensure tests are run in isolation.

ChefSpec brings the knowledge of Chef to RSpec specifically it:

* Allows you to simulate a chef-client run against a Server or in a Solo environment
* Allows you to reference the recipe name in the parent `descrbed` with the variable `described_recipe`
* Allows you to setup expectations against Chef core resources

```ruby
require 'chefspec'

describe 'apache::default' do
  let(:chef_run) do
    ChefSpec::ServerRunner.new.converge(described_recipe)
  end

  it 'installs apache2' do
    expect(chef_run).to install_package('httpd')
  end
end
```

Here we define a simulated Server Runner named `ChefSpec::ServerRunner` and we ask it to converage the default recipe for the apache cookbook. We return that value any time we request the `chef_run` within our examples.

The `described_recipe` is in reference to the recipe name specified in the topmost describe string. In this instance it is the 'apache::default'. This helper saves you from repeating the recipe name throughout the test file.

Within the example 'installs apache2' you see that we have defined an expectation on the `chef_run` to install the package named 'httpd'. This is one of the many expectations that ChefSpec provides.

The format of these expectations:

```
expect(chef_run).to ACTION-NAME_RESOURCE-TYPE(RESOURCE-NAME)
```

You will define tests per cookbook and store them in a directory named `spec/unit`. A test file is a ruby file that ends with `_spec.rb`. You define a test file for each of the recipes that you have defined. The name of the recipe, e.g. `recipes/default.rb`, would have a matching test file named `spec/unit/default_spec.rb`.

RSpec is a command-line application that runs tests files. You need to execute RSpec within your cookbook. It will raise an error if you attempt to run the application from any other directory but the cookbook directory.

```bash
$ cd cookbooks/COOKBOOK_NAME
$ rspec
```

> This will likely represent itself as an error related to issues with `require 'spec_helper'`. It is a Ruby load path issue. When RSpec runs it adds a few directories onto the load path (i.e. 'lib', 'spec'). So when you are in the directory of a cookbook all the files within that 'spec' directory can be required without any additional paths (i.e. `require 'spec_helper'`).

RSpec has many different forms of output. The default is called the 'progress' format and the output looks and reads similar to rubocop and foodcritic.

* '.' - represents a passing example
* 'F' - represents a failing example

Upon completion of the run if there are any failures they will be displayed. Failures are numbered and use the language from the test file as the description of the failure.

After the description the error will be displayed to you. It often times repeats the expectation that you have defined. Then it will show you the expected value and the got (the value that came back from the execution) value.

The position of the failure will be displayed by outputting the file name followed by the line number.

RSpec also represents the failures again as commands you could execute if you wanted to run a specific test again. To do this you need to specify the file name and a line number. The line number is any line within the defined example ('it' block).

```
$ rspec ./spec/unit/default_spec.rb:29
```

RSpec has a different format outputter that displays more text. It's called the 'documentation' formatter and you can enable it from the command line with `-f documentation` or the shorter format `-f d`.

```bash
$ rspec -f documentation
$ rspec -f d
```

You can also enable color in the output with the flag `--color` or `-c`. This will display passing examples in green and failing examples in red.

ChefSpec also provides the ability to provide coverage. Coverage is a measure of all the resources your examples are able to touch through testing divided by the the total number of resources that could be tested. This results in a fractional value that is represented as a percentage.

To enable coverage we need to ask for ChefSpec::Coverage to yield the report when the test run is complete. This is done by adding the following source to each of your test files.

```ruby
require 'chefspec'
at_exit { ChefSpec::Coverage.report! }
```

Every test file would need to repeat this content so instead we will write this into a helper file - `spec/spec_helper.rb`. Now in each of our test files we instead require the spec_helper.rb file.

```ruby
require 'spec_helper'
```

There are for more helpers and other things to learn about ChefSpec. The best thing is to read the project [README](https://github.com/sethvargo/chefspec) and review the large set of [examples](https://github.com/sethvargo/chefspec/tree/master/examples).

## Test Kitchen

[Test Kitchen](http://kitchen.ci/) is a test harness tool to execute your configured code on one or more platforms in isolation. A driver plugin architecture is used which lets you run your code on various cloud providers and virtualization technologies.

"It is designed to execute isolated code run in a pristine environment ensuring that no prior state exists."

Kitchen gives us the ability to very quickly define a matrix of recipes we want to converge across multiple platforms.

ChefSpec and Test Kitchen are very different from each other. ChefSpec simulates the collection and execution of the resources defined in the tested recipe. Test Kitchen is actually provisioning instances of a specified OS and converging the recipes and executing the resources against the specified OS. This comes at the cost of speed. ChefSpec is much faster at validating the expectations of your recipe then using Test Kitchen.

This is a great tool for local development as it allows you to preview the effects of the recipes you have written in an environment that is easy to commision and then decomission.

Test Kitchen is able to work with a large number of drivers that allow you to provision local virtual machines or provision a machine from a cloud provider. With local development we often use drivers like vagrant or docker.

Test Kitchen provides a command-line application which allows you to control the test matrix that you have defined in your `.kitchen.yml`.

```bash
$ kitchen --help
```

Each cookbook is now commonly generated with a `.kitchen.yml` file. If the cookbook is missing this file you can generate it with a kitchen init command

```bash
$ kitchen help init
$ kitchen init
```

The `.kitchen.yml` file is a YAML file. It is white-space significant formatted file, like python. The kitchen YAML, as its often called, allows for us to configure the way that test kitchen will operate as well as define our test matrix.

```
---
driver:
  name: vagrant

provisioner:
  name: chef_zero

platforms:
  - name: ubuntu-12.04
  - name: centos-6.4


suites:
  - name: default
    run_list:
      - recipe[setup::default]
    attributes:

```

The driver is responsible for creating a machine that we'll use to test our cookbooks. The name of the driver is specified as a key-value pair under the driver key.

The provisioner tells Test Kitchen how to run Chef when applying the code in our cookbook to the machine under test. The default and simpliest approach is to use chef_zero. The name of the provisioner is specified as a key-value pair under the provisioner key.

> chef_zero can also be referred to as `chef-client --local-mode` as it will create an in-memory Chef Server.

The platforms is the list of operating systems on which we want to run our code. They are defined here in a list. The initial dash, '-', represents a new specified operating system. Each of the operating systems are key-value values of the name of the of the operating system.

> The list of available OS is dependent on the [bento](https://github.com/chef/bento) project which maintains the list of virtual machines that Test Kitchen will easily download.

The suites is a list of test suites to execute. They are defined here in a list. Each item has a number of key-value pairs of information. The name determines the name of the suite - this is usual in differeniating it from other suites you may define. The `run_list` attribute defines a list of recipes that will be executed when this suite is converged. The attributes define a list of custom attribute that you could define on the node object during execution.

The command `kitchen list` will generate the test matrix for you. The number of instances defined is the result of the number of platforms multiplied by the number of suites. Each instance will have a name generated from both of those fields.

Kitchen has a number of commands that represent the lifecycle of the virtual machine that you are about to administer.

* `kitchen create` is the first state. This will download the box file, if necessary, from the bento repo and store it away for you. It will creates and turns on a virtual machine through the specified driver for you. At this moment you would be able to login to the machine `kitchen login` to look around.

* `kitchen converge` is the second state. This will perform all create does and then it will install chef and perform a `chef-client` run with the run_list specified in the kitchen YAML file. This process will validate that your recipes successfully converges.

* `kitchen verify` is the third state. This will perform all of converge and then it will execute any test kitchen tests that you have defined in the the `test/integration/...` directory. Without tests this step is the same as converge.

* `kitchen destroy` destroys the instance. This is a useful clean up step if you want to converge the recipes on a brand new instance.

* `kitchen test` destroys the instance if present, runs through to the verify stage, and then destroys the instance. This is a useful step if you have defined a number of tests and you want to ensure that when they are run that the instance is clean.

* `kitchen login` allows you to login to the machine. This is useful if you want to perform some exploratory examination of the OS.

There are additional features that can be specified in the kitchen YAML file to make interacting with the specified platform. One particular setting is the ability to forward the ports from the guest system (the virtual machine) to the host system (the workstation).

```
platforms:
  - name: centos65
    driver_config:
      network:
      - ["forwarded_port", {guest: 80, host: 8880}]
      - ["forwarded_port", {guest: 81, host: 8881}]
```

## ServerSpec

ServerSpec provides a framework for performing tests on the recipes that you write. ServerSpec tests are run when either the `kitchen verify` or `kitchen test` commands are executed. When these tests execute they are making assertions against the virtual OS to ensure that the system meets the defined expectations.

ServerSpec and ChefSpec are similar in their syntax because they share the same roots with RSpec. However, ServerSpec has its own setup requirements and provides its own helper methods and matchers to assist with testing an OS.

These tests often do not take long to execute but the entire process of setting up the OS it is never as fast as ChefSpec. ServerSpec tests are often times refered to as integration tests because they are making assertions about the state of the OS after a recipe or multiple recipes have been executed. These tells ensure that you are writing valid recipes that have a higher chance of working when deployed to a similar OS.

Similar to Chef being a Domain Specific Language (DSL) written on top of Ruby there are many other tools that use this same paradigm. ServerSpec is one such tool. ServerSpec is built on top of a DSL named RSpec. And RSpec is a testing framework that is built on top of Ruby.

> Refer to the RSpec introduction in the ChefSpec portion

Test Kitchen is more particular than RSpec with regard to where the test files are located. The file structure mirrors the following:

```
COOKBOOK_NAME/test/integration/SUITE_NAME/serverspec/RECIPE_NAME_spec.rb
```

* `test/integration` - Test Kitchen will look for tests to run under this directory. It allows you to put unit or other tests in test/unit, spec, acceptance, or wherever without mixing them up. This is configurable, if desired.

* `SUITE_NAME` - This corresponds exactly to the Suite name we set up in the .kitchen.yml file. If we had a suite called "server-only", then you would put tests for the server only suite under

* `serverspec` - This tells Test Kitchen (and Busser) which Busser runner plugin needs to be installed on the remote instance.

* `RECIPE_NAME_spec.rb` - All test files (or specs) are name after the recipe they test and end with the suffix "_spec.rb". A spec missing that will not be found when executing kitchen verify.

There are for more helpers and other things to learn about ServerSpec. The best thing is to read the project [website](http://serverspec.org/) and review the large set of testable [resource types](http://serverspec.org/resource_types.html).

## Workstation Setup

* Install the ChefDK

* Install VirtualBox

* Install Vagrant

* Install Sublime or Atom or Notepad++
