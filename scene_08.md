## Foodcritic - Exercise

> TODO: There are instructions that need to be written to follow along with the exercise. The text that follows is some flavor text that can be used to talk about when to apply the rules.




When you violate a rule it does not automatically mean that the offending code needs to be changed. It is important to understand the rule and its intentions.

For instance, rule [FC003](http://www.foodcritic.io/#FC003), describes a scenario where the recipe uses the method `search` to retreive data from the Chef Server. It suggests that this cookbook will raise an error when running in a situation without a Chef Server. To maintain portability it suggests you check if you happen to be in "solo" mode (e.g. Chef Server is absent).

You may not want to adopt this rule within your organization because there are no imagineable situations where you will be executing this cookbook without a Chef Server. In these situations it is possible to execute foodcritic in a manner which excludes a rule.

```bash
$ foodcritic . --tags ~FC003
```

> Be aware that your rules you reject for your cookbooks may make it hard to use your cookbook in scenarios that you did not imagine. This is important to remember if you decide to share your cookbook with another team or the open-source community.