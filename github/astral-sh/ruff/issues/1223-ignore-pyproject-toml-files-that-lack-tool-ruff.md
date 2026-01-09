---
number: 1223
title: "Ignore `pyproject.toml` files that lack `[tool.ruff]`"
type: issue
state: closed
author: charliermarsh
labels:
  - question
  - configuration
assignees: []
created_at: 2022-12-13T00:11:25Z
updated_at: 2022-12-15T00:35:54Z
url: https://github.com/astral-sh/ruff/issues/1223
synced_at: 2026-01-07T13:12:14-06:00
---

# Ignore `pyproject.toml` files that lack `[tool.ruff]`

---

_Issue opened by @charliermarsh on 2022-12-13 00:11_

Maybe? I think so?


---

_Label `configuration` added by @charliermarsh on 2022-12-13 00:11_

---

_Label `question` added by @charliermarsh on 2022-12-13 00:11_

---

_Comment by @messense on 2022-12-13 04:33_

Not sure if it's related, ruff starts to complain about E501 line too long suddenly: https://github.com/PyO3/maturin/actions/runs/3682202853/jobs/6229621666

while I have set `line-length = 120` in top level `pyproject.toml`: https://github.com/PyO3/maturin/blob/e7b8c22591e89a8b3e9bb02416105a8731fed847/pyproject.toml#L49 while the `pyproject.toml` in sub-project lacks `[tool.ruff]`: https://github.com/PyO3/maturin/blob/e7b8c22591e89a8b3e9bb02416105a8731fed847/test-crates/with-data/pyproject.toml

---

_Comment by @charliermarsh on 2022-12-13 04:41_

Yeah it's related. We now use the closest `pyproject.toml` file to each Python file when resolving settings. And right now, we're not ignoring empty `pyproject.toml`, so from Ruff's perspective, there's no configuration for the files within `test-crates/with-data`.

One way to fix this is to add the following to `test-crates/with-data/pyproject.toml`:

```toml
[tool.ruff]
extend = "../../pyproject.toml"
```

But, this issue was sort of based around this question -- of whether we should ignore `pyproject.toml` files that don't have a `[tool.ruff]`. Any opinion?


---

_Comment by @charliermarsh on 2022-12-13 04:45_

(I should just go ahead and change this to ignore those “empty pyproject files, it’ll almost always lead to undesired behavior like you’re experiencing now.)

---

_Comment by @messense on 2022-12-13 04:51_

I'd expect sub-project extends parent ruff settings automatically, so when `pyproject.toml` file doesn't have a `[tool.ruff]` it's the same as its parent settings, when `pyproject.toml` file has a `[tool.ruff]` it merges with its parent settings.

---

_Comment by @charliermarsh on 2022-12-13 05:09_

Yeah, you do get the merging if you use the `extend` keyword, it's just explicit rather than automatic right now. I'll take that as +1 vote for making it automatic...


---

_Comment by @charliermarsh on 2022-12-13 05:13_

(And if I made the change described in this issue, then if the child doesn't have `[tool.ruff]`, you'd automatically get the parent settings, which isn't true today.)

---

_Comment by @charliermarsh on 2022-12-13 05:18_

I was hesitant to make this behavior automatic, but it _is_ what ESLint does.

I ran a Twitter poll and it did get 53% of the vote 🤔 

https://twitter.com/charliermarsh/status/1601803434948456448

---

_Comment by @charliermarsh on 2022-12-13 05:19_

\cc @smackesey in case you have an opinion on whether parent-child settings should be inherited automatically or only explicitly (as it is now)

---

_Comment by @smackesey on 2022-12-13 09:21_

> Yeah, you do get the merging if you use the extend keyword, it's just explicit rather than automatic right now. I'll take that as +1 vote for making it automatic...

I like ESLint's system of auto-merging, described fully here: https://eslint.org/docs/latest/user-guide/configuring/configuration-files

Am important feature is that you can stop merging at any point by putting `root: true` in your config-- this provides an escape from frustrating scenarios where you have a complex and bespoke root config but want something closer to the defaults in a subtree.

Regardless, the most important thing is that the system be fully documented in all its nuances and that those docs are easy to find. Would also be nice to have a CLI command to dump the full, post-merge config used for a file. It's always frustrating trying to discern the source of implicit configuration when docs are inaccurate or unclear.

---

_Comment by @auscompgeek on 2022-12-13 11:34_

Could also limit the amount of automatic nested configs one could have. I personally don't see many use cases than two levels of nesting, and if more really is desired the explicit `extend` is still there.

---

_Comment by @charliermarsh on 2022-12-13 15:01_

I know all-docs-in-README is bad (and not intended as a proper solution) but if helpful, I did document the behavior in the [`pyproject.toml` discovery](https://github.com/charliermarsh/ruff#pyprojecttoml-discovery) section.

---

_Comment by @charliermarsh on 2022-12-13 15:03_

@smackesey - This actually does exist as `--show-settings`, like `ruff examples/docs_snippets/docs_snippets/__init__.py --show-settings`.

(It needs to be improved in a number of ways, but it'll show the settings for the _first_ Python file resolved in the path, so it's best to pass it a single Python file. The output format is also a little overwhelming, but it is useful for debugging.)

---

_Comment by @arthur-st on 2022-12-13 16:32_

I have a use case where this would've helped - a monorepo with root-level CI-ish files like a `pyproject.toml` with configs for `ruff` and friends, which then has subprojects in a hierarchy roughly like this:
```
pyproject.toml
foo/pyproject.toml
foo/main.py
```
And running `ruff foo/main.py` from the context root seems to consume `foo/pyproject.toml` exclusively.

---

_Comment by @charliermarsh on 2022-12-13 16:48_

Ok cool. Sounds like this is confusing enough that we should indeed ignore “empty” pyproject files.

Just confirming that “extend” does solve your problem though, if foo/pyproject.toml gets an “extend”: “../pyproject.toml”?

---

_Comment by @smackesey on 2022-12-13 17:48_

>Could also limit the amount of automatic nested configs one could have. I personally don't see many use cases than two levels of nesting, and if more really is desired the explicit extend is still there.

IMO this could become very confusing-- I think it should be either 100% explicit or auto-merge all the way to the filesystem root.



---

_Comment by @arthur-st on 2022-12-13 18:01_

> Ok cool. Sounds like this is confusing enough that we should indeed ignore “empty” pyproject files.Just confirming that “extends” does solve your problem though, if foo/pyproject.toml gets an “extend”: “../pyproject.toml”?

You know, this would even be the preferred solution in my case - as that way monorepo "tenants" don't need to janitor tracking the code quality "upstream". I didn't think to check for proper remedies before posting, apologies.

That said, I agree with @smackesey that the tool should be explicit about the config it's using. My current take on possible solution, using file system layout example from my previous post, would have the tool log a message in case it detects multiple `pyproject.toml` files in the hierarchy. As in, running `ruff foo/main.py` from context root would log (output verbose by default, hidden by flag/config) `Using config from foo/pyproject.toml, multiple config files detected in hierarchy`. 

Say, then we change `foo/pyproject.toml` to extend the root `pyproject.toml`. Running `ruff foo/main.py` from context root would now log `Using config from foo/pyproject.toml, which extends root pyproject.toml`.

Undoubtedly, messaging can be improved, and there are some corner cases to ponder about here, but I think the key idea to give users control and information here is to explicitly inform them of a situation with multiple "unlinked" configuration files in the scanned tree.

---

_Comment by @charliermarsh on 2022-12-13 20:57_

Cool, so just recapping for a sec.

The current behavior is that when considering a given file, Ruff looks for the first `pyproject.toml` in the past (regardless of its contents), and uses that to determine the appropriate settings. Any `pyproject.toml` can extend any other `pyproject.toml` via an explicit `"extend"` option.

There are two different changes to this behavior being discussed in this thread.

The first change would be to keep the above behavior, but ignore any `pyproject.toml` that doesn't include `[tool.ruff]`. So inheritance would always be explicit, but we'd avoid "breaking the chain" in some common cases (like a base configuration in a monorepo, with some example projects nested therein, as we see in Maturin and Dagster).

The second change (mutually exclusive with the first) would be to _always_ merge all `pyproject.toml` files in the path (i.e., merge every parent into its child). This is effectively how ESLint works. It would lead to the same behavior as the first change in the case of a `pyproject.toml` without a `[tool.ruff]` (since merging the parent into the child is the same as ignoring an empty child entirely).

(In this latter case, I guess we'd still support an `"extend"` keyword to allow extending a configuration file by a file that's _not_ in its path, though it's hard for me to reason about what the order of precedence is when we have both explicit `"extend"` and this implicit merging. What would the semantics be? Do you merge in the `"extend"`, _then_ the parents? Or skip the parents?)

(In this latter case, we'd also need to support something like ESLint's `root: true`.)

My current preference is to make the first change, but to continue requiring explicit `"extend"` rather than merging parents implicitly. I believe that all of these systems support effectively the same functionality -- it's mostly a question of what should be implicit vs. explicit. I honestly think they're all reasonable choices as long as they're well-documented!


---

_Comment by @arthur-st on 2022-12-13 21:14_

I'm not sure I fully follow the first scenario, but if I do then I like it. Let's consider the following file hierarchy, where "p" is `pyproject.toml` file (sorry, scribbling on phone):

p
1/p
1/2/p
1/2/foo.py

If I understand you correctly, calling ruff on the Python file with `1/2/p` being the only `pyproject.toml` file without a ruff configuration block would load `1/p` settings, and it's up to the author of `1/p` to explicitly import settings from the root `p`.

If that's the case, I like this. I think a logging message noting the config used would remain a practical guardrail, but this would in any case remove a plausible source of confusion for newer users.

---

_Comment by @charliermarsh on 2022-12-13 21:16_

@arthur-st - Yes, I believe that's correct and matches what I'm proposing. (Today, calling ruff on `1/2/foo.py` would load `1/2/p` even if it's "empty". If we make this change, it'd instead skip `1/2/p` and fall back to `1/p`.)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-14 22:48_

---

_Referenced in [astral-sh/ruff#1243](../../astral-sh/ruff/pulls/1243.md) on 2022-12-14 22:59_

---

_Closed by @charliermarsh on 2022-12-15 00:35_

---
