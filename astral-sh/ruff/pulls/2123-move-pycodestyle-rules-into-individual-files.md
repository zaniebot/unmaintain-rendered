```yaml
number: 2123
title: Move pycodestyle rules into individual files
type: pull_request
state: merged
author: ericroberts
labels: []
assignees: []
merged: true
base: main
head: pycode-style-rules-dir
created_at: 2023-01-24T12:27:48Z
updated_at: 2023-01-24T15:02:16Z
url: https://github.com/astral-sh/ruff/pull/2123
synced_at: 2026-01-12T04:52:00Z
```

# Move pycodestyle rules into individual files

---

_Pull request opened by @ericroberts on 2023-01-24 12:27_

As discussed in https://github.com/charliermarsh/ruff/pull/2038 the project is moving to a model where every rule gets its own file.

I did not put the violations with the rules yet, it's already a large changeset, figured I could follow up with that in another PR. But let me know if you would like me to add it to this instead.

There's one function in `/src/rules/pycodestyle/rules/mod.rs` called `compare` that gets used by multiple rules. I'm not sure if this is where it should live or if there's a standard practice for these utility or helper functions. Would appreciate some guidance on that!

---

_@charliermarsh reviewed on 2023-01-24 12:40_

---

_Review comment by @charliermarsh on `src/rules/pycodestyle/rules/mod.rs`:6 on 2023-01-24 12:40_

This is small enough that I'd be fine inlining it at each call site. I think it used to be "longer".

Alternatively, you could put this in a `helpers.rs` file in `pycodestyle`. Totally up to you.

---

_Comment by @charliermarsh on 2023-01-24 12:40_

Looks great! Thank you

---

_@ericroberts reviewed on 2023-01-24 12:45_

---

_Review comment by @ericroberts on `src/rules/pycodestyle/rules/ambiguous_names.rs`:49 on 2023-01-24 12:45_

I also meant to mention this, I put three different rules here but they are almost exactly the same and use the same helper method. I wasn't sure if you wanted to stick to one rule per file or if this is ok @charliermarsh 

---

_@ericroberts reviewed on 2023-01-24 12:49_

---

_Review comment by @ericroberts on `src/rules/pycodestyle/rules/mod.rs`:6 on 2023-01-24 12:49_

OK, I put the function in each rule file that uses it (two of them)

---

_@charliermarsh reviewed on 2023-01-24 12:59_

---

_Review comment by @charliermarsh on `src/rules/pycodestyle/rules/ambiguous_names.rs`:49 on 2023-01-24 12:59_

I think it'd be _slightly_ preferable to split this into three files and reference a shared helper in `pycodestyle/helpers.rs`, if you don't mind?

---

_@ericroberts reviewed on 2023-01-24 13:06_

---

_Review comment by @ericroberts on `src/rules/pycodestyle/rules/ambiguous_names.rs`:49 on 2023-01-24 13:06_

Can do!

---

_@charliermarsh reviewed on 2023-01-24 13:09_

---

_Review comment by @charliermarsh on `src/rules/pycodestyle/rules/ambiguous_names.rs`:49 on 2023-01-24 13:09_

The reason being that we may start using the top-level rustdoc at the top of the violation file to automatically generate documentation about the rule (e.g., what does it do, why is it recommended, etc.) -- so having each rule in its own file has benefits unrelated to the code structure itself :)

---

_@ericroberts reviewed on 2023-01-24 13:54_

---

_Review comment by @ericroberts on `src/rules/pycodestyle/rules/ambiguous_names.rs`:49 on 2023-01-24 13:54_

Yup totally makes sense. I've made that change!

---

_@not-my-profile reviewed on 2023-01-24 14:21_

---

_Review comment by @not-my-profile on `src/rules/pycodestyle/rules/mod.rs`:6 on 2023-01-24 14:21_

> This is small enough that I'd be fine inlining it at each call site.

Just a note that Rust supports `#[inline]` for inlining functions ... personally I'd prefer us to not duplicate code in general even if it is just 10 lines.

---

_Merged by @charliermarsh on 2023-01-24 14:27_

---

_Closed by @charliermarsh on 2023-01-24 14:27_

---

_Comment by @charliermarsh on 2023-01-24 14:27_

Thank you!

---

_Branch deleted on 2023-01-24 14:32_

---

_@ericroberts reviewed on 2023-01-24 14:37_

---

_Review comment by @ericroberts on `src/rules/pycodestyle/rules/mod.rs`:6 on 2023-01-24 14:37_

I looked up #[inline] but I'm not sure I understand how to use it right away (very new to Rust). Perhaps I misunderstood the original request. I will look into it some more, or if you have an example of how to do it in this case I'd really appreciate it.

---

_@charliermarsh reviewed on 2023-01-24 14:42_

---

_Review comment by @charliermarsh on `src/rules/pycodestyle/rules/mod.rs`:6 on 2023-01-24 14:42_

`#[inline]` is a hint to the compiler that it should inline the function wherever it's used. It's typically not needed, since the compiler is pretty good at figuring out where that's a helpful behavior.

---

_@charliermarsh reviewed on 2023-01-24 14:43_

---

_Review comment by @charliermarsh on `src/rules/pycodestyle/rules/mod.rs`:6 on 2023-01-24 14:43_

My suggestion to inline the code was more of a WET vs. DRY thing :) But I don't feel strongly.

---

_@not-my-profile reviewed on 2023-01-24 14:44_

---

_Review comment by @not-my-profile on `src/rules/pycodestyle/rules/mod.rs`:6 on 2023-01-24 14:44_

No worries. The term "inlining" is somewhat ambiguous ... I don't think Charlie meant inlining as in avoiding a function call. `#[inline]` can be added  for performance when a function is called very often, see [Inlining](https://nnethercote.github.io/perf-book/inlining.html) ... but this is not something we probably actually want here ... I just wanted to mention it.^^

---

_Review comment by @ericroberts on `src/rules/pycodestyle/rules/mod.rs`:6 on 2023-01-24 15:02_

well since I did end up adding a helpers file for a different change, I could move compare to that file. That seems reasonable to me, I can open another PR for that

---

_@ericroberts reviewed on 2023-01-24 15:02_

---
