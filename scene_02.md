## Rubocop - Intro

The majority of the time you are working with Chef you are writing ruby code. Most every file within a cookbook is a ruby file - the metadata, recipes, attributes and any source that you have defined in the libraries folder.

In this section we are going to use Rubocop, a linting tool, to evalute the ruby files within our cookbooks. Rubocop assists us by evaluating our code and giving us feedback. This feedback shows us where our code violates ruby community conventions, display warnings, potential errors, and even fatal errors.

This tool is built for Ruby developers and the conventions that it attempts to enforce are those defined by the community of Ruby developers that work with the project. As cookbook authors we do not always have the same objectives as Ruby developers but there is enough of an overlap that the tool is beneficial.