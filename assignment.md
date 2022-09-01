# How are `git push`, `git fetch`, and `git pull` used?

In modern software development and documentation, git is most often used for source control. While git provides many commands and options, the following three basic operations are used repeatedly:

- [`git push`](#git-push): Sends changes from a local branch to a remote repo.
- [`git fetch`](#git-fetch): Receives information from a remote repo about the current state of its branches.
- [`git pull`](#git-pull): Receives information from a remote repo about the current branch, and updates your local branch so it matches the remote branch.

The following sections describe each of these operations in detail:

## `git push`

## `git fetch`

## `git pull`

Often `git push` and `git pull` are described as equivalent. This isn't entirely correct, since under the hood `git pull` does two things. `git push` takes our current branch, and checks to see whether or not there is a tracking branch for a remote repository connected to it. If so, our changes are taken from our branch and pushed to the remote branch. This is how code is shared with a remote repository, you can think of it as "make the remote branch resemble my local branch". This will fail if the remote branch has diverged from your local branch: if not all the commits in the remote branch are in your local branch. When this happens, your local branch needs to be synchronized with the remote branch with git pull or git fetch and git merge.`git fetch` again takes our current branch, and checks to see if there is a tracking branch. If so, it looks for changes in the remote branch, and pulls them into the tracking branch. It does not change your local branch. To do that, you'll need to do `git merge origin/master` (for the "master" branch) to merge those changes into your branch - probably also called "master".`git pull` simply does a `git fetch` followed immediately by `git merge`. This is often what we desire to do, but some people prefer to use git fetch followed by git merge to make sure they understand the changes they are merging into their branch before the merge happens.
