# How are `git push`, `git fetch`, and `git pull` used?

In modern software development and documentation, git is most often used for source control. This system is based on two concepts: repositories and branches. A *repository* is a collection of files. A *branch* is a version of those files that can be edited independently of other versions of those files. A repository holds one or more branches. Often, though, it contains many branches, allowing different users to edit different versions of the files at the same time, so that one user does not overwrite another user's work.

Since each developer (and writer) contains a copy of the repository on their local machine (a *local repository*), they edit these *local branches* without requiring network access. When a developer is ready to share their work with other users, they upload their local branch to a repository accessible by the other users, the *remote repository*, which contains *remote branches*. Developers frequently download work from the remote repository and upload work to the remote repository. In this way, many different developers can work independently and share their work when it is complete.

While git provides many commands and options, you will use the following operations repeatedly:

- [`git push`](#git-push): Uploads changes from a local branch to a remote repo.
- [`git fetch`](#git-fetch): Downloads information from a remote repo about the current state of its branches.
- [`git merge`](#git-merge): Inserts material from one branch into another branch.
- [`git pull`](#git-pull): Performs `git fetch` and then performs a `git merge` from the remote branch into your local branch.

The following sections briefly describe each of these operations. For more detailed options for each of these operations, please see the [official git documentation online](https://git-scm.com/doc). Also, note that in the following sections, we assume that you have set up your local branches to track remote branches. For more information on setting up tracking branches, see [this page](https://www.git-tower.com/learn/git/faq/track-remote-upstream-branch/).

## `git push`

`git push` uploads changes from your local branch to the remote branch so that they are identical. Since the remote branch is in the remote repo (which everyone has access to) using `git push` shares your work with everyone else. Note that this operation requires your local branch to contain all of the commits that the remote branch contains *before* running `git push`, and will fail if this condition is not met. If the remote branch contains commits that your local branch does not yet contain, you have two choices: (a) first run [`git pull`](#git-pull), or (b) first run [`git fetch`](#git-fetch) followed by [`git merge`](#git-merge). After this, you can run `git push`.

**Syntax:** To upload changes from your local branch `hotfix` to the remote branch `hotfix` in the remote repository named `origin`, run

```
git push origin hotfix
```

## `git fetch`

`git fetch` downloads information about the current state of the remote repository, including all of the new commits and branches it contains. This operation is always safe to run, however, because it does not *change* your local branch. To update your local branch so it matches the remote branch, first run `git fetch` and then run [`git merge`](#git-merge).

**Syntax:** To download information from the remote repository named `origin`, run

```
git fetch origin
```

## `git merge`

`git merge` compares two branches and updates one of them with changes that the other one contains. This operation can be used to update one branch with information from any other branch, including remote branches and local branches. 

**Syntax:** To update the local branch `master` with information from the local branch `hotfix`, first switch to `master` by running

```
git switch master
```

and then execute the merge by running

```
git merge hotfix
```

## `git pull`

`git pull` does two things.  `git pull` simply does a `git fetch` followed immediately by `git merge`. This is often what we desire to do, but some people prefer to use git fetch followed by git merge to make sure they understand the changes they are merging into their branch before the merge happens.


