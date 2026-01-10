```yaml
number: 11091
title: Ruff rule to remove six
type: issue
state: closed
author: inoa-jboliveira
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-04-22T14:49:43Z
updated_at: 2024-12-03T04:13:34Z
url: https://github.com/astral-sh/ruff/issues/11091
synced_at: 2026-01-10T11:09:53Z
```

# Ruff rule to remove six

---

_Issue opened by @inoa-jboliveira on 2024-04-22 14:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Proposal: creation of rules to remove usages of the six module, replacing with the Python 3 only equivalent. This would align with the UP rules, but I don't know if they would be allowed in there or simply into the RUF code. The idea is to have fixable rules for the most common features of six and help people migrate with more confidence. There should also exist a catch-all rule just checking if six was imported.

- **UP900** Deprecate six (not fixable) . 
  - E.g. `import six` -> "Usage of six is discouraged on applications not supporting Python 2"
- **UP901** Replace types (e.g. `six.string_types`, `six.text_type`, `six.binary_type`, `six.integer_types`) with corresponding Python 3 types. 
  - E.g.: `isinstance(foo, six.string_types)` -> `isintance(foo, (str,))`
  - Note: arguably this is the most used feature of six
- **UP902** Replace `six.iter*` with the corresponding method.
  - E.g.: `for k, v in six.iteritems(foo):` -> `for k, v in foo.items():`
- **UP903** Replace `six.print_` with `print`.
  - E.g.: `six.print_("hello")` -> `print("hello")`
- **UP904** Replace `six.PY2, PY3, ... with direct checks`. 
  - E.g.: `if six.PY3:` -> `if sys.version_info[0] == 3:` 
  - Note: this is a quite important one because six.PY3 is a source of bugs -- see YTT202
  - Could also warn the user that the check is no longer be necessary since all code is running on Python 3




---

_Comment by @charliermarsh on 2024-04-22 23:56_

There's some of this in the `UP` rules already (like `UP029` removes `six.callable` and others that are now builtins), but I think I intentionally cut scope on other `six` rules because I wanted to avoid adding and maintaining complex rules that would only be relevant for one-time 2-to-3 transitions (as opposed to rules that are useful for ongoing Python 3 development).

---

_Comment by @charliermarsh on 2024-04-22 23:57_

I'm open to adding some of these selectively under the `UP` category but it's a bit of a balance -- I don't want to add and maintain a bunch of complex `six` rules since the value is diminishing over time.

---

_Label `rule` added by @MichaReiser on 2024-04-23 07:07_

---

_Label `needs-decision` added by @MichaReiser on 2024-04-23 07:07_

---

_Comment by @MichaReiser on 2024-04-23 07:09_

I share the sentiment with @charliermarsh. If the primary target group is python 2 users, then we should not implement the rule according to our rule acceptance guidelines. 

`UP900` can be achieved by using [`banned-api`](https://docs.astral.sh/ruff/settings/#lint_flake8-tidy-imports_banned-api)

---

_Comment by @inoa-jboliveira on 2024-04-23 11:53_

Hi @charliermarsh and @MichaReiser, thank you for the replies. 

The point here is not serve people upgrading from python 2, it is to drop the old compatibility layer many projects kept for a long time even now supporting most modern python versions only. 

Same thing with [UP025](https://docs.astral.sh/ruff/rules/unicode-kind-prefix/) where you replace "u" prefix of strings which is leftover from years of legacy support.

I have a repo with over 1M LOC of python and it took a while to remove everything, but then I noticed many dependencies still imported six even if they no longer supported python 2.

This means having these rules could speed up projects from dropping this layer (again same purpose as many of the UP rules).

If you feel this doesn't bring enough benefit, you may close the issue

---

_Comment by @eli-schwartz on 2024-05-07 20:03_

Related:

https://github.com/astral-sh/ruff/pull/2049#discussion_r1090077646
https://github.com/astral-sh/ruff/pull/2332

---

_Comment by @eli-schwartz on 2024-05-07 20:05_

If it's possible to reimplement via more generic rewriting methods that didn't exist at the time, I think that would be quite convenient as IMO the UP rules are mostly a matter for oneshot actions, and six isn't particularly special. I think the only real reason in the past for not covering these rules effectively boiled down to implementation complexity and no one available to do the work?

---

_Comment by @jamesliu4c on 2024-12-02 21:40_

What about just the first rule?

> UP900 Deprecate six (not fixable) .
> E.g. import six -> "Usage of six is discouraged on applications not supporting Python 2"

The only things that have to be checked are `import six` and `from six import`.

This would be a minimal amount of work to implement and maintain.

It would also be high impact. I implemented this as a Pylint rule in my org. It was an enormous success. We were two years past a migration to Python 3 and had a bunch of `six` code lying around. We also had a rule that required lint to pass to merge, so within a few months, six was gone from our codebase.

---

_Comment by @inoa-jboliveira on 2024-12-02 22:24_

@jamesliu4c 
> The only things that have to be checked are `import six` and `from six import`.

For that case alone, if you can use [TID251](https://docs.astral.sh/ruff/rules/banned-api/#banned-api-tid251) and configure six on [banned-api](https://docs.astral.sh/ruff/settings/#lint_flake8-tidy-imports_banned-api) for pyproject.toml

Sometimes I use TID253 if the ban is only on top level imports


By the way, I now agree with the ruff team here as there are other ways to block six and there is low value in maintaining these rules.

IMO this Issue can be closed 

---

_Comment by @jamesliu4c on 2024-12-02 22:52_

Oh, that's great. Thanks!

---

_Closed by @dhruvmanila on 2024-12-03 04:13_

---
