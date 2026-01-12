```yaml
number: 3122
title: "[pycodestyle] trailing-whitespace, blank-line-contains-whitespace (W291, W293)"
type: pull_request
state: merged
author: mknaw
labels: []
assignees: []
merged: true
base: main
head: pycodestyle-trailing-whitespace
created_at: 2023-02-22T13:17:51Z
updated_at: 2023-02-24T00:04:45Z
url: https://github.com/astral-sh/ruff/pull/3122
synced_at: 2026-01-12T15:55:12Z
```

# [pycodestyle] trailing-whitespace, blank-line-contains-whitespace (W291, W293)

---

_@mknaw_

- [x] failing checks
- [x] ~follow pycodestyle precedent on what constitutes whitespace~

---

_Comment by @matthewlloyd on 2023-02-22 19:05_

Is this code Unicode-safe? Rust strings behave in unexpected ways when there are Unicode characters, and there are in fact a few whitespace characters that are non-ASCII (e.g. '\u{A0}').

For example, if Unicode characters are present, the byte count does not equal the character count, so `line.chars().count() != line.len()` and `if whitespace_count == line.len()` will break. I'm not sure whether `Location.column` is a byte or character offset, but that could potentially be an issue also.

```
    let whitespace_count = line.chars().rev().take_while(|c| c.is_whitespace()).count();
    if whitespace_count > 0 {
        let start = Location::new(lineno + 1, line.len() - whitespace_count);
        let end = Location::new(lineno + 1, line.len());

        if whitespace_count == line.len() {
```

May be worth adding some tests with UTF-8 encoded (as that's Python's default encoding) lines with some whitespace and non-whitespace Unicode characters to do some TDD and make sure everything works as expected.


---

_Comment by @charliermarsh on 2023-02-22 19:09_

(`Location.column` is a character offset, not a byte offset.)

---

_@charliermarsh reviewed on 2023-02-22 19:41_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/pycodestyle/W293.py`:10 on 2023-02-22 19:41_

We should copy over `testsuite/W29.py` from pycodestyle.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/trailing_whitespace.rs`:49 on 2023-02-22 19:46_

All `line.len()` usages here should be based on `line.chars().count()` instead.

---

_@charliermarsh reviewed on 2023-02-22 19:46_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/physical_lines.rs`:158 on 2023-02-22 19:48_

For consistency, let's define these up by `fix_shebang_whitespace` using the same pattern. 

---

_@charliermarsh reviewed on 2023-02-22 19:48_

---

_Comment by @charliermarsh on 2023-02-23 01:49_

Looking good, thank you!

---

_Comment by @mknaw on 2023-02-23 01:52_

> Is this code Unicode-safe?

It appears that characters such as `\uA0` are not [valid whitespace](https://docs.python.org/3/reference/lexical_analysis.html#whitespace-between-tokens). So, although they can be used in literals, a line such as `print('foo')\uA0` isn't valid python syntax in the first place.

This does however raise an interesting point; it appears that `pycodestyle` also checks this lint based on "physical lines," so perhaps questionably will flag whitespace at the end of a given line in a multiline literal. Worse still, as written, this PR will flag a line of a multline literal ending with an `\uA0` whereas pycodestyle will not. Perhaps for feature parity I should restrict the check to just those characters from `pycodestyle` rather than the more extensive `is_whitespace` list?

---

_Comment by @charliermarsh on 2023-02-23 01:55_

Yeah I did read that source and asked myself the same question. I was originally going to comment that this shouldn't be done on physical lines for that very reason RE multiline strings, but then saw that pycodestyle was using physical lines so I just ignored it. (FWIW we _could_ omit lines that are included in multiline strings -- we'd probably want to track those lines on `Indexer`, similar to how we track commented-out lines, then omit lines here that are "string lines". But I don't think it's a blocker to merging this.) I do think it makes sense to mimic their exact logic for trailing spaces though, if you're up for it. I doubt it matters much in practice, but it's good to be consistent where we can.


---

_Comment by @matthewlloyd on 2023-02-23 02:08_

> It appears that characters such as `\uA0` are not [valid whitespace](https://docs.python.org/3/reference/lexical_analysis.html#whitespace-between-tokens). So, although they can be used in literals, a line such as `print('foo')\uA0` isn't valid python syntax in the first place.

Aha, good to know! For what it's worth, `crates/ruff/src/ast/whitespace.rs` also uses Rust's broader notion of whitespace i.e. `char::is_whitespace`, so consistency within Ruff would argue in favor of continuing to use `is_whitespace` here. (Perhaps we should fix that and use Python's notion of whitespace e.g. by checking the CPython source since docs are sometimes incorrect?)

Another consideration in favor of continuing to use `is_whitespace` in this specific check is that while Python may treat `\uA0` as non-whitespace therefore making it a syntax error at the end of a line, in practice, we would probably still want to autofix and trim it away if a user accidentally inserts it (or some other invisible whitespace non-space char!).

I can't imagine `\uA0` needing to appear at the ends of lines particularly often, and while there are perhaps specific circumstances where that might be useful, e.g. in i18n multiline strings, I would guess that's a rare case compared to accidentally pressing some combination of keys that inserts "\<whitespace block\>"...

---

_Comment by @charliermarsh on 2023-02-23 02:10_

Yeah that's a good argument in favor of keeping it as-is. Honestly I could go either way -- I'll defer to the author in this case @mknaw, totally up to you.

---

_Comment by @matthewlloyd on 2023-02-23 02:13_

> CPython source

https://github.com/python/cpython/blob/main/Parser/tokenizer.c#L1597

```python
    /* Skip spaces */
    do {
        c = tok_nextc(tok);
    } while (c == ' ' || c == '\t' || c == '\014');
```

\014 is Control-L (formfeed).

---

_Comment by @charliermarsh on 2023-02-23 21:31_

@mknaw - I say we merge this as-is -- ok with you? Wdyt?

---

_Comment by @mknaw on 2023-02-23 23:06_

Sounds reasonable to me 👍

---

_Merged by @charliermarsh on 2023-02-24 00:04_

---

_Closed by @charliermarsh on 2023-02-24 00:04_

---
