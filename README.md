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

### Optimizaitons

Git optimize other, like packagefiles, delta

- Git objects are compressed
- As files change, their contents remain mostly similar
- Git optimizes for this by compressing these files together, into a Packfile
- The Packfile stores the object, and "deltas" or the difference between one version of the file and the next.

- Packagefiles are generated when:
    - You have too many objets, during gc, or during push to a remote

## Git Commit