# Dev Practice Hook

Enforcing git hooks for a better development workflow.  
As [Pre-commit](https://pre-commit.com) plugin, you can easily combine with other rules to make your team development routine more consistent and reduce the conflict effect.

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
    rev: 0.0.1
    hooks:
    -   id: is-dev-branch-updated
        args: ['main']
```
### Argument
The sole argument is branch name which should be your main development branch.  
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
***Concept***: From [Short live feature branches](https://trunkbaseddevelopment.com/short-lived-feature-branches/), when many developers work concurrently, **they must merge updated main/trunk before merging back to main/trunk**  
![process](https://trunkbaseddevelopment.com/short-lived-feature-branches/slfb_pull-push.png)  
(cr: [Trunk based development](https://trunkbaseddevelopment.com))  

**Description**: This hook will recheck remote development branch if it has been updated.
If so, It will stop the git action  

**Recommended State**: pre-push, pre-merge, pre-rebase  (pre-commit is still acceptable, but it may appear annoying)
