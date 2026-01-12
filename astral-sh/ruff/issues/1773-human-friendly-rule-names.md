```yaml
number: 1773
title: Human-friendly rule names
type: issue
state: open
author: not-my-profile
labels:
  - core
  - linter
assignees: []
created_at: 2023-01-10T20:54:32Z
updated_at: 2025-07-23T05:51:25Z
url: https://github.com/astral-sh/ruff/issues/1773
synced_at: 2026-01-12T15:54:41Z
```

# Human-friendly rule names

---

_@not-my-profile_

Pylint, ESLint and Clippy all let you enable or disable lints via human-friendly names e.g. Pylint [lets you configure](https://pylint.pycqa.org/en/v2.11.1/faq.html#do-i-have-to-remember-all-these-numbers):

```
disable= wildcard-import,
 method-hidden,
 too-many-lines
```

The idea is to also support such human-friendly names for ruff in `pyproject.toml`, while still somehow supporting the numeric codes for backwards compatibility. The reasoning being that symbolic names are much more human-friendly than cryptic numeric codes. Case in point projects that care about readable configs end up adding comments to every numeric code in their config: [zulip](https://github.com/zulip/zulip/blob/a7fd994cbd00cdc905801c6394e198f81a4a4c32/pyproject.toml#L126), [fastapi](https://github.com/tiangolo/fastapi/blob/5905c3f740c8590f1a370e36b99b760f1ee7b828/pyproject.toml#L172), [sphinx](https://github.com/sphinx-doc/sphinx/blob/e17f39e7b535f4849754d46f2cd2e411df8aa535/pyproject.toml#L163)

Currently Ruff requires every rule to have a numeric code, which I don't think makes much sense given that Ruff also has Ruff-specific rules ... and I don't think we should have to introduce new cryptic numeric codes when we introduce a new Ruff-specific rule. This obviously requires quite some changes to the codebase/UI but I think doing this is worth it.

In particular I would like us to follow the [Rust naming convention for lints](https://rust-lang.github.io/rfcs/0344-conventions-galore.html#lints):

> the lint name should make sense when read as "allow *lint-name*" or "allow *lint-name* items"

I think this is a very nice convention because it results in descriptive names. And currently many of our lint names are not descriptive, e.g. TrueFalseComparison should probably rather be called EqualityComperatorForTrueOrFalse, UnnecessaryMap should probably rather be called MapWithLambdaInsteadOfComprehension.

(Previous discussion: #967)

* [x] #2902

---

_Comment by @charliermarsh on 2023-01-10 21:53_

I agree with this and I want to do it! Will reply in a bit with some more thoughts.

---

_Comment by @charliermarsh on 2023-01-11 03:35_

Okay, so like I said -- I agree! And I think there's a lot of appetite for this (I know I've heard it from @andersk and @smackesey to name a few). We should definitely do it.

A few comments:

- I _do_ feel strongly about retaining the numeric codes, even just for the sake of retaining Flake8 compatibility (I wrote a bit on that point [here](https://github.com/charliermarsh/ruff/issues/1767#issuecomment-1378025011) -- it's been critical for adoption), but IMO that in no way conflicts with adding and preferring human-friendly lint names. Of course, in some ways, we already _have_ these names defined in the code as the `Violation` structs, but they're not exposed to the user at all, and the names aren't very good or consistent.

- I _do_ think we should give some thought to the constraints we'd like to introduce around names. For example, I don't know if Clippy has a formal convention here, but from a very informal skim, `case_sensitive_file_extension_comparisons` looks like the longest name. We probably want to have some reasonable cap on length.

- We need to think on the mechanics of this from a configuration perspective (I'm referring here to the `pyproject.toml`, CLI arguments, and in-code directives (`# noqa: ...`)). The most basic proposal would be to accept rule codes and rule names interchangeably -- so you could do `--select F841` or `--select whatever-that-rule-is-called`.

Separately:

> the lint name should make sense when read as "allow lint-name" or "allow lint-name items"

This I appreciate too. I've thought about it in the past (in particular, whether the rules should describe the positive form or the negative behavior), but we've never been consistent on it. (Some of the names were also drawn from the originating plugins, which explains _some_ of the discrepancy.)


---

_Comment by @charliermarsh on 2023-01-11 03:43_

Oh, it's worth mentioning: the one thing we lose by moving away from the `F841`-style system, is that we'll no longer have the ability to turn off sets of rules (apart from entire categories) with (e.g.) `--select F8`. I'm totally fine with this. I think it's quite rare to use that behavior -- anecdotally, most configurations tend to use precise codes (`F841`) and category codes (`F`) anyway.

---

_Label `core` added by @charliermarsh on 2023-01-11 04:28_

---

_Renamed from "Human-friendly lint names" to "Human-friendly rule names" by @not-my-profile on 2023-01-19 15:04_

---

_Comment by @spaceone on 2023-01-30 15:20_

I use the prefix categories heavily, as in `pycodestyle`'s `E` they are logically related. E.g. I am doing a tab-to-spaces migration by just selecting `E1` (in `autopep8`).

Also I can remember a lot of codes but could not remember a lot of long names. And also wouldn't want to name them in a `--select` argument.

---

_Comment by @scop on 2023-01-30 18:23_

I'd actually like to use the human friendly names as `--select` arguments rather than codes, provided that there's appropriate shell completion available for the args (currently there isn't). No need to remember that much, then. Some kind of prefix grouping would be highly useful in that scenario, too.

---

_Comment by @ngnpope on 2023-02-09 17:25_

+1 for switching to human friendly names everywhere eventually.

I think we should pivot away from the current flake8-plugin grouping of rules entirely to rule categories (as per #1774). That includes the way that the rules are grouped in the repository.

And then only maintain a mapping of flake8-plugins/codes to ruff rules to show what has been implemented to aid migration. Ideally I'd like to see this documentation piece also refer to the original code prefixes (especially where ruff has changed them) and also state reasons for not implementing rules that the original plugin did, or why the implementation differs, e.g. `F401`+`F403`.

This would also overcome some of the confusion highlighted in #2517 as well as the fact that many plugins have overlap with each other so they're implemented in some areas in the code, but not others.

A couple of other thoughts:
- When we have categories we could replace `--select S1` with, say, `--select @security`?
- We could also implement a rule, e.g. `upgrade-noqa-comments`, to rewrite the old codes to their human-friendly versions? 

---

_Comment by @Avasam on 2023-03-07 07:39_

Oh please yes!
Exactly for the reason of adding comments so I can better understand what a rule does, and why it may be disabled, at a glance and not have to remember hundreds of codes.

I've just started trying out migrating to Ruff, and here's an example of a bunch of extra comments for that: https://github.com/Avasam/speedrun.com_global_scoreboard_webapp/blob/6678056ac2d42ee66b97fc074515855646f2bfc4/pyproject.toml

---

_Comment by @sscherfke on 2023-04-19 10:22_

This would make `# noqa` comments more readable, too:

```python
def complex_func():  # noqa: C901
    ...
```
vs.
```python
def compoex_func(): # noqa: complex-structure
    ...
```

---

_Comment by @9999years on 2024-02-16 23:21_

I would also like this! I found it confusing that the URL for `G004` is https://docs.astral.sh/ruff/rules/logging-f-string/ but that `--ignore logging-f-string` didn't work. 

Previous comments state that the names aren't exposed to users, but that isn't true any more:

```ShellSession
$ ruff rule G004
# logging-f-string (G004)

Derived from the **flake8-logging-format** linter.

## What it does
Checks for uses of f-strings to format logging messages.
```

---

_Comment by @kaddkaka on 2024-02-26 21:15_

for pylint rules (PL): it might be wise to just reuse the pylint names without any modification.

---

_Comment by @kaddkaka on 2024-03-19 22:10_

What needs to be done to get this implemented? What's left and can a beginner in this repo help out? 

---

_Label `linter` added by @dhruvmanila on 2024-03-20 07:27_

---

_Comment by @gpshead on 2024-06-12 21:42_

I pretty much agree with what was already summarized above. One lesson from pylint: rules will need multiple names.  Only one should be considered the _primary_ (presumably used in the error message and suggested for addition to noqa comments or use in a modern suppression config elsewhere).  That way when you don't really like an existing name or come up with a more meaningful alternative name, or need to align with another tools name, you can.  But all existing configs and directives in code still remain valid.

---

_Comment by @Pierre-Sassoulas on 2024-06-13 07:22_

> One lesson from pylint: rules will need multiple names.

Yes, the refactor to be able to have multiple aliases after the fact was very painful.

> Only one message should be considered the primary 

In pylint we call this, "old_names" and we have a **primary** name the only one that is active (it has an impact in the documentation in particular), but in fact ``aliases`` would be more accurate / more powerful. A possible use case is to permits to have one message be a placeholder for multiple one, so you can deactivate multiple similar messages using a single englobing one.  For example, ``missing-docstring`` as a placeholder for ``missing-function-docstring`` / ``missing-module-docstring`` / ``missing-class-docstring``. It's currently possible to do that in pylint but the semantic in the code / doc is not accurate. The documentation present ``missing-docstring`` as a deprecated message and not as a possible elegant disable shortcut and if we ever add a tool to upgrade the deprecated messages to the primary one, it would make the code worst / more verbose for nothing.

---

_Comment by @sbrugman on 2024-11-20 16:02_

Do you think there is enough consensus to start extending the rule selectors to accept the current human-readable names in addition to the legacy codes? Happy to give it a try.

Selecting by human-readable names is a stepping stone to presets/recategorisation.

We could start off with extending support `ruff rule [human-name]` and when that's stable extend the rule selection in the remainder of `ruff`? 

I see there has been an attempt to extend selection for `noqa` comments already: https://github.com/astral-sh/ruff/pull/11757. However, that PR in addition modifies the linter output, which is not strictly necessary right now.

---

_Comment by @MichaelOultram-pexip on 2024-11-20 16:33_

We have been running the changes proposed in https://github.com/astral-sh/ruff/pull/11757 internally at Pexip for the last few months, and have been very happy with the experience. While the linter output changes are useful, I agree they are not required for this PR. I'll work on removing that part to make the PR more manageable.

EDIT: that PR has been closed as the ruff team wish to take a different approach.

---

_Comment by @InSyncWithFoo on 2024-11-28 10:01_

I also have a very good use case for `ruff rule rule-name`. Will create a subissue and submit a PR tomorrow.

---

_Comment by @MichaReiser on 2024-11-28 12:04_

> I also have a very good use case for `ruff rule rule-name`. Will create a subissue and submit a PR tomorrow.

I suggest to first discuss it on the issue before submitting a PR. But I'm interested to hear about your use case.

---

_Comment by @nathanscain on 2025-07-23 03:01_

Quite a bit of time has passed on this, and I was getting curious on the team's intent. Since this discussion took place, ty has been released in alpha with human-friendly names from the start. That project is obviously based on ruff and would eventually be able to function as a full type-aware linter.

Is the intention to still work towards something like this under ruff, or would this be something to instead expect to be part of a future ty-based linter.

Similar question applies to #1774 regarding recategorizing rules more like clippy.

---

_Comment by @MichaReiser on 2025-07-23 05:51_

This is still on our mind and we want to work towards human readable names and a new form of rule categorization in Ruff. It's just that we've all been very busy with ty but we do plan on porting back some of the improvements (like human readable rule names) to Ruff in the future.

---
