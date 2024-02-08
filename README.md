# Dev Practice Hook

Enforcing git hooks stuff, for better development routine.
As [pre-commit](https://pre-commit.com) extension, you can easily integrate with other rules in order to make your team development routine become more consistent and minimize conflict effect.

## Prerequisite
1. [Pre-commit Hook](https://pre-commit.com)
2. [Git](https://git-scm.com)
3. [Bash Script](https://www.gnu.org/software/bash/)

## Limitation
Just test only in MacOS for now

## Configuration
1. Just add this stuff into `.pre-commit-config.yaml`
```yaml
-   repo: https://github.com/bomu-bomu/dev-practice-hook.git
    rev: 4ce1faddb10c98fd0151f75d1688188845e66b70
    hooks:
    -   id: is-dev-branch-updated
        args: ['main']
```
### Argument
The only one argument is branch name which should be your main development branch.
_Default_: `dev`
For example:
- if your development branch is `main`, use
```yaml
  args: ['main']
```

2. If you are not install hooks, do this command
```sh
> pre-commit install                           # for pre-commit hook
> pre-commit install --hook-type pre-merge     # for pre-merge hook
> pre-commit install --hook-type pre-rebase    # for pre-rebase hook
> pre-commit install --hook-type pre-push      # for pre-push hook
```


## Available Hooks
### 1. is-dev-branch-updated
***Concept***: From [Short live feature branches](https://trunkbaseddevelopment.com/short-lived-feature-branches/), when multiple of developers work concurrently, **they have to merge updated main/trunk before merging back to main/trunk**
![process](https://trunkbaseddevelopment.com/short-lived-feature-branches/slfb_pull-push.png)
**Description**: This hook will recheck remote development branch if they are updated.
If so, It will stop git action
**Recommended State**: pre-push, pre-merge, pre-rebase  (pre-commit is still ok, but may seem annoyed)