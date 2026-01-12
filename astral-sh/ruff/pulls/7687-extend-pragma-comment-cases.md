```yaml
number: 7687
title: Extend pragma comment cases
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/pragma
created_at: 2023-09-28T02:06:41Z
updated_at: 2023-09-28T19:03:13Z
url: https://github.com/astral-sh/ruff/pull/7687
synced_at: 2026-01-12T15:55:24Z
```

# Extend pragma comment cases

---

_@charliermarsh_

## Summary

Extends the pragma comment detection in the formatter to support case-insensitive `noqa` (as supposed by Ruff), plus a variety of other pragmas (`isort:`, `nosec`, etc.).

Also extracts the detection out into the trivia crate so that we can reuse it in the linter (see: https://github.com/astral-sh/ruff/issues/7471).

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-28 02:06_

---

_Review requested from @konstin by @charliermarsh on 2023-09-28 02:06_

---

_Label `formatter` added by @charliermarsh on 2023-09-28 02:06_

---

_@charliermarsh reviewed on 2023-09-28 02:07_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/pragmas.rs`:30 on 2023-09-28 02:07_

Wonder if we should use aho-corasick or similar here.

---

_Comment by @codspeed-hq[bot] on 2023-09-28 02:15_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/pragma)

### Merging #7687 will **not alter performance**

<sub>Comparing <code>charlie/pragma</code> (df2f4be) with <code>main</code> (1c02fcd)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-09-28 03:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/pragmas.rs`:30 on 2023-09-28 07:07_

It feels a bit overkill to me to add such a heavy dependency for one (not very hot) code path

---

_@MichaReiser reviewed on 2023-09-28 07:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/trailing_comments.py`:3 on 2023-09-28 07:11_

I'm not sure how I feel about this... Our documentation is relatively clear that it should be written all lower case. Supporting multiple casing will slow down all our tooling, for no obvious benefit. 

It also opens the question why we only support different casing for `noqa` but not `type`, `pyright`, etc...

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/pragmas.rs`:13 on 2023-09-28 07:13_

Nit

```suggestion
pub fn is_pragma_comment(comment: &str) -> bool {
```

To make it clear that it tests the whole comment and isn't just testing whether `text` is a pragma keyword

---

_Review comment by @MichaReiser on `crates/ruff_python_trivia/src/pragmas.rs`:25 on 2023-09-28 07:15_

```suggestion
    matches!(
        trimmed.as_bytes(),
        [b'n' | b'N', b'o' | b'O', b'q' | b'Q', b'a' | b'A', ..] | [b'n', b'o', b's', b'e', b'c', ..]
    ) ||
```

---

_@MichaReiser approved on 2023-09-28 07:16_

---

_@konstin approved on 2023-09-28 10:49_

Do those mixed case comments come from some specific projects that uses them?

---

_@charliermarsh reviewed on 2023-09-28 14:09_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/pragmas.rs`:25 on 2023-09-28 14:09_

`nosec` doesn't support case-insensitive matching! I checked :)

---

_@charliermarsh reviewed on 2023-09-28 14:57_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/trailing_comments.py`:3 on 2023-09-28 14:57_

The answer is that it depends on what the tools themselves support, and Flake8 decided that its pragma should be case-insensitive. The same is not true of the others, even `nosec`.

We continue to support these for compatibility and I think it's important to support _at least_ `# NOQA`:

- `# noqa`: 1.7M files ([source](https://github.com/search?type=code&q=%2F%28%3F-i%29noqa%2F+language%3APython))
- `# NOQA`: 80k files ([source](https://github.com/search?type=code&q=%2F%28%3F-i%29NOQA%2F+language%3APython))
- `# NoQA`: 1.5k files ([source](https://github.com/search?type=code&q=%2F%28%3F-i%29NoQA%2F+language%3APython))

(That being said, the linter _already_ supports these so this logic should be consistent with that decision.)


---

_Comment by @charliermarsh on 2023-09-28 18:40_

@konstin - It's mostly just Sphinx and EdgeDB.

---

_@charliermarsh reviewed on 2023-09-28 18:42_

---

_Review comment by @charliermarsh on `crates/ruff_python_trivia/src/pragmas.rs`:30 on 2023-09-28 18:42_

I'm fine not adding it, but isn't this a fairly hot code path? It's run for every end-of-line comment.

---

_@charliermarsh reviewed on 2023-09-28 18:43_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/trailing_comments.py`:3 on 2023-09-28 18:43_

We could also consider only supporting `noqa`, `NOQA`, and `NoQA`.

---

_Merged by @charliermarsh on 2023-09-28 18:55_

---

_Closed by @charliermarsh on 2023-09-28 18:55_

---

_Branch deleted on 2023-09-28 18:55_

---
