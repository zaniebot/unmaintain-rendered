```yaml
number: 2300
title: "`--extend-select` and `--extend-ignore` work in unexpected ways"
type: issue
state: closed
author: not-my-profile
labels:
  - question
assignees: []
created_at: 2023-01-28T15:42:03Z
updated_at: 2023-01-30T21:27:01Z
url: https://github.com/astral-sh/ruff/issues/2300
synced_at: 2026-01-12T15:54:42Z
```

# `--extend-select` and `--extend-ignore` work in unexpected ways

---

_@not-my-profile_

```
$ ruff --select E501 --ignore ALL scripts
scripts/add_rule.py:44:89: E501 Line too long (92 > 88 characters)
$ ruff --select E501 --extend-ignore ALL scripts
# (nothing)
```

I would expect `--extend-select` and `--extend-ignore` to simply append the values to the `--select` and `--ignore` values however they in fact take precedence over the former.

I find the current behavior to be unintuitive, is it intentional?

---

_Comment by @charliermarsh on 2023-01-28 15:43_

Yes it's intentional.

---

_Comment by @charliermarsh on 2023-01-28 15:45_

Here are some resources on the reasoning behind it:

- https://github.com/charliermarsh/ruff/pull/1245
- https://github.com/charliermarsh/ruff/issues/1838

---

_Comment by @charliermarsh on 2023-01-28 15:46_

Closing as this is correct from my perspective but happy to keep discussing.

---

_Closed by @charliermarsh on 2023-01-28 15:46_

---

_Comment by @not-my-profile on 2023-01-28 15:48_

If it's intentional I think the options should rather be called `--override-*` because that would be more accurate than `extend` ... my point is that it doesn't extend.

---

_Comment by @charliermarsh on 2023-01-28 15:50_

I'm open to it. Though I'd like it to be done in a backwards-compatible way, and I'd prefer not to support all of `--select`, `--extend-select` (with old semantics), and `--override-select` (with the existing semantics), so I'd likely want to just not support `--extend-select`.

---

_Comment by @charliermarsh on 2023-01-28 15:53_

Although, `--override-select` suggests to me that it's clearing out the `select` and replacing it entirely -- which isn't the case. The extends just get applied last.


---

_Comment by @not-my-profile on 2023-01-28 15:55_

To be honest I don't find your reasoning in #1245 convincing.

If I understand correctly you say that it's unintuitive that `--extend-select F401` doesn't do anything when you have `ignore = ["F401"]` in your `pyproject.toml` ... however we could simply have CLI options take precedence over `pyproject.toml`  ... that doesn't require `extend-select` to always be applied last.

That is I'd argue even `--select F401` should work when you have `ignore = ["F401"]` in your `pyproject.toml`.

---

_Comment by @charliermarsh on 2023-01-28 16:17_

There are a lot of cases to consider.

The primary motivation for this change wasn't the CLI. It was that, if you don't use these semantics for `extend-select` and `extend-ignore` in a `pyproject.toml`, it becomes _impossible_ for configuration files that extend other configuration files to turn rules on and off in common cases.

Specifically, prior to that change, if you had a `pyproject.toml` like this:

```toml
[tool.ruff]
select = ["F"]
ignore = ["F401"]
```

And then a child that extended it:

```toml
[tool.ruff]
extend = "../pyproject.toml"
extend-select = ["F401"]
```

`F401` would _not_ be turned on. And in fact, it'd be impossible for the child to turn it on.

This change allowed us to treat the resolution in two passes: resolve the (select, ignore) rules, then apply the (extend-select, extend-ignore) rules.

Happy to consider a proposal that takes all of these cases into account. What you describe _sounds_ like replacing `--extend-select` and `--extend-ignore` on the CLI with `--select` and `--ignore`, but perhaps it's slightly different.


---

_Label `question` added by @charliermarsh on 2023-01-28 16:27_

---

_Reopened by @charliermarsh on 2023-01-28 16:27_

---

_Comment by @charliermarsh on 2023-01-28 16:31_

> however we could simply have CLI options take precedence over pyproject.toml

What does "take precedence" here mean precisely? Could you provide examples of the expected behavior? In a sense, it _does_ take precedence right now, since it overrides the `select`.

Is the suggestion that if a user provides either `--select` or `--ignore`, we omit any selection and ignore rules specified in `pyproject.toml` entirely? Just trying to understand the exact semantics of what you're proposing.


---

_Comment by @charliermarsh on 2023-01-28 16:38_

To be clear: I'd love to make this stuff more intuitive. I won't personally be working on it right now, but I'm very open to suggestions. I just want to make sure we're thinking through all the implications and that I'm clearly explaining _why_ certain things work the way they do right now.

---

_Comment by @not-my-profile on 2023-01-28 16:49_

> Specifically, prior to that change, if you had a pyproject.toml like this [..] And then a child that extended it [..] F401 would not be turned on. And in fact, it'd be impossible for the child to turn it on.

Right I agree that this is a problem however I think the obvious cause is that `Configuration::combine` is combining `select`, `ignore`, `extend_select` and `extend_ignore` all independently, whereas it should preserve them together.

---

_Comment by @charliermarsh on 2023-01-28 16:52_

Like, keep every foursome intact, then when resolving, resolve the first foursome, then pass those rules along and "apply" the next foursome, etc.?

---

_Comment by @not-my-profile on 2023-01-28 16:52_

Yes. This would also mean that latter `select`s override earlier `ignore`s which currently isn't the case but I think really should be.

---

_Comment by @charliermarsh on 2023-01-28 16:58_

Would we have any need for `extend-select` and `extend-ignore` in that world?

---

_Comment by @charliermarsh on 2023-01-28 17:30_

I feel like you need a PhD in `--select` to understand all the possible resolution semantics. Makes me very sad!

---

_Closed by @charliermarsh on 2023-01-30 21:27_

---
