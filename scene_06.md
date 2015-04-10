## Foodcritic - Introduction

Welcome back. In this section we will be introducing and using Foodcritic, a linting tool. As mentioned before, linting tools provide automated ways to ensure that the code we write adheres to conventions that ensure code uniformity, portability, and uses best practices. This ensures everyone on the team writes similarly structured source code. It helps weave the expectations into the development of the code, and encourages collaboration over time. Ensuring the uniformity of source code helps set the expectations for fellow project contributors.

When it comes time to composing cookbook code you often have many ways to solve your problems. Chef provides a domain-specific language that helps you quickly define attributes, recipes, and libraries. The DSL is simply helper methods on top of Ruby.  While Rubocop assists in linting ruby specifically, Foodcritic will help us specifically lint Chef code.

This tool is built for Chef developers to evaluate cookbooks and the conventions that it attempts to enforce are those defined by the community. Depending on your background, you may find that some conventions are unhelpful. Foodcritic is configurable allowing you to modify which rules are checked. You can also create and share additional rules that you create based on your organization's conventions.

During this section you will learn and demonstrate how to use Foodcritic effectively:

* Lint with Foodcritic
* Modify Foodcritic Configuration
* Create custom Foodcritic rule
* Foodcritic exercises