# Submodules in Git

When one repository requires files in another repository, access the required files by using Git's submodule capabilities. When you create a submodule in a Git repo, the submodule is actually a separate repo, so you have a *repo inside a repo*. You would use this functionality if you have one project that references another project -- for instance, multiple projects which utilize the same CSS or the same code from one module. This topic covers:

- [Add a submodule to an existing repo](#add-a-submodule-to-an-existing-repo)
- [Commit new submodule in parent repo](#commit-new-submodule-in-parent-repo)
- [Clone a repo containing a submodule](#clone-a-repo-containing-a-submodule)
    - [Pull a submodule during a clone](#pull-a-submodule-during-a-clone)
    - [Pull a submodule after a clone](#pull-a-submodule-after-a-clone)
- [Update submodule with upstream changes](#update-submodule-with-upstream-changes)
    - [Update all submodules from default branches](#update-all-submodules-from-default-branches)
    - [Update one submodule from default branch](#update-one-submodule-from-default-branch)
    - [Update one submodule from specified branch](#update-one-submodule-from-specified-branch)
    - [Configure branch and merge behavior for submodule updates](#configure-branch-and-merge-behavior-for-submodule-updates)
    - [Add and commit changes](#add-and-commit-changes)
- [Update local repo with submodule changes in remote copy of parent](#update-local-repo-with-submodule-changes-in-remote-copy-of-parent)
- [Edit a submodule](#edit-a-submodule)
## Add a submodule to an existing repo

If your current repo does not yet contain the dependent submodule, first add it. The repo that will contain the submodule is the *parent repo*.

```bash
git submodule add [--branch <branch>] <url of dependent repo>
```

The following output shows adding the `ClarityEdits` branch of the `Assignment` repo located at `https://github.com/joshwriter/Assignment.git`.

```
$ git submodule add -b ClarityEdits https://github.com/joshwriter/Assignment.git
Cloning into 'C:/Website/TechWritingSamples/Assignment'...
remote: Enumerating objects: 45, done.
remote: Counting objects: 100% (45/45), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 45 (delta 14), reused 45 (delta 14), pack-reused 0
Receiving objects: 100% (45/45), 9.23 KiB | 858.00 KiB/s, done.
Resolving deltas: 100% (14/14), done.
```

Next, [commit the new submodule in the parent repo](#commit-new-submodule-in-parent-repo).

## Commit new submodule in parent repo

Immediately after adding a submodule to a repo, the files in the submodule are not available. 

```bash
$ git status
On branch LearnVim
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   ../.gitmodules
        new file:   Assignment
```

To have the files present in the repo for use, add and commit the `.gitmodules` file and the new directory.

```bash
$ git add ../.gitmodules

$ git add Assignment/

$ git status
On branch LearnVim
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   ../.gitmodules
        new file:   Assignment

$ git commit -m "Added submodule of HashiCorp writing assignment, which is an introduction to git."
[LearnVim 4229105] Added submodule of HashiCorp writing assignment, which is an introduction to git.
 2 files changed, 5 insertions(+)
 create mode 100644 .gitmodules
 create mode 160000 TechWritingSamples/Assignment
```

Now the files in the submodule are ready for use.

## Clone a repo containing a submodule

When you clone a repo containing a submodule, the submodule in the parent repo is not by default available for use after cloning. To get the files, you must run additional commands. (See [Pull a submodule after a clone](#pull-a-submodule-after-a-clone).) Avoid this issue by passing arguments to `git clone`. (See [Pull a submodule during a clone](#pull-a-submodule-during-a-clone).)

## Pull a submodule during a clone

The simplest way of using a submodule referenced in a remote repo is to clone it along with the parent repo, so that the submodule is immediately available.

```bash
git clone --recurse-submodules <url of parent repo>
```

## Pull a submodule after a clone

If you did not pull a submodule during cloning, you can pull it afterwards by using the following syntax:

```
git submodule update --init --recursive
```

## Update submodule with upstream changes

Submodules are not automatically updated with changes from their upstream remotes. To perform that update, do the following:

1. [Configure branch and merge behavior for submodule updates](#configure-branch-and-merge-behavior-for-submodule-updates).
2. Update the submodule from its remote.
3. Add and commit the submodule changes in the *parent repo*.
4. Push the changes to the parent's remote.

This part of the workflow is useful for an admin who updates `main` in repos that use the submodule. 

**<span style="color:red">WARNING:</span>** This is not the same action as updating a parent repo from its remote, when the parent's remote has updates to its submodules. The action we're discussing in this section is someone updating a *submodule from the submodule's remote.* After performing the actions in this section, **others who pull from the parent repo need to perform another command besides `git pull` to update their submodules.** (For instructions on that, see [Update local repo with submodule changes in remote copy of parent](#update-local-repo-with-submodule-changes-in-remote-copy-of-parent).

When updating submodules from their remotes, you have numerous options, including:

- [Update all submodules from default branches](#update-all-submodules-from-default-branches).
- [Update one submodule from default branch](#update-one-submodule-from-default-branch).
- [Update one submodule from specified branch](#update-one-submodule-from-specified-branch).

### Update all submodules from default branches

To update all submodules with the latest changes to the remote repos, run the following from the parent repo, not the submodule repo:

```bash
git submodule update --remote
```

Now [add and commit the changes](#add-and-commit-changes).

### Update one submodule from default branch

To update just one submodule, run:

```bash
git submodule update --remote <name of submodule>
```

If you don't know the name of the submodule, see the `.gitmodules` file. Now [add and commit the changes](#add-and-commit-changes).

### Update one submodule from specified branch

If you don't want to specify the name of the submodule, you can configure the default name (and default branch of the submodule repo) once and then just run `git submodule update --remote`.

```
git config -f .gitmodules submodule.<remote name>.branch <branch name>
git submodule update --remote
```

The first line configures the default submodule and branch to pull, so you do not need to specify them each time. (You only need to run that line the first time you run this operation. ) As an example, to update the submodule tied to the `ClarityEdits` branch (which is not the default branch) of the  `Assignment` repo, run the following:

```bash
$ git config -f .gitmodules submodule.Assignment.branch ClarityEdits
$ git submodule update --remote
remote: Enumerating objects: 5, done.
remote: Counting objects: 100% (5/5), done.
remote: Compressing objects: 100% (1/1), done.
remote: Total 3 (delta 1), reused 3 (delta 1), pack-reused 0
Unpacking objects: 100% (3/3), 287 bytes | 19.00 KiB/s, done.
From https://github.com/joshwriter/Assignment
   4ea90de..4f812b5  ClarityEdits -> origin/ClarityEdits
Submodule path 'Assignment': checked out '4f812b5a1b9f54a2f88e9bca43de36b7b37de614'
```

(In the future, since you have run the `git config` command, only run the `git submodule` command during updates.) Now run `git status` from that same directory, and notice that there are new commits in the submodule.

```bash
$ git status
On branch LearnVim
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   Assignment (new commits)
no changes added to commit (use "git add" and/or "git commit -a")
```

Now [add and commit the changes](#add-and-commit-changes).

### Configure branch and merge behavior for submodule updates

Git may default to trying to pull from `master` instead of `main`, which leads to an error, if there is no `master` branch on the upstream remote of the submodule. Additionally, you'll probably get in a detached HEAD mode after updating, unless you configure `merge` behavior. Here's an example of a `.gitmodules` file that configures the branch to pull from and the merge behavior, so no errors or detached HEAD state occurs after a submodule update:

```bash
[submodule "Content/Resources"]                                           
        path = Content/Resources
        url = git@github.com:Documentation/FlareGUI.git               branch = main
        update = merge 
```

You can configure your submodule to pull from any valid branch on the submodule's remote (if there are others besides `main`. Additionally, you can set the merge behavior to `rebase`, if you like.)

### Add and commit changes

Now `git add` the changes to the parent repository by using `git add` with the directory containing the submodule.

 ```bash
$ git add Assignment/

$ git status
On branch LearnVim
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   Assignment

 ```

Next, commit the changes.

```bash
$ git commit -m "Pulled in latest changes."
[LearnVim 37d375e] Pulled in latest changes.
 1 file changed, 1 insertion(+), 1 deletion(-)
```

## Update local repo with submodule changes in remote copy of parent

After someone updates the submodules in the remote copy of the parent repo, running `git pull` with no options does not automatically update the parent's submodules on a local machine. You can, however, do this by running a modified pull:

```bash
git pull recurse-submodules
```

For members of the team who are pulling using GitHub desktop or some other GUI utility, this is not available in the GUI. Instead, after they execute their pull, they need to open a terminal within the GUI and run

```bash
git submodule update --recursive
```

No additional operations or commits are needed at this point. This just updates the working directory of the submodules. The commits are already in the parent project. I need to instruct tech. writers to run this command by opening a terminal window from the **Repository** menu. I can automate this for tech. writers by authoring a post-merge Git hook and telling them where to install it.

## Edit a submodule

To edit a submodule and push its changes back to the parent repo's remote, use the following workflow:

1.  From inside your submodule, create a branch, commit, and merge your work into the default branch. (Or work directly off of the default branch.)
3. Move into the parent repo.
4. Optionally update the submodule with updates from its remote, by running `git submodule update --remote --merge`.
5. `cd` into the parent repo.
6. Commit anything necessary in the parent, including changes to the submodule.
7. Update the remote of the parent using `git push --recurse-submodules=on-demand`

