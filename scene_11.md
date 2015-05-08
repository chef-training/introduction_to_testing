## RSpec - Application

RSpec is a command-line application that by default is executed against the 'spec' directory within a cookbook. RSpec needs to be executed from within your cookbook otherwise it will be unable to find the 'spec' directory or be able to load helper files.

```bash
$ cd cookbooks/COOKBOOK_NAME
$ rspec
```

> This will likely represent itself as an error related to issues with `require 'spec_helper'`. It is a Ruby load path issue. When RSpec runs it adds a few directories onto the load path (i.e. 'lib', 'spec'). So when you are in the directory of a cookbook all the files within that 'spec' directory can be required without any additional paths (i.e. `require 'spec_helper'`).

RSpec has many different forms of output. The default is called the 'progress' format and the output looks and reads similar to rubocop and foodcritic.

* '.' - represents a passing example
* 'F' - represents a failing example

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

Upon completion of the run if there are any failures they will be displayed. Failures are numbered and use the language from the test file as the description of the failure.

After the description the error will be displayed to you. It often times repeats the expectation that you have defined. Then it will show you the expected value and the got (the value that came back from the execution) value.

The position of the failure will be displayed by outputting the file name followed by the line number.

RSpec also represents the failures again as commands you could execute if you wanted to run a specific test. To do this you need to specify the file name and a line number. The line number is any line within the defined example ('it' block).

```
$ rspec ./spec/unit/default_spec.rb:29
```

RSpec has a different format outputter that displays more text. It's called the 'documentation' formatter and you can enable it from the command line with `-f documentation` or the shorter format `-f d`.

```bash
$ rspec -f documentation
$ rspec -f d
```

You can also enable color in the output with the flag `--color` or `-c`. This will display passing examples in green and failing examples in red.