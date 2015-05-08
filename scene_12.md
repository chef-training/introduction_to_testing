## RSpec - Language

RSpec is a Behavior Driven Development (BDD) framework that uses natural language to quickly describe scenarios in which systems are being tested.

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

* `context` - this is an alias of the `describe`. It cannot however be used as a top-level item. The reason for nesting a `context` or `describe` is to further refine your tests by creating a group of specific tests for a given scenario.

The rule to use when considering if you should add a context to your tests is if you are thinking to yourself ... when I am on this platform ... and when I'm on that platform ...


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
