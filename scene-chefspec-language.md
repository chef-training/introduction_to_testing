## ChefSpec

ChefSpec brings the knowledge of Chef to RSpec specifically it:

* Allows you to simulate a chef-client run against a Server or in a Solo environment
* Allows you to reference the recipe name in the parent `descrbed` with the variable `described_recipe`
* Allows you to setup expectations against Chef core resources

You will define tests per cookbook and store them in a directory named `spec/unit`. A test file is a ruby file that ends with `_spec.rb`. You define a test file for each of the recipes that you have defined. The name of the recipe, e.g. `recipes/default.rb`, would have a matching test file named `spec/unit/default_spec.rb`.

Let's look at example of a test defined for the apache cookbook's default recipe:

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

Here we define a simulated Server Runner named `ChefSpec::ServerRunner` and we ask it to converage the default recipe for the apache cookbook. We return the instance of the runner any time we use the `chef_run` within our examples.

The `described_recipe` is in reference to the recipe name specified in the top-most describe string. In this instance it is the 'apache::default'. This helper saves you from repeating the recipe name throughout the test file.

Within the example 'installs apache2' you see that we have defined an expectation on the `chef_run` to install the package named 'httpd'. This is one of the many expectations that ChefSpec provides.

The format of these expectations:

```
expect(chef_run).to ACTION-NAME_RESOURCE-TYPE(RESOURCE-NAME)
```

I expect the chef_run to ACTION a RESOURCE with the given NAME.

ChefSpec also provides the ability to provide coverage. Coverage is a measure of all the resources your examples are able to touch through testing divided by the the total number of resources that could be tested. This results in a fractional value that is represented as a percentage.

To enable coverage we need to ask for ChefSpec::Coverage to yield the report when the test run is complete. This is done by adding the following source to each of your test files.

```ruby
require 'chefspec'
ChefSpec::Coverage.start!
```

Every test file would need to repeat this content so instead we will write this into a helper file - `spec/spec_helper.rb`. Now in each of our test files we instead require the spec_helper.rb file.

```ruby
require 'spec_helper'
```

There are more helpers and other things to learn about ChefSpec. The best thing is to read the project [README](https://github.com/sethvargo/chefspec) and review the large set of [examples](https://github.com/sethvargo/chefspec/tree/master/examples).