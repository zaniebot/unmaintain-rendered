```yaml
number: 1796
title: Example 01b_quick_examples error
type: issue
state: closed
author: OAyomide
labels:
  - A-docs
assignees: []
created_at: 2020-04-08T17:52:52Z
updated_at: 2020-04-08T18:08:34Z
url: https://github.com/clap-rs/clap/issues/1796
synced_at: 2026-01-12T16:14:11Z
```

# Example 01b_quick_examples error

---

_@OAyomide_

### Clap version
2.33.0

### Where
https://github.com/clap-rs/clap/blob/master/examples/01b_quick_example.rs

### What's wrong
On line 40 and 53, the "short" examples were written as chars instead of strings. I tried just copy/pasting to try the code and I got compile errors.

I can make a PR for this. Unless I am missing something.
### How to fix
I changed to a string (double quotes) to get it to run.


---

_Label `C: docs` added by @OAyomide on 2020-04-08 17:52_

---

_Comment by @CreepySkeleton on 2020-04-08 18:08_

You see, the master branch is the development branch. Currently we're striving for 3.x release which won't be backward compatible with 2.x; the difference you outlined is one of the changes.

If you want examples for 2.x, you should checkout the appropriate tag, like this:

![image](https://user-images.githubusercontent.com/50968528/78818267-124d2380-79dd-11ea-8843-09b5a16616f8.png)


---

_Closed by @CreepySkeleton on 2020-04-08 18:08_

---
