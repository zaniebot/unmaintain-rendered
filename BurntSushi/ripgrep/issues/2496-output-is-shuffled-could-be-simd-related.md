```yaml
number: 2496
title: Output is shuffled (Could be SIMD-related?)
type: issue
state: closed
author: imshvc
labels: []
assignees: []
created_at: 2023-04-22T18:33:54Z
updated_at: 2023-07-13T01:59:17Z
url: https://github.com/BurntSushi/ripgrep/issues/2496
synced_at: 2026-01-12T16:13:24Z
```

# Output is shuffled (Could be SIMD-related?)

---

_@imshvc_

#### What version of ripgrep are you using?

13.0.0

#### How did you install ripgrep?

From GitHub Release via `dpkg` 

#### What operating system are you using ripgrep on?

```
$ cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=22.04
DISTRIB_CODENAME=jammy
DISTRIB_DESCRIPTION="Ubuntu 22.04.2 LTS"
```

#### Describe your bug.

The bug occurs when traversing multiple directories of source code.

On each initial run the results returned in the STDOUT appear "shuffled" and thus trying to `diff` from one iteration to another is not possible.

I use [theZiz/aha](https://github.com/theZiz/aha) to redirect STDOUT to a HTML file for later inspection and that's how I noticed this behavior.

https://user-images.githubusercontent.com/102489121/233800755-eb6354ba-3154-4763-8bb0-a4fdbe70d133.mp4

#### What are the steps to reproduce the behavior?

Go to my repository [oxou/repro-bug-ripgrep](https://github.com/oxou/repro-bug-ripgrep) there I've provided easy steps to reproduce the bug.

#### What is the actual behavior?

It appears as if multiple threads are running and depending on which finishes first the output is not identical to the previous iteration.

#### What is the expected behavior?

The output should always be the same.


---

_Comment by @imshvc on 2023-04-22 19:11_

After looking through the source code, I've realized there's a flag you can pass `--sort path` that solves this issue.


---

_Closed by @imshvc on 2023-04-22 19:11_

---

_Comment by @BurntSushi on 2023-04-23 11:50_

SIMD certainly has nothing to do with it. SIMD doesn't usually result in out-of-order things. The underlying reason for the shuffled output is two-fold:

1. The order of results when traversing a directory tree is usually unspecified from the operating system itself. So unless you explicitly sort things, the result order of _any_ directory traversal isn't guaranteed to remain the same. (In practice, it doesn't shuffle too much.)
2. ripgrep traverses the directory tree in parallel, processing each file in an undetermined order. ripgrep just prints things out as it is done with them.

You are correct that `--sort path` is what you want if you want a consistent ordering. Note though that it disables parallelism and so may be non-trivially slower.

---

_Comment by @craftyguy on 2023-07-13 01:51_

> You are correct that --sort path is what you want if you want a consistent ordering. Note though that it disables parallelism and so may be non-trivially slower.

Why do you have to disable parallelism in order to sort a set of strings?

---

_Comment by @BurntSushi on 2023-07-13 01:56_

> > You are correct that --sort path is what you want if you want a consistent ordering. Note though that it disables parallelism and so may be non-trivially slower.
> 
> Why do you have to disable parallelism in order to sort a set of strings?

You don't. Never said that. What I said is that ripgrep disables parallelism when asked to sort. This is an implementation problem, not a requirement of the problem itself.

And the problem is far more complicated than "set of strings." Those are your words, not mine.

---
