# Resources for a basic barebone git tutorial

## Basic git commands

Clone a repository onto your computer from a remote:
```
git clone https://github.com/giuliomoro/git-tutorial
```

View your local changes. To see what files have changed:
```
git status
```
to see what changes have been made to those files:
```
git diff
```

Save your local changes:
```
git commit ...
```

Send your commits to the remote:
```
git push
```

Get commits from the remote:
```
git pull
```

## Quick glossary

- _repository_ (_repo_): a virtual storage for your project, which allows to save and inspect different versions of the code (e.g.: a folder on your computer, or on a server)
- _commit_: a set of changes to the files in the repo, complete with a date, author and message. This is normally represented by a long hex hash (e.g.: c693597d37a6b0c5ef66c40969e745c70d0c0d53)
- _branch_: one independent line of development. It also doubles up as a named pointer to a commit which is always attached to the most recent commit. For instance `master`.
- _remote_: a git repository that is connected to our local git repository but lives somewhere else (e.g.: in another folder, on another computer, on a server)
- _working tree_: the current content of the repo folder, including the files from the last commit and any modifications you have made on them.
- _index_: a staging area where changes are added in preparation for a commit

## How did we get here?

```
git log
```

show changes for each revision:
```
git log --patch
```

show stats (files changed, number of line changes) for each revision:

```
git log --stat
```

## Temporarily undo my changes but do not throw them away

This way you can test whether it was working before you made those changes.

Stash them:
```
git stash
```
re-apply them:
```
git stash apply
```

## Branches

```
git log --graph --oneline --decorate --all
```

```
git log --graph --oneline --decorate --all --color=always | sed "s/\(.\{,200\}\).*/\1/" | less -r
```

## Joining branches

### Merging

(from `git merge --help`)

```
       Assume the following history exists and the current branch is "master":

                     A---B---C topic
                    /
               D---E---F---G master

       Then "git merge topic" will replay the changes made on the topic branch since it diverged from master (i.e., E) until its current commit (C) on top of
       master, and record the result in a new commit along with the names of the two parent commits and a log message from the user describing the changes.

                     A---B---C topic
                    /         \
               D---E---F---G---H master
```

### Rebasing

(from `git rebase --help`)

```
       Assume the following history exists and the current branch is "topic":

                     A---B---C topic
                    /
               D---E---F---G master

       From this point, the result of either of the following commands:

           git rebase master
           git rebase master topic

       would be:

                             A'--B'--C' topic
                            /
               D---E---F---G master
```

Note that in the final form, we have `A'`, `B'` and `C'`, which are commits that have the same content as `A`, `B`, and `C`, respectively, but different metadata (because the parent commit of `A` is `E`, and the parent commit of `A'` is `G`): we have rewritten history for the `topic` branch.

### Pull

`git pull` vs `git pull --rebase`

### Conflicts

When both branches have changed the same lines of a file, a `merge` or `rebase` operation will cause conflicts. These are highlighted as such in the code, and need to be fixed before the operation can complete. This can be done by manually editing the conflicting files and/or using dedicated tools, such as `vimdiff` or `FileMerge`

External link: [use FileMerge as mergetool](https://gist.github.com/bkeating/329690)

## Deleting stuff

You don't (but there are plenty of ways to do it).

## Bela workflow
(or, more in general: how to make your repo talk to the internet when it has no internet connection, but is connected to something that has an internet connection)

### Setup:

#### Bela (computer without internet access):

Use the GUI to create and commit

#### Hosting service

On your favourite git hosting service (e.g.: github.com), create a new repo and grab its URL

#### On the host computer

```
mkdir myproject
cd myproject
git init
git remote add board root@bela.local:Bela/projects/myproject
git remote add origin git@github.com:giuliomoro/myproject
git pull board master
```

### Every time you want to synchronize

#### Push from the board to an internet-connected remote

Run this from the host:
```
git pull board master
git push origin master
```

### Pull from an internet-connected remote to the board

Run this on the host:
```
git pull origin master
git push board master:tmp
```
then get onto the board:
```
ssh root@bela.local
cd Bela/projects/myproject
git merge tmp
git branch -D tmp
```

## Who, why and when did (I) do this, again?

```
git blame
```

## When did i break this?

```
git bisect
```

## I hate typing my username and password for every push

- set up ssh keys: https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh
- make sure you always use the `git@github.com:YourUser/YourRepo.git` format for the remote instead of `https://github.com/YourUser/YourRepo`. For an already existing repo, you can do:

```
git remote set-url origin git@github.com:YourUser/YourRepo.git
```
