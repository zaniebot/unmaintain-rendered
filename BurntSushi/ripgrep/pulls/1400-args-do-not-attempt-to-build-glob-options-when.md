```yaml
number: 1400
title: "args: Do not attempt to build glob options when not specified."
type: pull_request
state: closed
author: JesseH-
labels:
  - rollup
assignees: []
base: master
head: issue-1291
created_at: 2019-10-08T03:24:05Z
updated_at: 2020-02-17T22:16:36Z
url: https://github.com/BurntSushi/ripgrep/pull/1400
synced_at: 2026-01-12T18:23:13Z
```

# args: Do not attempt to build glob options when not specified.

---

_@JesseH-_

Closes #1291 

This is a first shot at addressing #1291, though I think some additional input is needed.

After reviewing and manually testing, I found that the source of the errors being raised was due to unconditionally building an `OverrideBuilder` from the globbing options. As this relies on the current working directory, it understandably fails when the cwd does not exist.

The immediate error can be suppressed by checking that the globbing options are actually required before attempting to handle them. This will still raise the same error when any of these options are present without an existing cwd, though perhaps a clearer error message could also be emitted as part of this change in that case.

On the testing front, I wasn't able to suitably recreate the original issue in a regression test. In my experiments, I tried to set the `current_dir` of the `TestCommand` to something non-existent before running, but this seemed to result in a panic before actually entering any ripgrep code when we go to execute the command.

Manual testing showed that we were able to execute non-globbing ripgrep commands without failure from a non-existent cwd with this change applied.

---

_Label `rollup` added by @BurntSushi on 2020-02-17 02:06_

---

_Comment by @BurntSushi on 2020-02-17 02:08_

Thank you very much for this PR! I ended up going in a different direction. In particular:

* I created a new `current_dir` helper function. If `std::env::curent_dir` fails, then ripgrep will try to read `PWD`. If that fails, then ripgrep will print a more reasonable error message.
* Similarish to what you did, if ripgrep does not need to build the glob matchers, then it just creates an empty `Override` matcher.

While I didn't end up using your PR, your diagnosis of the situation was super helpful. Likely saved me quite a bit of time! So thank you!

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---
