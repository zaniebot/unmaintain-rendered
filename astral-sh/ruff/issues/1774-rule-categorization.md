```yaml
number: 1774
title: Rule categorization
type: issue
state: open
author: not-my-profile
labels:
  - core
assignees: []
created_at: 2023-01-10T21:11:14Z
updated_at: 2025-09-24T17:30:18Z
url: https://github.com/astral-sh/ruff/issues/1774
synced_at: 2026-01-12T15:54:41Z
```

# Rule categorization

---

_@not-my-profile_

I think there are two perspectives ruff users may have:

* using ruff as a replacement for an existing linter (such as pylint or flake8)
* using ruff from the get-go

The README currently groups rules by their origin. Which I think is suboptimal for both cases. For the first case our README only lists the lints implemented by ruff but doesn't tell you which (or how many) lints ruff is missing. For the second case you don't actually care about where the lints are coming from, you just want to see them grouped by topic.

So I think we should steal a nice feature from Clippy which are [lint categories](https://github.com/rust-lang/rust-clippy#readme). Clippy has the following categories:

* correctness: code that is outright wrong or useless
* suspicious: code that is most likely wrong or useless
* style: code that should be written in a more idiomatic way
* complexity: code that does something simple but in a complex way
* perf: code that can be written to run faster
* pedantic: lints which are rather strict or have occasional false positives
* nursery: new lints that are still under development
* cargo: lints for the cargo manifest

We could simply use the same categorization (except with cargo instead being pyproject as per #1472). I think we would however need some additional categories since there are several lint categories that Rust does not need, for example:

* syntax: code that is syntactically invalid (syntax errors cannot go by unnoticed in Rust)
* deprecated: functions that have been deprecated (Rust has a `#[deprecated]` macro for that) (see also [#1507](https://www.github.com/charliermarsh/ruff/issues/1507))

What do you think about this?

---

_Comment by @charliermarsh on 2023-01-10 22:08_

I agree that we should kick off a re-categorization effort. Like #1773, will reply in a bit with some more thoughts :)

---

_Comment by @charliermarsh on 2023-01-11 03:48_

I generally agree. I need to give some thought to the right categorization since this will be hard to undo.

My gut reaction is that... we may need more categories? Two that come to mind for me are `docstrings` and `typing` (or `type-annotations`, or whatever).

> For the first case our README only lists the lints implemented by ruff but doesn't tell you which (or how many) lints ruff is missing.

It's not really relevant for this debate, but: in the majority of cases, we support the entire plugin. There are a few exceptions.

---

_Label `core` added by @charliermarsh on 2023-01-11 04:28_

---

_Renamed from "Lint categorization" to "Rule categorization" by @not-my-profile on 2023-01-19 15:04_

---

_Comment by @sbrugman on 2023-01-23 21:58_

For inspiration/reference: overview of [categories](https://github.com/dosisod/refurb/blob/master/docs/categories.md) used in `refurb`

Plugin ticket: https://github.com/charliermarsh/ruff/issues/1348

---

_Comment by @tgross35 on 2023-06-14 22:09_

+1 for simplifying categories, it's kind of a mess with _60 categories_ now (wow that's a lot).

I think it is totally reasonable to recategorize everything and assign coherent numbers for the error output (could use the `R` prefix and not just `RUF` since I don't see that used anywhere). With https://github.com/astral-sh/ruff/issues/1256, it would be useful to assign different levels to each lint group and allow the user to override the options, e.g.:

```toml
[lints]
suspicious = "allow"
docstrings = "error"
```

I _do_ think it is still important to keep the `flake8` numbers/groups around in the docs and usable for configuration - just since people are going to be looking for flake8 compatibility for the next decade. Doesn't need to be the main promoted style, but I have to imagine it would slow new adoption if `enable = ["E", "W"]` doesn't continue to work. Maybe there could be a "fix" tool that upgrades the flake8 style codes to whatever the new style is?

---

_Comment by @charliermarsh on 2023-06-14 22:10_

Yeah, that's roughly my thinking too: recategorize the rules, but try to preserve support for the Flake8 compatibility layer at runtime + ship tooling to automatically migration an existing configuration file.

---

_Comment by @MichaReiser on 2024-03-04 11:44_

Related requests are to disable all formatter rules, or deprecated rules

https://github.com/astral-sh/ruff/issues/10225
https://github.com/astral-sh/ruff/issues/9778

---

_Assigned to @AlexWaygood by @MichaReiser on 2024-03-30 09:08_

---

_Comment by @wwuck on 2024-09-24 11:54_

> > For the first case our README only lists the lints implemented by ruff but doesn't tell you which (or how many) lints ruff is missing.
> 
> It's not really relevant for this debate, but: in the majority of cases, we support the entire plugin. There are a few exceptions.

Is this the case where ruff implements an entire flake8 plugin but then the plugin later adds new linting checks not covered by ruff? It seems to be hard to keep track of this sort of thing.

---

_Unassigned @AlexWaygood by @AlexWaygood on 2024-09-24 13:52_

---

_Comment by @kaddkaka on 2024-11-28 13:10_

As another reference, the c++ checker clang-tidy (https://clang.llvm.org/extra/clang-tidy/checks/list.html) has amongst others, these categories:

- `performance`
- `modernize`
- `readability`
- `concurrency`
- `bugprone`

---

_Comment by @Goldziher on 2024-12-26 12:47_

this is now a convention adopted by BiomeJS and OXC. 

---

_Comment by @oXidFoX on 2025-09-24 13:52_

My use case would be to check if people have already run the ruff autofix on their code.
I would like to have the "all-autofixable" subset of rules for use with the command `ruff check --select ALL-autofixable`.

This subset would be the same as the one used by `ruff check --fix`, including the extends/ignores and the unsafe rules, if selected, but for a simple `ruff check`.

Example:
```shell
# auto fix everything it can
ruff check --fix --select ALL --ignore D203
# should return nothing more or less than the previous command
ruff check --select ALL-autofixable --ignore D203
```

Would this be possible?

---

_Comment by @allanlewis on 2025-09-24 14:01_

> I would like to have the "all-autofixable" subset of rules for use with the command `ruff check --select ALL-autofixable`.

Is this not what `--fix-only` does?


---

_Comment by @oXidFoX on 2025-09-24 16:14_

> > I would like to have the "all-autofixable" subset of rules for use with the command `ruff check --select ALL-autofixable`.
> 
> Is this not what `--fix-only` does?

no, `--fix-only` does the fix, without reporting. I would like the "check only" version.

[As the documentation states](https://docs.astral.sh/ruff/settings/#fix-only): _Apply fixes to resolve lint violations, but don't report on, or exit non-zero for, leftover violations. Implies `--fix`._

---

_Comment by @allanlewis on 2025-09-24 16:19_

> no, `--fix-only` does the fix, without reporting. I would like the "check only" version.

I see - so more like "is anything fixable".

---

_Comment by @oXidFoX on 2025-09-24 17:17_

> > no, `--fix-only` does the fix, without reporting. I would like the "check only" version.
> 
> I see - so more like "is anything fixable".

exactly this !

---
