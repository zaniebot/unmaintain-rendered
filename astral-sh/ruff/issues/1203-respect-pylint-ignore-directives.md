```yaml
number: 1203
title: Respect pylint ignore directives
type: issue
state: open
author: LefterisJP
labels:
  - core
  - suppression
assignees: []
created_at: 2022-12-11T23:49:53Z
updated_at: 2025-10-07T21:19:33Z
url: https://github.com/astral-sh/ruff/issues/1203
synced_at: 2026-01-10T11:09:43Z
```

# Respect pylint ignore directives

---

_Issue opened by @LefterisJP on 2022-12-11 23:49_

It would be really cool for a codebase moving from pylint slowly to ruff to be able to understand and respect pylint directives.

For example `ARG` module has flake8-unused-arguments. That is very similar to something pylint does. And as usual if somewhere you want to disable the warning you can add the proper pylint directive. For this it would be `# pylint: disable=unused-argument`.

It would be cool if ruff understood those directives even for other modules like flake8-unused-arguments which do the same thing.

---

_Label `enhancement` added by @charliermarsh on 2022-12-12 01:10_

---

_Label `configuration` added by @charliermarsh on 2022-12-12 01:10_

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:14_

---

_Label `configuration` removed by @charliermarsh on 2022-12-31 18:14_

---

_Label `core` added by @charliermarsh on 2022-12-31 18:14_

---

_Label `noqa` added by @charliermarsh on 2022-12-31 18:15_

---

_Comment by @joshuachapnick-bc on 2023-01-02 13:55_

+1 for this feature

---

_Comment by @charliermarsh on 2023-01-17 12:58_

I think there are two pieces here:

1. Supporting Pylint directives for rules that are mapped explicitly to existing Pylint rules (anything in `PLR`, `PLC`, etc.).
2. Supporting Pylint directives for rules that aren't currently mapped, but have analogs.

I'm open to supporting (1), but it does require a decent amount of work, since Pylint provides [a lot of flexibility](https://pylint.pycqa.org/en/latest/user_guide/messages/message_control.html) in where enables and disables are placed (scopes, blocks, single lines, etc.).

(Unrelated to this issue, but I do eventually want to add Ruff's own system for ignores that's decoupled from Flake8 (while continuing to support Flake8's `noqa`), and that system would definitely support multi-line ignores.)

I'm hesitant to support (2). It feels hard to get right, since it requires that (e.g.) we're reporting errors on the exact same lines.


---

_Comment by @LefterisJP on 2023-01-17 13:02_

I guess (1) is already good enough.

For (2) there is some easy wins. For example at rotki we disable the ruff "BLE" check due to also running pylint with that check on and ruff not respecting/seeing the ignores.

---

_Comment by @sbrugman on 2023-01-24 23:59_

In the case of replacing pylint entirely with ruff, there could simply be a rule RUFF9XX that detects pylint directives, and as autofix removes them. This could be simple to implement and helpful to start. When explicit mapping is available, the fix could be changed to use flake8 or ruff directives.

---

_Comment by @spaceone on 2023-01-30 15:10_

A fixer which transforms the pylint-comments into ruff noqa-comments would be helpful.
```diff
-# pylint: disable-msg=R0133
+# noqa: PLR0133
```

---

_Comment by @coby-soffer on 2023-05-06 21:04_

+1


---

_Comment by @StefanKX on 2024-01-16 06:24_

+1 here as well

---

_Comment by @kaddkaka on 2024-02-27 09:06_

> A fixer which transforms the pylint-comments into ruff noqa-comments would be helpful.
> 
> ```diff
> -# pylint: disable-msg=R0133
> +# noqa: PLR0133
> ```

Pylint disable support rule names. Without rule names, this lowers the value of the waiver comment. 

> ```diff
> -# pylint: disable=comparison-of-constants
> +# noqa: PLR0133
> ```

After this transformation the next person arriving at the code will not easily understand why the disable is there.

After ruff adopts #1773 "human-friendly rules names", this transformation feels more attractive. 

---

_Comment by @kaddkaka on 2024-11-20 17:38_

> I think there are two pieces here:
> 
>     1. Supporting Pylint directives for rules that are mapped explicitly to existing Pylint rules (anything in `PLR`, `PLC`, etc.).
> 
> I'm open to supporting (1), but it does require a decent amount of work, since Pylint provides [a lot of flexibility](https://pylint.pycqa.org/en/latest/user_guide/messages/message_control.html) in where enables and disables are placed (scopes, blocks, single lines, etc.).

A possible way forward could be to only some kind of directive to start. Or is there a risk that would cause more confusion?

---

_Comment by @ran-isenberg on 2025-01-02 07:00_

is there any work done on this?
I work in an enterprise and we want to ditch pylint in favor of Ruff but unless there's either a utility to rename or ruff accepts pylint disable lines, we wont be able to do that.

---

_Comment by @gsemet on 2025-01-02 09:47_

If you really want ruff to be a must have in enterprise, you would indeed support pylint, bandit, flake8 in-code exception directly.
But if you really want to hit a score for sensitive code, you would provide a way to support “justification comments” to describe why this rules is disabled in this place. This would need to be reported in the report file as “violation with justification”.

---

_Comment by @SamuelMarks on 2025-10-07 21:19_

Any update here? - Would be great to see support for ignoring the same things `pylint` ignores but in `ruff`.

---
