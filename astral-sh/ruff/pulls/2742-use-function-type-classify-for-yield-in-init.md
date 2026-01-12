```yaml
number: 2742
title: "Use `function_type::classify` for `yield-in-init`"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/init
created_at: 2023-02-10T21:18:39Z
updated_at: 2023-02-10T22:11:48Z
url: https://github.com/astral-sh/ruff/pull/2742
synced_at: 2026-01-12T04:52:01Z
```

# Use `function_type::classify` for `yield-in-init`

---

_Pull request opened by @charliermarsh on 2023-02-10 21:18_

_No description provided._

---

_Merged by @charliermarsh on 2023-02-10 21:19_

---

_Closed by @charliermarsh on 2023-02-10 21:19_

---

_Branch deleted on 2023-02-10 21:19_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pylint/rules/yield_in_init.rs`:16 on 2023-02-10 21:29_

It's `__init__`  methods ... not `__init__.py` methods (you made the mistake twice).

---

_@not-my-profile reviewed on 2023-02-10 21:29_

---

_@not-my-profile reviewed on 2023-02-10 21:30_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pylint/rules/yield_in_init.rs`:20 on 2023-02-10 21:30_

Even without that mistake I find that description to be very bad.

Firstly it is misleading because generators can be used in `__init__` methods ... it's just that the `__init__` method cannot be a generator.

Secondly "is not allowed" is just completely nondescript ... what does that mean? Not allowed by whom?

I think [GitHub's CodeQL documentation](https://codeql.github.com/codeql-query-help/python/py-init-method-is-generator/) does a better job:

> The `__init__` method of a class is used to initialize new objects, not create them. As such, it should not return any value. By including a yield expression in the method turns it into a generator method. On calling it will return a generator resulting in a runtime error.

The last sentence here is the important part.

---

_@charliermarsh reviewed on 2023-02-10 21:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/yield_in_init.rs`:20 on 2023-02-10 21:35_

Alright, I'll fix it up! Though, in my opinion, calling something "very bad" is not a productive way to deliver feedback and is a good way to turn a discussion into an argument.

---

_@not-my-profile reviewed on 2023-02-10 22:05_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pylint/rules/yield_in_init.rs`:20 on 2023-02-10 22:05_

The documentation is written in a voice of authority ... when we use such a voice I think we should take great care that what we are writing is correct, clear and informative because otherwise we are very much bound to cause confusion or waste time of users who take our documentation at face value.

I think accurately and clearly answering `Why is this bad?` can be quite work-intensive for many of our lints. I'd still like us to put in that work rather than adding vague descriptions.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/yield_in_init.rs`:20 on 2023-02-10 22:08_

I agree! Nowhere did I disagree that the documentation here could be improved.

---

_@charliermarsh reviewed on 2023-02-10 22:08_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pylint/rules/yield_in_init.rs`:20 on 2023-02-10 22:08_

I am not sure if it's ok for us to copy the documentationfrom GitHub CodeQL in verbatim ... while you did add a reference in e5f5142e3ed6857108a610f6c732d80491dd2492 it's not clear that the section is from there. I guess this could be clarified with a Markdown footnote ... but I guess it also raises some licensing questions ... is the licensing of our documentation more lax than of our code?

---

_@not-my-profile reviewed on 2023-02-10 22:08_

---

_@charliermarsh reviewed on 2023-02-10 22:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pylint/rules/yield_in_init.rs`:20 on 2023-02-10 22:11_

I thought a reference was sufficient but I don't want there to be any ambiguity so I'll just rewrite it entirely.


---
