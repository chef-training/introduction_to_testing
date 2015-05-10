-

## RSpec - Language

Welcome to Introduction to Testing with Chef.

-

In this section we will be introducing RSpec, a testing framework.

-

RSpec is a Behavior Driven Development (BDD) framework that uses a natural language domain-specific language (DSL) to quickly describe scenarios in which systems are being tested.

-

ChefSpec, a unit testing tool, is built on the RSpec DSL.

-
5
-

ServerSpec, an integration testing tool, is also built on the same RSpec DSL.

-

RSpec allows you to setup a scenario, execute the scenario, and then define expectations on the results.

-

```ruby
describe "1 plus 1" do
  it "equals 2" do
    a = 1
    b = 1
    sum = a + b
    expect(sum).to eq(2)
    expect(sum).not_to eq(3)
  end
end
```

Here is an example of a specification. This specification, or test, describes the scenario where one plus one equals the value two.

-
8
-

The outer describe method creates an example group.

-
9
-

The first parameter is a string which describes the focus of the example group.

-
10
-

The second parameter is the block that contains all the examples defined.

-
11
-

In this specific instance we are concerned with defining an example group dedicated to testing everything that happens when adding a one to another one.

-
12
-

Inside the example group we define examples. An example starts with the it method.

-

Its first parameter is a string which describes the focus of this single example.

-

The second parameter is the block that contains a scenario to test.

-
15
-

Reading an example out loud one starts from the outermost describe block and moves inwards until you reach a specific example. Here the example we are testing is "1 plus 1 equals 2"

-

An example setups a scenario, executes the scenario, and then asserts if the expectations are met.

Here we setup two variables a and b. Assigning them both the value of 1.

To execute the scenario we add these two values together.

Finally we define an expectation to validate that the resulting sum of adding the two numbers is equal to the value 2. And to be safe we ensure that in another expectation that the sum is not equal to the value 3.

-

The expectations are defined with another set of helper methods that allow us to continue the use of natural language to create an English-like sentence with ruby code.  

-
18
-

The expect method wraps the resulting value from the scenario. Then that wrapped value, called the expectation target, is sent a message to compare it to a matcher.

-

We use `to` when we want the comparison between the resulting value and the matcher to be true.

-
20
-

We use `not_to` when we want the value and the matcher to not be true.

-
21
-

```ruby
describe "Math" do
  context "when adding 1 + 1" do
    let(:sum) { 1 + 1 }

    it "equals 2" do
      expect(sum).to eq(2)
    end
  end

  context "when adding 2 + 2" do
    let(:sum) do
      2 + 2
    end

    it "equals 4" do
      expect(sum).to eq(4)
    end
  end
end
```

RSpec provides two more important helper methods.

The first is context.

-
22
-

Context is an alias of `describe`. It gives the ability to define an even smaller example group that is part of the larger example group. This smaller group is isolated from other similarly defined example groups.

The rule to use when considering if you should add a new example group is if you are thinking to yourself of different scenarios. As in ... when I am on this platform, when I am on that platform.

-
23
-

Here within the first context we use another helper method, let.

-

The let defines a helper method that shares the name of the symbol specified when creating it.

This method is useful as it will calculate the value of sum the first time, store it, and return that same value every time it is requested until the end of the example.

This is a performance benefit and provides clarity to the tests that they define by again focusing on natural language in the specifications.
