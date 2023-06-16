# Josh's introduction to Git

In modern software development and documentation, the version control system that is most often used for source control is Git. This system is based on two concepts: repositories and branches. A *repository*, also known as a *repo*, is a collection of files in a directory and its subdirectories, recursively.

A *branch* is a version of the repo files that can be edited completely independently of other versions of those files. A repo holds one or more branches. Often, though, it contains many branches, allowing different users to edit, delete, and add to different versions of the files in the repo at the same time, without overwriting other users’ work.

Since each developer (and writer) contains a complete copy of the repo on their local machine (a *local repo*), they edit the *local branches* in these repos without requiring network access. When a developer is ready to share their work with other users, they upload their local branch to a repo accessible by the other users, the *remote repo*. Developers frequently download and upload work from the remote repo. In this way, different developers can work independently, sharing their work when it is ready to be used by others.

## Commits: the building blocks of Git

As mentioned previously, repos contain branches, which are different versions of the files in the repo. Git stores the information about branches in *commits*. The term "commit" has two meanings. It refers both to the object that is created as well as the action that the user takes to create that object. In other words, when a user alters the files in a branch and then wants to save those changes in the Git repo, the user *commits* those changes. This results in an object known as a commit, which contains the list of files changed, and the exact changes to those files since they were last committed.

Note that last sentence. While the difference between storing an edited file vs. storing the edits to a file may sound like a trivial difference, it becomes extremely important when using commits in more powerful ways, such as re-ordering them or even applying a commit to the repo at a different point in time or to a different branch, in order to re-create the effects of the commit. (Re-creating the effects of a commit means reproducing the edits it contains to the list of files it references.)

The changes from one commit to another are collectively referred to as the *diff* between those two commits, and Git can compute and show you the diff between any two commits in a repository -- not just a commit that occurred directly after another commit. By viewing diffs, you can see how your project has evolved from any point to any other point in time.

Each commit is based on one or more previous commits. When a commit is based on a single commit, the latest commit updates a branch that contains the previous commit. When the commit is based on multiple commits, this is usually the result of two branches [merging](#git-merge). (More than two branches can be merged at once, but this is less typically done. For this discussion, we assume that all commits either result from one previous commit or are the result of two commits merging together.)

While we often use branches, because they are usually easier to refer to than referring to individual commits, each branch is fundamentally a collection of ordered commits, where each commit is based off of either one or two ancestor commits. Git computes the diff from the very first commit in the branch to the the latest commit in the branch, by adding up the individual diffs (that is, changes) from each commit, from the first to the last. Git uses this information to show you the latest version of your files, but this approach also allows for tremendously powerful workflows, including retrieving any previous version of any file in the repo -- even files that have been deleted years ago.

## Basic operations

While git provides many commands and options, you will use the following operations repeatedly:

- [`git push`](#git-push): Uploads changes from a local branch to an upstream branch.
- [`git fetch`](#git-fetch): Downloads information from a remote repo about the current state of its branches.
- [`git merge`](#git-merge): Inserts material from one branch into another branch.
- [`git pull`](#git-pull): Performs `git fetch` and then performs a `git merge` from the upstream branch into your local branch.

The following sections briefly describe each of these operations. For more detailed options for each of these operations, please see the [official git documentation online](https://git-scm.com/doc). Also, note that in the following sections, we assume that you have set up your local branches to track upstream branches. For more information on setting up tracking branches, see [this page](https://www.git-tower.com/learn/git/faq/track-remote-upstream-branch/).

### `git push`

`git push` uploads changes from your local branch to its upstream branch so that they are identical. Since the upstream branch is in the remote repo (which other users have access to), using `git push` shares your work with everyone who has access to the remote repo. Note that this operation requires your local branch to contain all of the commits that the remote branch contains *before* running `git push`, and will fail if this condition is not met. If the remote branch contains commits that your local branch does not yet contain, you have two options: (a) first run [`git pull`](#git-pull), or (b) first run [`git fetch`](#git-fetch) followed by [`git merge`](#git-merge). After doing (a) or (b), you can run `git push`.

**Syntax:** To upload changes from your local branch `hotfix` to the upstream branch `hotfix` in the remote repo named `origin`, run

```
git push origin hotfix
```

### `git fetch`

`git fetch` downloads information about the current state of the remote repository, including all of the new commits and branches it contains. This operation is always safe to run, however, because it does not *change* your local branch. To update your local branch with changes in the upstream branch, first run `git fetch` and then run [`git merge`](#git-merge).

**Syntax:** To download information from the remote repository named `origin`, run

```
git fetch origin
```

### `git merge`

`git merge` compares two branches and updates one of them with changes that the other one contains. This operation can be used to update one branch with information from any other branch, including upstream branches and local branches. 

**Syntax:** To update the local branch `master` with changes in the local branch `hotfix`, first switch to `master` by running

```
git switch master
```

and then execute the merge by running

```
git merge hotfix
```

### `git pull`

Frequently you will want to perform a a [`git fetch`](#git-fetch) immediately followed by a [`git merge`](#git-merge), which is exactly what `git pull` does. The advange of first performing a `git fetch` and *then* performing a `git merge` is that you can run `git status` between those two operations to determine whether or not running `git merge` will result in a merge conflict. (A merge conflict occurs during `git merge` when git cannot automatically integrate the changes introduced by the merge because the two branches contain commits that conflict with one another. When a merge conflict occurs, you must instruct git how to [resolve the conflict](https://git-scm.com/docs/git-merge#_how_to_resolve_conflicts).)

**Syntax:** To update the local branch `master` with information from the upstream branch `master` in the remote repo named `origin`, first switch to `master` by running

```
git switch master
```
and then execute the pull by running

```
git pull origin master
```

## Further information

This introduction to git covered several basic git commands, but there are many others, as well as many options for each command. By leveraging additional commands and options, git becomes not just a version control system but a set of tools that assists in your work. For instance, git allows you to re-order your work so that it is more understandable to you and your peers. The system also allows you to do work that you need temporarily -- for instance, importing test files -- and later undo that work by undoing a commit. To fully understand git, it's best if you first take the time to understand the [three trees](https://git-scm.com/book/en/v2/Git-Tools-Reset-Demystified#the-three-trees). Once you have a solid understanding of that topic, you may be interested in learning about these additional git commands:

- `git diff`
- `git switch`
- `git stash`
- `git restore`
- `git revert`
- `git reset`
- `git rebase`

You can find information on all of these commands as well as all other git commands, at [https://git-scm.com/doc](https://git-scm.com/doc). 
