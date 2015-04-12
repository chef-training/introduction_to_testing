## Rubocop - Introduction

Welcome to Introduction to Testing with Chef. In this section we will be introducing and using Rubocop, a linting tool. As mentioned before, linting tools provide automated ways to ensure that the code we write adheres to conventions that ensure code uniformity, portability, and uses best practices. This ensures everyone on the team writes similarly structured source code. It helps weave the expectations into the development of the code, and encourages collaboration over time. Ensuring the uniformity of source code helps set the expectations for fellow project contributors.

The majority of the time you are working with Chef you are writing ruby code. While Chef uses an internal domain specific language (DSL), it is all ruby! Most every file within a cookbook is a ruby file - the metadata, recipes, attributes and any source that you have defined in the libraries folder as signaled by the file ending **.rb**. To lint ruby code, you use Rubocop the ruby linting tool. 

Rubocop will evalute the ruby files within your cookbooks and provide feedback about your adherence to [ruby style guidelines](https://github.com/bbatsov/ruby-style-guide). This feedback shows you where the code violates ruby community conventions, display warnings, potential errors, and even fatal errors. For example, in ruby the expectation is that there are **two spaces** per indentation level. If Rubocop discovers lines with **three spaces** you will get a warning.

This tool is built for Ruby developers and the conventions that it attempts to enforce are those defined by the community of Ruby developers that work with the project. As cookbook authors we do not always have the same objectives as Ruby developers but there is enough of an overlap that the tool is beneficial. Rubocop is configurable. You can switch on and off specific style guides to prevent warnings for checks that don't match your organization's conventions.

During this section you will learn and demonstrate how to use Rubocop effectively:

* Linting with Rubocop 
* Modifying Rubocop Configuration
* Rubocop exercise
