## Introducing Rubocop

### Obtaining Rubocop

Rubocop is part of the standard set of utilities included with the Chef Development Kit. 

// Do we need to explain how to get Rubocop for folks not using Chef DK

### Using Rubocop

Rubocop is a command-line application that is executed against a specified path. To test a complete cookbook, supply a path that is the name of the specific cookbook. 

In a monolithic workflow where your cookbooks are all stored within the chef-repo/cookbooks directory, you would run rubocop with the cookbooks/COOKBOOK_NAME:

```bash
$ rubocop cookbooks/setup
```

While developing cookbooks you will find it easier or necessary to work within the directory of a cookbook. In those instances you may execute the command with a period to represent the current cookbook.

```bash
$ cd cookbooks/setup
$ rubocop .
```

When executed with a path that contains ruby files, Rubocop will return to you, via standard out, the results of the evaluation. This evaluation is a suite of enabled 'cops' that examine the code from a number of different perspectives that yield a list of of warnings, deviations from conventions, potentional errors, and fatal errors.

* Enforces style conventions
* Enforces best practices within Ruby
* Evaluate the code against metrics (e.g. line length, function size)

By running Rubocop early and often in your development process you gain two advantages:

* identify simple errors early.
* improve the review process by eliminating stylistic issues  

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

### Configuring Rubocop

Rubocop feedback can sometimes feel less helpful. A majority of the raised issues will be related to style and formatting (e.g. spaces, parenthesis, and other minutia within the language).

> Even some of the files (e.g. metadata.rb) the Chef tools generate will immediately raise issues when reviewed by Rubocop.

Rubocop is executed with the following default set of enabled [rules](https://github.com/bbatsov/rubocop/blob/master/config/enabled.yml). There are also a number of [disabled rules](https://github.com/bbatsov/rubocop/blob/master/config/disabled.yml).

To improve the effectiveness of the tool, you can configure the various cops to align with the conventions within your organization that are important. In general, the conventions should be the same within an organization to ensure individuals within teams are familiar with the company standard. As a team, decide what standards you want to comply with, and you may disable cops that are not important for your cookbook design and correctness.

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

Within your cookbook:

```bash
$ rubocop --auto-gen-config
```

This generates a file named `.rubocop_todo.yml`. You can simply rename this file as `.rubocop.yml` if you want to accept it as the rubocop standard for the cookbook.

Often times this file is generated as a list of items you want to review one-at-a-time. This is why the file is named `.rubocop_todo.yml`. Instead of renaming this file you can define a `.rubocop.yml` that includes this file as well.

```yaml
inherit_from: .rubocop_todo.yml
```