# Summary Git in Depth Course

## About Git

**What is Git?**

Distributed Version Control System

**How Does Git Store Information?**

- Git save information like a key and value store
- The `Value = Data`
- The `Key = Hash of the Data`
- You can use the key to retrive the content

The `key` is SHA1 and it includes:

- Is a cryptographic hash function
- Given a price of data, it produces a 40 digits hexa number
- Value should always be the same if the given input the same

## Git with Blob Object

Git stores the file/data with *compressed* callit a blog, and with metadata:

1. The identifier blob
1. The size of the content
1. `\0` delimiter
1. content

Here's an example of how git creates a git-hash-object:

```bash
$ echo 'Hello, World!' | git hash-object --stdin

8ab686eafeb1f44702738c8b0f24f2567c36da6d // the result
```

Its same result when use openssl sha1 to generate a ssh key:

```bash
$ echo 'blob 14\0Hello, World!' | openssl sha1

8ab686eafeb1f44702738c8b0f24f2567c36da6d // the result
```

**Where git store it data?**

Git store its data in `.git` folder created while initialize git project with `git init`.

### Tree

Git informasi store in a tree.

**Tree** contains pointers (using SHA1)

- to blobs
- to other trees

And metadata:

- `type` of pointer (`blob` or `tree`)
- `filename` of directory name
- `mode` (executable file, symbolic link, ...

This is a sample illustration about tree and blob in git:

![](https://git-scm.com/book/en/v2/images/data-model-1.png)

> While have file with content is same/identical, but the file name/directory is dirrefent. Git only store content once.

### Optimizations

Git optimize other, like packagefiles, delta

- Git objects are compressed
- As files change, their contents remain mostly similar
- Git optimizes for this by compressing these files together, into a Packfile
- The Packfile stores the object, and "deltas" or the difference between one version of the file and the next.

- Packagefiles are generated when:
    - You have too many objets, during gc, or during push to a remote

## Git Commit

A Commit points to `a tree`

And in commit contains metadata:
- author and committer
- date
- message
- parent commit (one or more)

> The SHA1 of commit is the hash of all this information

**Git Commit can't cange** because:

- If you change any data about the commit, the commit will have a new SHA1 hash
- Even is the files don't change, the created date will

## Git Areas

There areas where code lives:

1. Working Area
1. Staging Area
1. Repository

### Working Area

- The files in your working area that are also not in the staging are not handled by git
- Also called **untracked files**

### The Staging Area

- What file are going to be part of the next commit
- The staging area is how git knows what will change between the current commit and next commit

Add file to next commit:

```sh
git add <file>
```

Remove file in the commit:

```sh
git rm <file>
```

Rename file in the commit:

```sh
git mv <file>
```

Add file to next commit with diff of changes:

```sh
git add -p
```

### Repository

- The files knows about!
- Contains all commits

## Git Stash

- Save uncommited work
- The stash is safe from destructive operations

**Basic Use Command**:

- Stash Changes: `git stash`
- List Changes: `git stash list`
- Show the Content: `git stash show stash@{0}`
- Apply latest stash: `git stash apply`
- Apply specifiic stash: `git stash apply stash@{0}`
- Stash with untracked files: `git stash --include-untracked`
- Keep all files: `git stash --all`
- Naming stashes for easy references: `git stash save "WIP: making progress on foo"`
- Remove & apply latest stash: `git stash pop`
- Remove latest stash: `git stash drop`
- Remove specific stash: `git stash drop stash@{0}`
- Remove all stash: `git stash clear`

## Three Type of Git References

1. Tags & Annotations Tags
1. Branches
1. HEAD

### Branch

- A branch is just a pointer to a particular commit
- The pointer of the current branch changes as new commit are made

### HEAD

Head is how git knows what branch you're currently on, and what the next parent will be.

HEAD is a pointer:

- Usually points at the name of the current branch
- But, it can point at a commit too (detached HEAD)

HEAD moves when:

- Make a commit in the currently active branch
- When checkout a new branch

To show current HEAD print out HEAD on .git folder:

```sh
cat .git/HEAD
```

### Lightweight Tags

Is a simple pointer a commit.

When create a tag with no arguments, it capture the value in HEAD.

Sample Create Tags

```sh
git tag my-first-commit
```

TO store additional information like: Author, message, date in git tags, use `git tag -a`. Example:

```sh
> git tag -a v1.0 -m "Version 1.0 of my blog"

> git tag
// v1.0

> git show v1.0 
// tag v1.0
// Tagger: Wisnu Ginanjar Saputra <wisnuwiry@gmail.com>
// Date:   Tue Nov 22 21:05:59 2022 +0700
// 
// Version 1.0 of my blog
```

- List all the tags in a repo: `git tag`
- List all tags, and what commit they're point to: `git show-ref --tags`
- List all the tags pointing at a commit: `git tag --points-at <commit>`
- Looking at the tag, or tagged contents: `git show <tag-name>`

**The Difference of Tags & Branches**

**Branches**: The current branch pointer moves with every commit to the repository

**Tags**: The commit that a tag points doesn't change & it's a snapshot

## Merging & Fast Forward

> Merge commit are just commit

### Fast Forward

```sh
> git checkout master
> git merge feature
// Updating .......
// Fast Forward
```

> Fast forward happens when there are no commits on the base branch that occured after the feature branch was created.

To retain the history of a merge commit, even if there are no changes to the base branch use `--no-ff` (No Fast Forward).
This will force a merge commit, event when one isn't necessary.

### Merge Conflict

- Attempt to merge, but files have diverged
- Git stops until the conflicts are resolved

### Git RERERE (Reuse, Recorded, Resolution)

- Git saves how you solve your conflicts
- Next conlict: reuse the sample resolution

This useful for:
- Long lived feature branch (like a refactor)
- rebasing

> To enable Git RERERE: `git config rerere.enabled true` and use `--global` flag to set enable for all projects

## History & Diff

### Good Commit are really important

- Git commit help preserve the history of code base
- They help with:
    - Debugging & troubleshooting
    - Creating release notes
    - Code review
    - rolling back
    - associating the code with an issue or ticket

**Anatomy og Good Commit:**
- Good commit message
- Encapsulates one logical idea
- Doesn't intruduce breaking changes ex: tests pass

### Git Log

Git log, is a basic command to show history of repository

Command: `git log`

Show git log with `since` flags Ex:

```sh
> git log --since="yesterday"
> git log --since="2 weeks ago"
```

Filtering history git log with `grep` flag ex:

```sh
git log --grep=mail --author=wisnu --since=2.weeks
```

To show log with filter selectively include/exclude files you have been:

- (A)dd
- (D)elete
- (M)odified
- (R)ename

Ex:

```sh
git log --diff-filter=M --stat
```

### Git Show & Diff

**Show**

Show commit and content history changed

- Show commit and content: `git show <commit>`
- Show files changed in commit: `git show <commit> --stat`
- Look at file from another commit: `git show <commit>:<file>`

**Diff**

Diff show changes of files:

- Between commits
- Between staging area and the repository
- what's in the working area

Show diff in unstage area: `git diff`
Show diff in staged area: `git diff --staged`

## Fixing Mistakes

### Git Clean

Clean will clear your working area by deleting untracked files.

Tips:

- Use `--dry-run` flag to see what would be deleted
- Use `-f` flag to do deletion
- Use `-d` flag will clean directory

### Git Reset

Reset is another command that perform different action depending on arguments:
- With path
- Without path

> By default git performed: `git reset --mixed`

Git reset have 3 main option: soft, mixin, hard

**--soft**: "Move HEAD & current branch"
**--mixed**: "Move HEAD & current branch" and "Reset the staging area"
**--hard**: "Move HEAD & current branch" and "Reset the staging area" and "Reset the working area" 

### Git Revert (The safe of reset)

Git revert creates a new commit that introduce the opposite changes from the specified commit.

The original commit stays in the repository.

> **Tips**: Use revert is you're undoing a commit that has already been shared. Revert doesn't change history.

## Excercies

1. [Excercise Commit](./excercise/excercise1-commit.md)
1. [Excercise Staging & Stashing](./excercise/excercise2-staging-and-stasing.md)
1. [Excercise Reference](./excercise/excercise3-reference.md)
1. [Excercise Merging](./excercise/excercise4-merging.md)
1. [Excercise History Diff](./excercise/excersise5-history-diff.md)
1. [Excercise Fix Mistake](./excercise/excercise6-fix-mistake.md)
1. [Excercise Rebase Amend](./excercise/excercise7-rebase-amend.md)
1. [Excercise Form & Remote](./excercise/excercise8-fork-remote.md.md)
1. [Excercise History Diff](./excercise/excercise9-advance-tool.md)
1. [Excercise Hooks](./excercise/excercise10-hooks.md)

## References

- [Git In Depth Course](https://frontendmasters.com/courses/git-in-depth)