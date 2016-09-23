# handbook
Living styleguide and development best practices for Blackbird Studios.

## Table of Contents

- [Workflow](#workflow)

## Workflow

- GitHub branches should be utilized as follows:
	- `master`: "Stable" code. Usually corresponds to __production__ environments. Developers should not push new features to this branch. Ideally, this branch should only be pushed to after code has been thoroughly tested.
	- `dev`: The "working" branch for any given project. Developers can push small changes to this branch, but for larger features, a separate branch should be made and then merged in. Usually corresponds to __development__ and/or __staging__ environments depending on the project.
	- `staging`: This branch is useful for larger projects, but most likely not applicable to most of the prototypes we build. The __staging__ environment can also be considered as __pre-production__. This is a good branch to run tests on before deploying.

- For larger projects, we have **continuous integration** set up with **Jenkins**.

- External resources:
	- http://guides.beanstalkapp.com/deployments/best-practices.html
