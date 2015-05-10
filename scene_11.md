-

## RSpec - Application

Welcome to Introduction to Testing with Chef.

In this section we will be introducing RSpec, the command-line tool, that allows us to test our cookbooks.

-

RSpec is part of the standard set of utilities included with the Chef Development Kit.

-

RSpec is a command-line application that allows you to execute a body of tests against the source code that you have written.

-

Rspec, by default, assumes you are working within a cookbook and that cookbook has a sub-directory named 'spec'.

Here is an example of us changing into the directory for the git cookbook and running the rspec command. Rspec will evaluate the tests defined for the git cookbook.

-
5
-

Rspec allows you to specify one or more paths after the command. These can be individual test files or entire directories of test files.

In this example we are exercising the tests defined in the file spec-slash-unit-slash-default-underscore-dot-spec and all the examples in all the test files found in spec-slash-unit-slash-windows.

-

Most cookbooks assume that you are running tests from within the cookbook. So when you run rspec outside of the a cookbook you may be confronted with an error requiring helper files.

In this example we are in a monolithic repository where our cookbooks are stored in a single cookbooks directory.

We attempted to run rspec against all the tests defined for the iis cookbook. Instead of the correct output we receive a stack trace that mentions the failure attempting to load a helper file.

> This will likely represent itself as an error related to issues with `require 'spec_helper'`. It is a Ruby load path issue. When RSpec runs it adds a few directories onto the load path (i.e. 'lib', 'spec'). So when you are in the directory of a cookbook all the files within that 'spec' directory can be required without any additional paths (i.e. `require 'spec_helper'`).

-

RSpec has many different forms of output. The default is called the 'progress' format and the output looks and reads similar to rubocop and foodcritic.

-
8
-

A period is written to the output for each example that has successfully passed. An F is written out for a failing example. A star or asterick for examples that have been marked as pending.

-
9
-

```
Failures:

  1) ark::default sets default attributes prefix root
     Failure/Error: expect(default_cookbook_attribute("prefix_root")).to eq "/usr/l ocal"

       expected: "/usr/l ocal"
            got: "/usr/local"

       (compared using ==)
     # ./spec/recipes/default_spec.rb:30:in `block (3 levels) in <top (required)>'

Finished in 1.71 seconds (files took 1.98 seconds to load)
82 examples, 1 failure

Failed examples:

rspec ./spec/recipes/default_spec.rb:29 # ark::default sets default attributes prefix root
```

Upon completion of the run if there are any failures they will be displayed.

-
10
-

Failures are numbered and use the language from the test file as the description of the failure.

-
11
-

After the description the error will be displayed to you. It often times repeats the expectation that you have defined. Then it will show you the expected value and the got value -- the value that came back from the execution.

In this example the test expected a node attribute to return the value slash-user-slash-local. Instead we find that the node attribute is being set incorrectly as it returns slash-user-slash-L-Space-OCAL.

-
12
-

The position of the failure will be displayed by outputting the file name followed by the line number.

-
13
-

RSpec displays a summary of the results. Included is the time it took to execute the tests. How many passed, failed, and are marked as pending.

In this example 83 tests were found; 82 passed; 1 failed.

-
14
-

RSpec also represents the failures again as commands you could execute if you wanted to run a specific test. To do this you need to specify the file name and a line number. The line number is any line within the defined example ('it' block).

In this example, if we were to copy and run this command. We are asking rspec to execute only the example defined in the file spec-slash-unit-slash-default-underscore-spec-dot-R-B at line 29.

-
15
-

RSpec has a different format outputter that displays more information on execution for all tests - not just the failing ones. It's called the 'documentation' formatter and you can enable it from the command line with dash-F flag passed with the parameter documentation. The documentation format can be abbreviated with simply the letter D.

In this example we are changing into the directory of the ark cookbook and then running all the tests in the spec directory but choosing to display the output in documentation format as opposed to the default progress format.

-

RSpec also supports color in the output when you provide the rspec command dash-dash-color or dash-C. This will display failing examples in red, and pending examples in yellow.

In this example we changed into the redis cookbook's directory and ran rspec against all the tests in the spec directory while displaying colors to assist us with reading the output.
