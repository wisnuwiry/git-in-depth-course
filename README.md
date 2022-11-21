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

### Repository

- The files knows about!
- Contains all commits

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