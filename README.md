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
    - id: is-dev-branch-updated
      args: ['main']
    - id: does-branch-last-too-long
      args: ['-d', 3, '-e' , '"main dev"']
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
#### Concept
From [Short-lived feature branches](https://trunkbaseddevelopment.com/short-lived-feature-branches/), when many developers work concurrently, **they must merge updated main/trunk before merging back to main/trunk**  
![process](https://trunkbaseddevelopment.com/short-lived-feature-branches/slfb_pull-push.png)  
(cr: [Trunk based development](https://trunkbaseddevelopment.com))  

#### Description
This hook will recheck remote development branch if it has been updated.
If so, It will stop the git action  

#### Recommended States
pre-push, pre-merge, pre-rebase  
(pre-commit is still acceptable, but it may appear annoying)

#### Configuration
```yaml
hooks:
  - id: is-dev-branch-updated                        # required
    args: ['main']                                   # optional
    states: ['pre-push', 'pre-merge, pre-rebase']    # optional
```
#### Arguments
| argument       | example | description                                   | default |
|----------------|---------|-----------------------------------------------|---------|
| &lt;branch&gt; | dev     | main development branch that have to be check | dev     |


  
### 2. does-branch-last-too-long
#### Concept
According to [Short-lived feature branches](https://trunkbaseddevelopment.com/short-lived-feature-branches/), the branch should only last a couple of days.  If the branch is left for more than two days, there is a risk of that branch becoming a long-lived feature branch, which is the antithesis of trunk based development
#### Description
This hook will check if the current branch is older than the specified threshold.
If it lasts too long, the pre-commitment will fail.

There are some branches that are special, such as master, main, and trunk. That long-lived branch should be added in the exclusion list to ignore those branches from this rule.

#### Recommended States
pre-commit, pre-push

#### Configuration
```yaml
hooks:
  - id: does-branch-last-too-long                 # required
    args: ['-d', 3, '-e' , "main dev"]            # optional
    states: ['pre-commit', 'pre-push']            # optional
```
#### Arguments
| argument                  | example       | description                                            | default |
|---------------------------|---------------|--------------------------------------------------------|---------|
| -d/--days &lt;days&gt;    | -d 3          | Maximum number of days that a branch can be committed. | 3       |
| -e/--exclude &lt;list&gt; | -e "main dev" | List of branches that will be excluded from this rule. | master  |
 