```yaml
number: 7203
title: Do not use code location for Gitlab fingerprints.
type: pull_request
state: merged
author: gregersn
labels:
  - breaking
  - linter
assignees: []
merged: true
base: main
head: 7159-dont-use-code-position-in-gitlab-fingerprint
created_at: 2023-09-06T16:07:37Z
updated_at: 2023-09-08T06:25:27Z
url: https://github.com/astral-sh/ruff/pull/7203
synced_at: 2026-01-12T02:45:38Z
```

# Do not use code location for Gitlab fingerprints.

---

_Pull request opened by @gregersn on 2023-09-06 16:07_

Using code location when creating hash for Gitlab fingerprints makes the codequality reports in Gitlab very unstable. Any change of position would trigger both a "fixed" message, and a "new problem detected" message in Gitlab, making it very noisy.

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

[Do not use code location for Gitlab fingerprints.](https://github.com/astral-sh/ruff/commit/f5589b9e95b2c7d0fa01e5c486f9d5376f619c5f)

Using code location when creating hash for Gitlab fingerprints
makes the codequality reports in Gitlab very unstable.
Any change of position would trigger both a "fixed" message, and
a "new problem detected" message in Gitlab, making it very noisy.

## Test Plan

By running `RUFF_UPDATE_SCHEMA=1 cargo test`

Also, ran `cargo run -p ruff_cli -- /path/to/pyfile --no-cache --format gitlab` before and after moving adding new lines, and observed that fingerprints in report did not change.


Close #7159 

---

_Renamed from "WIP: Do not use code location for Gitlab fingerprints." to "Do not use code location for Gitlab fingerprints." by @gregersn on 2023-09-06 16:42_

---

_Comment by @github-actions[bot] on 2023-09-06 16:47_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb reviewed on 2023-09-06 16:52_

---

_Review comment by @zanieb on `crates/ruff/src/message/gitlab.rs`:91 on 2023-09-06 16:52_

A comment about the purpose of this may be useful.

Doesn't this make the fingerprint change based on previous fingerprints? Won't this dependency change a downstream fingerprint if a conflicting upstream diagnostic is fixed?

---

_Review comment by @MichaReiser on `crates/ruff/src/message/gitlab.rs`:60 on 2023-09-07 06:19_

We can already initialize the hash set with the right capacity to avoid multiple resizes (and, copying over all elements multiple time). 

```suggestion
        let mut fingerprints = HashSet::<String>::with_capacity(self.messages.len());
```

---

_Review comment by @MichaReiser on `crates/ruff/src/message/gitlab.rs`:112 on 2023-09-07 06:31_

I recommend changing fingerprint to return a `u64` instead of the serialized string to improve performance. Returning a u64 over string has the advantages:

* The set can store the u64 values directly without having to "clone" the heap-allocated strings (less memory requirement and fewer heap allocations)
* Hashing a string requires more CPU cycles than hashing a u64

```suggestion
fn fingerprint(message: &Message, salt: u64) -> u64 {
```

The serialization of the u64 to a hex encoded string can be moved to line 97 to only do it for the final fingerprint.

---

_Review comment by @MichaReiser on `crates/ruff/src/message/gitlab.rs`:123 on 2023-09-07 06:37_

This is unrelated to your change but I noticed it now when reviewing this PR. It would probably be better to hash `kind.name` rather than the rule because the Rule's hash implementation changes every time a new rule is inserted into the `Rule` enum above which happens frequently. What do you think @zanieb ?

```suggestion
    kind.name.hash(&mut hasher);    
```

---

_Review comment by @MichaReiser on `crates/ruff/src/message/gitlab.rs`:123 on 2023-09-07 06:38_

Nit: I would move the salt to the top (line 123) as I consider it an initialization of the hashing. 

And we can use `hasher.write_u64(salt)` directly after changing salt to a u64



---

_@MichaReiser requested changes on 2023-09-07 06:40_

Thank you so much for your contribution. I hope you had some fun writing your first Rust code. 

I ask you to change `fingerprint` to return a `u64` and delay the string serialization until we initialize the `"fingerprint"` field for improved performance and lower memory usage. 

We're then good to mere and pack this into the next release! 

---

_Label `breaking` added by @MichaReiser on 2023-09-07 06:41_

---

_Label `linter` added by @MichaReiser on 2023-09-07 06:41_

---

_@gregersn reviewed on 2023-09-07 07:04_

---

_Review comment by @gregersn on `crates/ruff/src/message/gitlab.rs`:123 on 2023-09-07 07:04_

This made sense to change, as you present  it, since  that could lead to as much a breaking change every time a new rule is added, as changing the hashing as we are doing with this. 


---

_Review comment by @gregersn on `crates/ruff/src/message/gitlab.rs`:91 on 2023-09-07 07:37_

My thinking is that "value for money", just doing this will be a lot less annoying and give a lot less noise in the gitlab reports, than having code position included, which would trigger for almost no reason. At least with this method the false reporting of fixed/new issues will either be either when a new issue is introduced, which has the same name, or an issue which there are multiples are with the same name, is fixed. 

---

_@gregersn reviewed on 2023-09-07 07:37_

---

_@MichaReiser reviewed on 2023-09-07 08:24_

---

_Review comment by @MichaReiser on `crates/ruff/src/message/gitlab.rs`:91 on 2023-09-07 08:24_

The main "issue" I see with the heuristic is that GitLab will mark the wrong diagnostic as fixed if there's a hash collision, but I haven't developed a better heuristic. 

Let's say we have two unused variable diagnostics in a file:

```python
x = 1
y = 2
```

There's a hash collision for `x` and `y`, so `y` performs a second hashing round

and we now fix the first violation

```python
x = 1
y = 2
print(x)
```

There's no longer a hash collision for `y`, meaning that the diagnostic for `y` now gets the hash (fingerprint) of the violation that used to be for `x`. 

We could consider including more information when a hash collision occurs but not sure what information that would be that doesn't suffer from the original issue and distinguishes multiple diagnostics sufficiently.



---

_@MichaReiser approved on 2023-09-07 08:25_

---

_Merged by @MichaReiser on 2023-09-08 06:25_

---

_Closed by @MichaReiser on 2023-09-08 06:25_

---
