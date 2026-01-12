```yaml
number: 13122
title: "Could ruff also check files according to file's shebang?"
type: issue
state: open
author: praiskup
labels:
  - cli
  - needs-design
assignees: []
created_at: 2024-08-27T13:13:26Z
updated_at: 2025-09-22T08:27:48Z
url: https://github.com/astral-sh/ruff/issues/13122
synced_at: 2026-01-12T15:54:52Z
```

# Could ruff also check files according to file's shebang?

---

_@praiskup_

```
$ ruff  check .
warning: No Python files found under the given path(s)
All checks passed!
$ head -1 vcs-diff-lint
#! /usr/bin/python3
$ ruff check vcs-diff-lint
All checks passed!
```

Even though the file is not `*.py`, it still is a Python file and it is worth checking.

---

_Comment by @dhruvmanila on 2024-08-28 05:59_

Related https://github.com/astral-sh/ruff/issues/2192

**tl;dr** It's recommended to use the `include` / `extend-include` config options to explicitly list down the files that you want Ruff to check for. Adding support for it _might_ come with a performance cost although not sure.

---

_Label `cli` added by @dhruvmanila on 2024-08-28 05:59_

---

_Label `wish` added by @dhruvmanila on 2024-08-28 05:59_

---

_Comment by @dhruvmanila on 2024-08-28 06:00_

Does using [`include`](https://docs.astral.sh/ruff/settings/#include) / [`extend-include`](https://docs.astral.sh/ruff/settings/#extend-include) solve your use case? If so, we can close this issue.

---

_Comment by @praiskup on 2024-08-28 06:54_

The differential analysis tool we develop uses Ruff as one of the backends, and while we appreciate Ruff's overall performance - analyzing the shebang is worth it in our case.

The goal is to make future project enablements (= "start using vcs-diff-lint + ruff" in more projects) as trivial as possible.  Explicitly asking for `include/extend-include` per/project raises the initial enablement barrier, and incomplete analysis is also not nice...

That said, having something like `analyze-shebangs = true` opt-in would be completely fine for our use case; we can afford to spend some time on this.

---

_Comment by @praiskup on 2024-08-28 07:22_

https://github.com/fedora-copr/vcs-diff-lint/pull/25 seems to help a bit here;  vcs-diff-lint knows the set of **changed** Python, including those that do not end with `*.py`.  So we can indeed use `extend-include` to at least cover those changed files, but we are still not checking unchanged non-`*.py` files. 

---

_Label `wish` removed by @MichaReiser on 2024-09-02 06:16_

---

_Label `needs-decision` added by @MichaReiser on 2024-09-02 06:16_

---

_Label `needs-decision` removed by @MichaReiser on 2024-09-02 06:18_

---

_Label `needs-design` added by @MichaReiser on 2024-09-02 06:18_

---

_Comment by @MichaReiser on 2024-09-02 06:19_

Detecting all Python files based on shebang has the downside that Ruff needs to read at least the first 80 or so bytes to determine whether the file should be included in the analysis. This is significantly more costly than just looking at a file extension, so I would not want this behavior to be the default. I'm open to adding such a feature if we can come up with an interface that doesn't penalty the default use case where this isn't needed.

---

_Comment by @praiskup on 2024-09-02 07:55_

> Detecting all Python files based on shebang has the downside that Ruff needs to read at least the first 80 or so

Could we, e.g., only include files with no "suffixes"?  That would be the most common case, I assume.

Otherwise, opt-in is completely fine!

---

_Comment by @pmp-p on 2024-10-12 05:05_

Also some html files - though valid python - have a non standard shebang (like it used to be on MS-DOS) that Ruff cannot yet support/handle ( unlike black -x / python -x , fails with `SyntaxError: Expected a statement` ) 

eg source : https://github.com/pygame-web/showroom/blob/main/pygame-scripts/org.pygame.touchpong.html

for run with python-wasm or `python3 -x` : https://pygame-web.github.io/showroom/pygame-scripts/org.pygame.touchpong.html

ref: https://github.com/psf/black/issues/3214

---

_Comment by @praiskup on 2024-12-06 13:40_

> Also some html files - though valid python - have a non standard shebang

Indeed, this should be only an additional/optional analysis, and I don't think we need to have 100% success rate (nb. the basic `.py` suffix rule isn't 100% correct either).

---

_Comment by @daethnir on 2025-09-21 04:48_

Adding another +1 to supporting shebang lines.

Our executable scripts do not end in `.py` and have `chmod +`. In most cases they are `#!/usr/bin/env python3`.

However in some cases we have wrappers ala `#!/usr/bin/env -S uvwrapper ...` that automatically run the tool through `uv` - so supporting user-specifiable shebang lines would be even better.



---

_Comment by @MichaReiser on 2025-09-22 08:27_

@daethnir I don't know how many executable scripts you have but if there are only a few, does the solution outlined by @dhruvmanila might work for you in the meantime:

> tl;dr It's recommended to use the include / extend-include config options to explicitly list down the files that you want Ruff to check for. Adding support for it might come with a performance cost although not sure.


---
