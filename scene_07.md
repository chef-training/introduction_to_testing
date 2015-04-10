## Foodcritic

Foodcritic is command-line application that is executed against a specified path. The path provided to the application usually the name of a specific cookbook.

```bash
$ foodcritic cookbooks/setup
```

While developing cookbooks you will find it easier or necessary to work within the directory of the cookbook. In those instances you may execute the command with a period to represent the current cookbook.

```bash
$ cd cookbooks/setup
$ foodcritic .
```

Foodcritic will return to you, via standard out, the results of its evaluation. This evaluation is against a large body of enabled rules that examine the code from a number of different perspectives that yield a list of violations.

* a particular rule
* a classification of rules,
* all rules with the exclusion of some rules

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

Foodcritic allows you to execute specific rules. This is defined with the -t flag followed by the food critic violation.

```
$ foodcritic -t FC003
```

Everything but a specific rule. Again with the -t flag and followed by the food critic violation. Except, you may have noticed with a proceeding tilda character.

```
$ foodcritic -t ~FC003
```

Foodcritic violations have tags associated with each of them. The tags can be used in similar way when wanting to filter its execution.

```
$ foodcritic -t services
$ foodcritic -t ~style
```

The first example demonstrates running only the violations tagged with "services". The second example demonstrates all violations except the ones tagged with "style".

Foodcritic allows you to define multiple instances of the '-t' flag so that you can compose what rules you want to run.

```
$ foodcritic -t services -t style -t ~FC002
```

When you have defined your complete set of tag options you are able to save them to a file named '.foodcritic'. Foodcritic will read this file upon exectuion and use those as the default options.

```
$ echo "style,services" > my_cookbook/.foodcritic
```