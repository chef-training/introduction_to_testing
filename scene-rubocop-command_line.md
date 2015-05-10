## Introducing Rubocop Command Line

Welcome to Introduction to Testing with Chef.

In this section we will be introducing Rubcop, the command-line tool, that allows us to lint our cookbooks.

-

Rubocop is part of the standard set of utilities included with the Chef Development Kit.

-

Rubocop is a command-line application that analyzes source code to verify the syntax and structure of our cookbooks.

-

Rubocop, by default, assumes you are working within the directory of a cookbook. When invoked without any parameters it will assume you want to evaluate the contents of your current directory and any of its sub-directories.

Here is an example of us changing into the directory for the apache cookbook and running the rubocop command. Rubocop will evaluate the apache cookbook.

-

Rubocop allows you specify one or more paths after the command to specify the path or paths to the source code that you want to examine.

In a monolithic repository where your cookbooks are all stored within the chef-repo/cookbooks directory, you would run rubocop with the cookbooks/COOKBOOK_NAME.

-

Here is an example of us linting the iis cookbook.

-

If you wanted to test multiple cookbooks that were stored within chef-repo/cookbooks, you would run rubocop with each of the cookbook directories listed.

Here is an example of linting the mongodb cookbook and the mysql cookbook.

-

Rubocop will examine the local directory, path or paths specified for ruby files.

-

It will return, via standard out, the results of the evaluation. This evaluation is a suite of enabled rules known as 'cops' that examine the code from a number of different perspectives that yield a list of warnings, deviations from conventions, potential errors, and fatal errors.

-

By running rubocop early and often in your development process you gain two advantages:

* identify simple errors early
* improve the review process by eliminating stylistic issues

-

Now you know how to use the tool lets examine the results of a rubocop execution:

-

The results start with a summary describing the number of ruby files that were found and examined. Next it displays the results of each of those files as a series of symbols or letters.

* `.` means that the file contains no issues
* `C` means an issue with convention
* `W` means a warning
* `E` means an error
* `F` means a fatal error

-

After the summary each of the offences are displayed in the following format:

-

FILENAME is the partial path of the file relative to where the rubocop command was executed.

The colon character is a deliminiter.

The next field is the line number of the violation. Colon. Followed by the column number. Colon. The type of error. And finally the error message.

-

Below is an excerpt of the source code from the above described file. This is where the error was identified.

-

The carrot symbol on the next line indicates the column number where the error was detected.

-

By default, rubocop shows the result in the "progress" format. Formatters allow you to see the results in different ways. Lets demonstrate the use one such formmater.

Choosing the offenses format gives output grouped in categories like the following.

For example, "Style/SingleSpaceBeforeFirstArg" is a **style** cop referring to a specific rule around **the number of spaces that appear after a function name and the first parameter**.

-

In general, cops are divided into 3 types: Lint, Rails, and Style. If we want to run rubocop and only check the code for correctness you could run with the parameter **--lint**.

-

To list all of the available cops, you can run rubocop with the **--show-cops** parameter.

-

Here we see a lint cop named AmbiguousOperator with its description and a link to the Rubocop documentation.

-

With this information you could then test a single cop at a time; given the name with the parameter **--only COPNAME**.

-

### Configuring Rubocop

Rubocop is executed with the following default set of enabled [rules](https://github.com/bbatsov/rubocop/blob/master/config/enabled.yml).
-

There are also a number of [disabled rules](https://github.com/bbatsov/rubocop/blob/master/config/disabled.yml).

-

Rubocop feedback can sometimes feel less helpful. A majority of the raised issues will be related to style and formatting (e.g. spaces, parenthesis, and other minutia within the language).

> Even some of the files (e.g. metadata.rb) the Chef tools generate will immediately raise issues when reviewed by Rubocop.

To improve the effectiveness of the tool, you can configure the various cops to align with the conventions within your organization that are important. In general, the conventions should be the same within an organization to ensure individuals within teams are familiar with the company standard. As a team, decide what standards you want to comply with, and you may disable cops that are not important for your cookbook design and correctness.

Each cookbook you create can have its own custom set of enabled, disabled, and configured cops.

> While you are able to define different rubocop rules per cookbook often times all your cookbooks within your organization will share the same rules.

-

Within a particular cookbook you define a YAML (YAML Ain't Markup Language) file named `.rubocop.yml`. YAML files store data in a white-space significant formatted file.

This is an example of a common `.rubocop.yml` for Chef cookbooks.

-

Within the example file we disabled a number of cops that are enabled by default.

-

We also overrode the value of the LineLength to allow for us to write source code lines up to 200 characters without raising an error (defaults to 80).

-

Each entry within the file adheres to the following format:

```
NAME_OF_COP:
  Enabled: (true or false)
  PARAMETER_KEY: PARAMETER_VALUE
```

* NAME_OF_COP - The name of the cop that is being described.

-

* Enabled: (true or false) - This is where you can set the cop enabled or disabled. The default enabled setting is set to true for cops defined in the default [enabled](https://github.com/bbatsov/rubocop/blob/master/config/enabled.yml) file and false for cops defined in the default [disabled](https://github.com/bbatsov/rubocop/blob/master/config/disabled.yml) file.

-

* PARAMETER_KEY: PARAMETER_VALUE - define the parameters specific to the cop that is being applied. These values are dependent on the cops and not all cops have additional parameters to set.

-

When Rubocop is executed against a cookbook the default enabled and disabled cops are loaded and then the dot-Rubocop-dot-YAML file with the cookbook is evaluated. This allows you to customize the cops that are enforced.

-

There may be more "issues" that Rubocop is alerting you to that you would like to ignore initially or completely. Rubocop allows you to capture the current state of all the offences and write them to a file that you can accept as your core rules.

Within your cookbook run rubocop with the flag dash-dash-auto-dash-gen-dash-config.

-

This generates a file named `.rubocop_todo.yml`. You can simply rename this file as `.rubocop.yml` if you want to accept it as the rubocop standard for the cookbook.

-

