# git merge VS git push VS git fetch

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

TLDR: When you merge *your* branch in master, commits in your branch are squashed and applied as one commit, 
then this single commit is combined with last commit in master branch, and result is a new commit (H).

**git fetch** - 