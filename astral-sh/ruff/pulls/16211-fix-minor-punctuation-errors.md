```yaml
number: 16211
title: Fix minor punctuation errors
type: pull_request
state: closed
author: isgin01
labels:
  - documentation
assignees: []
base: main
head: main
created_at: 2025-02-17T12:34:11Z
updated_at: 2025-02-18T12:24:58Z
url: https://github.com/astral-sh/ruff/pull/16211
synced_at: 2026-01-12T15:55:54Z
```

# Fix minor punctuation errors

---

_@isgin01_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

<!-- What's the purpose of the change? What does it do, and why? -->

<!-- How was it tested? -->


---

_Review comment by @AlexWaygood on `CONTRIBUTING.md`:683 on 2025-02-17 13:42_

Hmm, these are the only changes in this PR that are clear improvement to me. For most of the others, I prefer them as they are -- I wouldn't usually put a comma after "e.g.". Even in this paragraph, I would remove the comma you've added after "e.g.".

---

_@AlexWaygood reviewed on 2025-02-17 13:43_

Thanks! Not sure about some of these changes, but some look like improvements :-)

---

_Label `documentation` added by @AlexWaygood on 2025-02-17 13:43_

---

_Review comment by @augustelalande on `CONTRIBUTING.md`:683 on 2025-02-17 17:25_

FYI, it's recommended by most style guides that "e.g." be followed by a comma. It's because it can be substituted for "for example" which should also typically be followed by a comma.

There are many sources, but here's one for reference: https://ontariotraining.net/grammar-tip-punctuation-with-i-e-and-e-g/

---

_@augustelalande reviewed on 2025-02-17 17:25_

---

_Review comment by @isgin01 on `CONTRIBUTING.md`:683 on 2025-02-18 02:36_

Hi @AlexWaygood, I see your point, I just wanted to keep it consistent with the rest of the docs, where most "e.g"s are followed by a comma.

---

_@isgin01 reviewed on 2025-02-18 02:36_

---

_@InSyncWithFoo reviewed on 2025-02-18 11:47_

---

_Review comment by @InSyncWithFoo on `CONTRIBUTING.md`:683 on 2025-02-18 11:47_

For the record, [uv's style guide](https://github.com/astral-sh/uv/blob/929e7c3ad96ff6b14aeb60527e6a4526ed24ec43/STYLE.md#general) also recommends putting comma after "e.g." and "i.e.".

---

_@AlexWaygood reviewed on 2025-02-18 12:14_

---

_Review comment by @AlexWaygood on `CONTRIBUTING.md`:683 on 2025-02-18 12:14_

> FYI, it's recommended by most style guides that "e.g." be followed by a comma. It's because it can be substituted for "for example" which should also typically be followed by a comma.

Typically, but not always. Take a look at the change in `BREAKING_CHANGES.md` in this PR. On `main` it's

```
Previously, Ruff supported the non-standard compliant emoji identifiers e.g. `📦 = 1`.
```

And I agree that's ungrammatical if you replace it with "for example":


```
Previously, Ruff supported the non-standard compliant emoji identifiers for example `📦 = 1`.
```

But it's also ungrammatical if you add a comma after "for example" here:

```
Previously, Ruff supported the non-standard compliant emoji identifiers for example, `📦 = 1`.
```

To make it grammatical, you'd need to place a comma _before_ the "for example" here as well as after:

```
Previously, Ruff supported the non-standard compliant emoji identifiers, for example, `📦 = 1`.
```

Or, even better, start a new sentence:

```
Previously, Ruff supported the non-standard compliant emoji identifiers. For example, `📦 = 1`.
```

But best of all would probably be to use a different construction altogether here, like "such as":

```
Previously, Ruff supported non-standards-compliant emoji identifiers such as `📦 = 1`.
```

---

_Comment by @AlexWaygood on 2025-02-18 12:23_

Thanks again @eqsdxr! I filed #16228 instead, which does a couple of things slightly differently. You're listed as a co-author of the PR :-)

---

_Closed by @AlexWaygood on 2025-02-18 12:24_

---
