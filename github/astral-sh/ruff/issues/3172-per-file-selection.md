---
number: 3172
title: Per file selection
type: issue
state: closed
author: henryiii
labels:
  - configuration
  - core
  - help wanted
assignees: []
created_at: 2023-02-23T15:42:56Z
updated_at: 2024-04-10T19:20:24Z
url: https://github.com/astral-sh/ruff/issues/3172
synced_at: 2026-01-07T13:12:14-06:00
---

# Per file selection

---

_Issue opened by @henryiii on 2023-02-23 15:42_

I'd like a way to activate a rule for a subset of files; basically the inverse of per-file-ignores. In my case, I'd like to activate pydocstyle on the main module, but not tests, examples, docs, benchmarks, support files, setup.py, etc.

Current and possible options:

* Use a second ruff.toml/pyproject.toml inside the package. This is not ideal, as I don't want to add extra files (especially pyproject.toml's, as those should only be at the root of packages). This is the only one in the list that would work today (that I know of). `.ruff.toml` (#2988) would help a little, but still not my preferred solution for this case - I'd like all configuration in a single file.
* Add `tool.ruff.per-file-extend-select` (technically, `per-file-ignores` is also extend, so maybe for symmetry it could be `tool.ruff.per-file-extend-select`). This would then be `"src/**.py" = ["D"]`.
* Support negation in `tool.ruff.per-file-ignores` like a gitignore. So `"!src/**.py" = ["D"]` would ignore D on anything except `src/**.py` files.

Thoughts?

Here's what I would have to put without this:

```toml
[tool.ruff.per-file-ignores]
"doc/**.py" = ["D"]
"tests/**.py" = ["D"]
"bench/**.py" = ["D"]
".ci/**.py" = ["D"]
"cmake_ext.py" = ["D"]
"version.py" = ["D"]
"setup.py" = ["D"]
```

---

_Comment by @charliermarsh on 2023-02-23 18:02_

I suspect that the last option would be much easier to support than `per-file-extend-select`. I'm open to it!


---

_Label `configuration` added by @charliermarsh on 2023-02-23 18:02_

---

_Label `core` added by @charliermarsh on 2023-02-23 18:02_

---

_Comment by @charliermarsh on 2023-02-23 18:04_

(The way `per-file-ignores` work right now is that we still run the ignored rules over those files, we just suppress the violations. We couldn't use the same strategy for `per-file-extend-select` -- we'd have to enable curating a rule set for every file, which would be a more significant refactor.)

---

_Comment by @justinchuby on 2023-04-25 14:28_

This feature will be useful for big repositories (eg. PyTorch) to incrementally adopt stricter rules in different components. 

Potentially run checks on all rules from the per-file-select as well for everyone, then suppress for files not in this group?

---

_Comment by @Kludex on 2023-04-25 14:35_

It's useful for small repositories as well. 👀

---

_Referenced in [astral-sh/ruff#5118](../../astral-sh/ruff/issues/5118.md) on 2023-06-15 13:41_

---

_Referenced in [astral-sh/ruff#8614](../../astral-sh/ruff/issues/8614.md) on 2023-11-11 12:29_

---

_Comment by @zanieb on 2023-11-12 02:56_

If someone wants to work on this, I'd be happy to review a proposal. I expect there to be some prototype / design work.

---

_Label `help wanted` added by @zanieb on 2023-11-12 02:56_

---

_Referenced in [apache/airflow#10742](../../apache/airflow/issues/10742.md) on 2024-02-04 12:38_

---

_Comment by @ryangalamb on 2024-03-13 17:45_

Leaving my notes here in case another contributor wants to pick this up.

The glob mechanism in Ruff is based on [globset](https://docs.rs/globset/latest/globset/), and it looks like the options supported there are supported in Ruff.

The only negation option that's supported is single character negation (e.g., `[!abc]` being the inverse of `[abc]`). I thought I could be clever and abuse this, but it doesn't seem very reliable. (It also looks awful.)

Extended globbing has negation, which could work, but extended globs are not likely to be supported by globset: https://github.com/BurntSushi/ripgrep/issues/2608

globset is part of the ripgrep project. Ripgrep allows negation by using a `!` prefix. **Putting a `!` in front of your glob will match everything except for the glob.** ([Link to ripgrep's globbing docs'](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#manual-filtering-globs))

The `!` prefix approach seems reasonable and easy to explain/document. `"!foo/*"` would match everything except for `"foo/*"`. Only a `!` prefix would be treated specially. Any other `!`s would be treated as part of the glob.

(And yes, I do realize that this was a lot of work to reach exactly what @henryiii suggested in his third bullet point :joy: Hopefully this helps other curious folks save themselves some time/effort.)

@zanieb Does the `!` prefix approach seem reasonable to you?

---

_Comment by @charliermarsh on 2024-04-05 17:05_

I'm supportive of adding negations via `!`.

---

_Comment by @zanieb on 2024-04-05 17:23_

I'm supportive as well. Is someone interested in putting up a prototype?

---

_Comment by @charliermarsh on 2024-04-05 17:29_

I think @carljm may be interested.

---

_Referenced in [astral-sh/ruff#10852](../../astral-sh/ruff/pulls/10852.md) on 2024-04-10 01:37_

---

_Closed by @carljm on 2024-04-10 03:53_

---

_Referenced in [astral-sh/ruff#10863](../../astral-sh/ruff/pulls/10863.md) on 2024-04-10 18:23_

---

_Comment by @carljm on 2024-04-10 19:20_

See some discussion at https://github.com/astral-sh/ruff/pull/10863#discussion_r1559906597 on how negative patterns should behave when the pattern does match the file (i.e. the negative pattern doesn't hit). Should this "un-ignore" the listed patterns, or do nothing (patterns are always only additive to what's ignored)?

---
