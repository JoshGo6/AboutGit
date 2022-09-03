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

`git push` takes our current branch, and checks to see whether or not there is a tracking branch for a remote repository connected to it. If so, our changes are taken from our branch and pushed to the remote branch. This is how code is shared with a remote repository, you can think of it as "make the remote branch resemble my local branch". This will fail if the remote branch has diverged from your local branch: if not all the commits in the remote branch are in your local branch. When this happens, your local branch needs to be synchronized with the remote branch with git pull or git fetch and git merge.

## `git fetch`

`git fetch` again takes our current branch, and checks to see if there is a tracking branch. If so, it looks for changes in the remote branch, and pulls them into the tracking branch. It does not change your local branch. To do that, you'll need to do `git merge origin/master` (for the "master" branch) to merge those changes into your branch - probably also called "master".

## `git merge`



## `git pull`

`git pull` does two things.  `git pull` simply does a `git fetch` followed immediately by `git merge`. This is often what we desire to do, but some people prefer to use git fetch followed by git merge to make sure they understand the changes they are merging into their branch before the merge happens.


