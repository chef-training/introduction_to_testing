## ChefSpec - Summary

* Where do ChefSpec tests run?

* How can ChefSpec improve your cookbook development process?

ChefSpec provides fast feedback as you are developing cookbooks. By defining and executing tests you ensure that the resources defined within the recipes are validated against the expectations defined in your tests. This allows you a greater change to introduce changes to your recipes without breaking existing functionality.

* Where does ChefSpec look for tests?

Within each cookbook there is a 'spec' directory. The test files are contained in that folder. All test files have the suffix '_spec.rb'.