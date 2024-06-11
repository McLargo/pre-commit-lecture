# Pre-commit lecture

[![pre-commit](https://img.shields.io/badge/pre--commit-enabled-brightgreen?logo=pre-commit)](https://github.com/pre-commit/pre-commit)

Before starting talking about pre-commit, let's talk about git hooks.

## Git hooks

- They are scripts that Gits runs before or after events.
- Divided in two groups:
  - client-side: are triggered by operations such as committing and merging.
  - server-side: run on network operations such as receiving pushed commits.
  - stored in `.git/hooks` directory.

## When is executed pre-commit hook?

- pre-commit hook is run before you commit your changes.
- It is executed on the client-side, not in the server. It does not affect the
  repository, only the local copy.

## Why is it useful?

- detect secrets/private keys in your repository.
- check if your code is formatted correctly (python, yaml, json, swagger...).
- prettify your code, like trims trailing white spaces, final break lines, etc.
- validates documentation (markdown, reStructuredText, docstrings).
- run linters and check for common errors.
- execute your tests and make sure they pass or check coverage.

But hey, some of this is already managed by your IDE, right? Yes, but pre-commit
is making sure that all the developers in your team are using the same tools and
configurations.

## pre-commit framework

- pre-commit is a framework for managing and maintaining multi-language
  pre-commit hooks.
- It is a client-side hook. Something you install on your local machine.
- It is a Python package that you can install with pip.
- It is configured using a `.pre-commit-config.yaml` file.
- It is run using the `pre-commit` command.

### Where to start

- Install pre-commit: `pip install pre-commit`. Remember to install it in your
  virtual environment if you are using one.
- Create a `.pre-commit-config.yaml` file in the root of your repository.
- Add the hooks you want to use in the `.pre-commit-config.yaml` file.
- Run `pre-commit install` to install the pre-commit hooks.
- Run `pre-commit run --all-files` to run the pre-commit hooks on all files.

### Hooks

There are hundreds of hooks available for pre-commit. You can find them in the
[pre-commit hooks](https://pre-commit.com/hooks.html) page. To configure a hook,
you can see keys and values in the
[pre-commit documentation](https://pre-commit.com/#pre-commit-configyaml---hooks).

pre-commit can be
[configured to use in other stages](https://pre-commit.com/#confining-hooks-to-run-at-certain-stages),
such as pre-push and pre-merge-commit. Just add the `stages` key in the hook and
set the stages you want to run the hook.

You can also create your own hooks following the instructions available in
[Creating new hook](https://pre-commit.com/index.html#new-hooks). You can use to
run your tests or check coverage. You can also use it to run your own scripts.

Which hooks to use, depends on the programming languages and the project. You
can add as many hooks as you want, but remember that the more hooks you add, the
longer it will take to run the pre-commit hooks. Let's take a look to the
`pre-commit-config.yaml` file in this repository.

### Maintenance

- As hooks are versioned, they can be updated to add new functionality and bug
  fixes. You can update the hooks by running `pre-commit autoupdate`
  periodically.

### Caveants

- `pre-commit` only runs in the staging area, so you need to add the files to
  the staging area before running `git commit`. Only applicable on stage commit.
- If you add `pre-commit` in a started project, or a new hook to your list, you
  need to manually execute `pre-commit run -all` to make sure hooks are executed
  in all files to detect issues.
- `pre-commit` uses a different virtualenv, so new dependencies are installed in
  the pre-commit virtualenv. This may cause some issues with the dependencies
  versions, in case you have different versions in your project/local and in the
  pre-commit virtualenv.
- It does not replace checks in the CI/CD pipeline, it is just a way to catch
  errors before they are committed. Additionally, pre-commit can be installed in
  your CI, as [github actions](https://pre-commit.com/#github-actions-example)
  or [gitlab CI](https://pre-commit.com/index.html#gitlab-ci-example). Saving
  time, effort and money. Very frustrating if you have to wait for the CI/CD
  pipeline to run to see if your code is correct.
- Ensure that the pre-commit hooks are not too slow, as it can be frustrating to
  wait for the hooks to run every time you commit, specially if you are using
  `git-flow`. If you have a slow hook, you can set it to run only on stage push.

### Skipping

- World is not perfect, isn't? You can
  [disabled temporarily hooks](https://pre-commit.com/#temporarily-disabling-hooks)
  by:
  - by using the `--no-verify` flag when you commit.
  - by using the `SKIP=hook-id` keyword in the commit message.
- If pre-commit is a pain in the neck, you can uninstall it by running
  `pre-commit uninstall`.

### The extra mile

- Documentation is quite extended and we just scratch the surface of all
  features and configuration pre-commit offers. Feel free to explore the
  [pre-commit documentation](https://pre-commit.com/).

## References

- [git hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)
- [pre-commit framework](https://pre-commit.com/)
- [pre-commit hooks](https://pre-commit.com/hooks.html)
