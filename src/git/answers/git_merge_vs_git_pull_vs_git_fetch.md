# git merge VS git pull VS git fetch

## Merge
**git merge** - command for joining one or more histories together.

```
git merge [-n] [--stat] [--no-commit] [--squash] [--[no-]edit]
	[--no-verify] [-s <strategy>] [-X <strategy-option>] [-S[<keyid>]]
	[--[no-]allow-unrelated-histories]
	[--[no-]rerere-autoupdate] [-m <msg>] [-F <file>] [<commit>…​]
git merge (--continue | --abort | --quit)
```

Includes changes from the named commits (since their histories were diverged from the current branch) 
into the current branch. This command is used be the `git pull` to incorporate (включить) changes from another 
repository and can be used by hand to merge changes from one branch to another.

Assume history in two branches, "master" is the current branch:
```
	  A---B---C topic
	 /
    D---E---F---G master

```
Then "git merge topic" will replay the changes made on 
the `topic` branch since it diverged from `master` (i.e., `E`) until 
its current commit (`C`) on top of master, and record the result in a new commit along with 
the names of the two parent commits and a log message from the user describing the changes.
```
	  A---B---C topic
	 /         \
    D---E---F---G---H master
```

###TLDR 
When you merge *your* branch in master, a new commit is created in master 
that points both on your branch and master.
All commits from your branch are checked (replayed) to prevent conflicts.
Merge commit has two parents.
Your branch stays unchanged, merge commit is added to the current (result branch) - master.

Warning: Running git merge with non-trivial uncommitted changes is discouraged: while possible, 
it may leave you in a state that is hard to back out of in the case of a conflict.

## Fetch
**git fetch** - downloads objects and refs from another repository.
Fetch branches and/or tags (collectively, "refs") from one or more other repositories, 
along with the objects necessary to complete their histories. 

By default, any tag that points into the histories being fetched is also fetched; 
the effect is to fetch tags that point at branches that you are interested in. 
This default behavior can be changed by using the `--tags` or `--no-tags options` 
or by configuring remote.<name>.tagOpt. By using a refspec that fetches tags explicitly, 
you can fetch tags that do not point into branches you are interested in as well.

When no remote is specified, by default the origin remote will be used, unless there’s an upstream branch configured for the current branch.

The names of refs that are fetched, together with the object names they point at, 
are written to `.git/FETCH_HEAD`. 
This information may be used by scripts or other git commands, such as `git-pull`.

## Pull
**git pull** - fetches from and integrates with another repository or a local branch.

Includes changes from a remote repository into the current branch. 

**IMPORTANT:**
`git pull` is a shorthand for `git fetch`+`git merge FETCH_HEAD` (it default mode of this command).

More precisely, git pull runs git fetch with the given parameters and calls git merge to merge the retrieved branch 
heads into the current branch. With --rebase, it runs git rebase instead of git merge.

[merge strategies - RU](https://www.atlassian.com/ru/git/tutorials/using-branches/merge-strategy)

# git merge VS git rebase

## rebase
*git rebase** - Reapplies commits on top of another branch.

Assume the following history exists and the current branch is "topic":
```
          A---B---C topic
         /
    D---E---F---G master
```

From this point, the result of either of the following commands:

```git rebase master
git rebase master topic
```

would be:
```
                  A'--B'--C' topic
                 /
    D---E---F---G master
```

### TLDR
git rebase squashes commits from current branch 
to a single commit and puts it on the top of another branch.

So, `git merge` saves another's branch history, creates new commits that point on two parent branches.
At the same time `git rebase` squashes branch history and creates one branch with new single commit on top of second branch.