```yaml
number: 3446
title: "[Panic] index out of bounds in locator.rs"
type: issue
state: closed
author: krpatter-intc
labels:
  - bug
assignees: []
created_at: 2023-03-10T23:31:50Z
updated_at: 2023-03-11T21:01:16Z
url: https://github.com/astral-sh/ruff/issues/3446
synced_at: 2026-01-12T15:54:43Z
```

# [Panic] index out of bounds in locator.rs

---

_@krpatter-intc_

I've got a particular buggy repo where I end up getting a crash here:

thread '<unnamed>' panicked at 'index out of bounds: the len is 1 but the index is 184', crates\ruff\src\source_code\locator.rs:71:9

The exact file it fails on changes from run to run. 

I suspect _maybe_ I'm just getting too many errors (specifically: too many failing to parse files).  If I run each individual directory one at a time, then it works.

I tried setting the backtrace, but the output is too intermixed with the other warnings....Last thing I see that seems readable is:

0xerror7fffdd5655a0 - BaseThreadInitThunk:

`ruff --version` == 0.0.254

The TOML is setup with:

```
select = ["F","T"]
ignore = [
    "F401",
    "F841",
    "F403",
]
```

If this is too little info to help (without the actual files)...That I totally understand and feel free to reject/close this. Thanks for your time.

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->


---

_Comment by @charliermarsh on 2023-03-10 23:59_

Hmm, very interesting that you're having success when running over individual directories. These kinds of index out-of-bounds errors are _most_ common when run on files that use CR line endings (as opposed to LF and CRLF which are much more common). If you have a file that uses CR line endings, that _could_ be the culprit, and that's a known issue, but without a link to the repo it's a little hard to say.

---

_Comment by @qarmin on 2023-03-11 06:13_

Looks like duplicate of https://github.com/charliermarsh/ruff/issues/3425 and this may be fixed by https://github.com/charliermarsh/ruff/pull/3439/files (not tested yet)

---

_Comment by @krpatter-intc on 2023-03-11 16:12_

Mmm...yep, found the file and its definitely some sort of hidden character thing. It does seem to be related to line endings. Just saving the file in VScode is enough and when I do a beyond compare diff the only delta is the line ending style.

I have plenty of other files with CR/LF that it is fine with, so its a tad odd, but also seems like it is a known issue. Thanks for the quick responses. I'm fine with closing if this is a known issue.

---

_Comment by @krpatter-intc on 2023-03-11 16:14_

so for clarity sake most of the lines have something like:

![image](https://user-images.githubusercontent.com/62627988/224495218-a94c704a-5c39-4ffa-847b-e6fbf72f1e59.png)

And the `0D 0A` file is the one failing...If I save in VS code Beyond Compare shows just the 0D or 0A and those work...It could be the bad file has a mix that is throwing things off

but I admit, I didn't check _every_ line, maybe there is _one_ with just CR.

---

_Comment by @charliermarsh on 2023-03-11 16:19_

> I have plenty of other files with CR/LF that it is fine with.

👍 Just to be clear, CR/LF is totally fine. It's CR alone that isn't properly handled right now. (My understanding is that CR alone is very rare these days, as it was last used in early versions of Mac OS. I still want to support it properly, of course.)

---

_Label `bug` added by @charliermarsh on 2023-03-11 20:53_

---

_Closed by @krpatter-intc on 2023-03-11 21:01_

---
