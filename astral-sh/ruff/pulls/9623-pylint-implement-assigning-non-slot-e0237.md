```yaml
number: 9623
title: "[`pylint`] Implement `assigning-non-slot` (`E0237`)"
type: pull_request
state: merged
author: tsugumi-sys
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: impl-assigning-non-slot-E0237
created_at: 2024-01-23T11:09:40Z
updated_at: 2024-01-24T02:58:01Z
url: https://github.com/astral-sh/ruff/pull/9623
synced_at: 2026-01-12T15:55:29Z
```

# [`pylint`] Implement `assigning-non-slot` (`E0237`)

---

_@tsugumi-sys_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Implement [assigning-non-slot / E0237](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/assigning-non-slot.html)

related #970

## Test Plan

<!-- How was it tested? -->

`cargo test`


---

_Comment by @github-actions[bot] on 2024-01-23 11:41_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/embeds.py#L206'>disnake/embeds.py:206:9:</a> PLE0237 Attribute `timestamp` is not defined in class's `__slots__`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/embeds.py#L214'>disnake/embeds.py:214:9:</a> PLE0237 Attribute `colour` is not defined in class's `__slots__`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLE0237 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+2 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/embeds.py#L206'>disnake/embeds.py:206:9:</a> PLE0237 Attribute `timestamp` is not defined in class's `__slots__`
+ <a href='https://github.com/DisnakeDev/disnake/blob/a24abf43297215b6fe70e0bc61b2ec1dada84b9f/disnake/embeds.py#L214'>disnake/embeds.py:214:9:</a> PLE0237 Attribute `colour` is not defined in class's `__slots__`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLE0237 | 2 | 2 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @charliermarsh on 2024-01-23 15:33_

How does Pylint handle this for subclasses? E.g., does it allow items in `__slots__` that are attributes on superclasses?

---

_Comment by @tsugumi-sys on 2024-01-24 00:53_

@charliermarsh

If subclasses define slots, pylint checks not redefining the same slots as its superclass in  [redefined-slots-in-subclass / W0244](https://pylint.readthedocs.io/en/latest/user_guide/messages/warning/redefined-slots-in-subclass.html).

Also we can set new slots in the superclass and pylint checks it in [assigning-non-slot / E0237](https://pylint.readthedocs.io/en/latest/user_guide/messages/error/assigning-non-slot.html).

---

_@charliermarsh approved on 2024-01-24 02:45_

Thanks @tsugumi-sys, I limited the rule to classes that don't inherit from "unknown" parent classes for now.

We actually have support for resolving parent classes within the same file, so we _could_ extend the rule to check that.

---

_Label `rule` added by @charliermarsh on 2024-01-24 02:45_

---

_Label `preview` added by @charliermarsh on 2024-01-24 02:45_

---

_Renamed from "[pylint] - implement `assigning-non-slot`/`E0237`" to "[`pylint`] Implement `assigning-non-slot` (`E0237`)" by @charliermarsh on 2024-01-24 02:45_

---

_Merged by @charliermarsh on 2024-01-24 02:50_

---

_Closed by @charliermarsh on 2024-01-24 02:50_

---
