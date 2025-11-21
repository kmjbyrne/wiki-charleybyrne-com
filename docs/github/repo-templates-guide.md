# Using GitHub Repository Templates

The following is a condensed guide on how to use GitHub repository templates to create a new repository based on an
existing template.

## Creating Template Repository

First, you'll need a repository to act as a template.

For example, here is a Python template repository we can use:

<https://github.com/kmjbyrne/templates-python>

It comes packaged with linting tools and `pyproject.toml` starter.

## Creating Repository From Template

Next, let's get making the target repository.

Either you can integrate the template into an existing repository or create one from scratch. Existing repositories have their own challenges as merge commits can be a bit messy if you have conflicting names. It's generally easier to do this for new repos.

Let's create one from scratch to demonstrate.

```shell
mkdir /tmp/templated-repository
cd /tmp/templated-repository
git init
git remote add template https://github.com/kmjbyrne/templates-python
git fetch --all
git merge template/main
```

Now, your template repository is ready to go. Check our the linter.

```shell
bin/lint --fix
```
