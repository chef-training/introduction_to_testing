## ServerSpec

Test Kitchen is more particular than RSpec with regard to where the test files are located. The file structure mirrors the following:

```
COOKBOOK_NAME/test/integration/SUITE_NAME/serverspec/RECIPE_NAME_spec.rb
```

* `test/integration` - Test Kitchen will look for tests to run under this directory. It allows you to put unit or other tests in test/unit, spec, acceptance, or wherever without mixing them up. This is configurable, if desired.

* `SUITE_NAME` - This corresponds exactly to the Suite name we set up in the .kitchen.yml file. If we had a suite called "server-only", then you would put tests for the server only suite under

* `serverspec` - This tells Test Kitchen (and Busser) which Busser runner plugin needs to be installed on the remote instance.

* `RECIPE_NAME_spec.rb` - All test files (or specs) are name after the recipe they test and end with the suffix "_spec.rb". A spec missing that will not be found when executing kitchen verify.

There are for more helpers and other things to learn about ServerSpec. The best thing is to read the project [website](http://serverspec.org/) and review the large set of testable [resource types](http://serverspec.org/resource_types.html).
