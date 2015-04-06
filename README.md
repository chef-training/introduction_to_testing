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

This workflow is sustainable for developing software if know for certain that you are never going to change the cookbook source code. In isolation, cookbooks with small goals on new systems are easy to reason what the recipes will install and the effects it will have on the system.

> This is often the crux of demonstrating testing tools and practices. The code or systems are reasonable. Asking you to write tests for a completely reasonable system feels often like wearing a belt and suspenders -- extra work without a lot of benefit. The benefit is often not felt until the system starts to become unreasonable and adding tests then often feels too difficult.

But it seldom that our cookbooks maintain small goals on new systems. We are often confronted with the complexity that originally brought us to use configuration management in the beginning.

Testing and a testing practice will help you better understand the system that you are building. It will make the code that you write more realible. And when something breaks (which it still will break) the errors will shift from your lack understanding of your objective and instead be related to the environment in which the code integrates with other code.

Testing is about understanding and verifying your intent of the code that you wrote. The tests will provide an automated way of ensure you don't lose sight of your goals. This becomes important as you invite new members to your team to assist you with development. Tests will help you understand code written by other team members. Tests will even help you when you return to code you have not seen in months or years.

In the same way that writing Chef recipes to capture the state of your infrastructure. Testing and linting tools will assist in ensuring that you have defined the correct infrastructure.

### New Cookbook Workflow

* Write some cookbook code
* Perform checks for code correctness
* Perform checks to ensure correctness with unit tests
* Deploy the cookbook to an integration environment
* Perform checks to ensure correctness with integration tests
* Deploy the cookbook to production environment

During this workshop the attendee will demonstrate this new cookbook workflow through the use of following tools:

* Code Correctness - Foodcritic and Rubocop
* Unit Tests - ChefSpec
* Integration Tests - Test Kitchen with ServerSpec

## Foodcritic

## Rubocop

## Test Kitchen

## ServerSpec

## ChefSpec

## Workstation Setup
