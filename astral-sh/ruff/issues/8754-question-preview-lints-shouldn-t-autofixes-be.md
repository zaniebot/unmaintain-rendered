```yaml
number: 8754
title: "[question] Preview lints: shouldn't autofixes be unsafe?"
type: issue
state: closed
author: magnuslarsen
labels:
  - question
assignees: []
created_at: 2023-11-18T13:33:33Z
updated_at: 2023-11-18T18:01:01Z
url: https://github.com/astral-sh/ruff/issues/8754
synced_at: 2026-01-10T11:09:51Z
```

# [question] Preview lints: shouldn't autofixes be unsafe?

---

_Issue opened by @magnuslarsen on 2023-11-18 13:33_


As the title says, wouldn't the autofixes on in-preview lint-rules, inherently be unsafe? 

Take the following example (no pyproject.toml or alike):
```sh
mlar@cettisanger /tmp/ruff-preview-test> cat test.py
# E201 (preview, "safe" autofix)
print( "E201")
# E703 (not preview, safe autofix)
print("E703");
# RUF015 (not preview, unsafe autofix)
list([1, 2])[0]

mlar@cettisanger /tmp/ruff-preview-test> ruff check test.py --extend-select "RUF" --extend-select "E"
test.py:4:14: E703 [*] Statement ends with an unnecessary semicolon
test.py:6:1: RUF015 Prefer `next(iter([1, 2]))` over single element slice
Found 2 errors.
[*] 1 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).

mlar@cettisanger /tmp/ruff-preview-test [1]> ruff check test.py --extend-select "RUF" --extend-select "E" --preview
test.py:2:7: E201 [*] Whitespace after '('
test.py:4:14: E703 [*] Statement ends with an unnecessary semicolon
test.py:6:1: RUF015 Prefer `next(iter([1, 2]))` over single element slice
Found 3 errors.
[*] 2 fixable with the `--fix` option (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

What is being said here, is that:
* `E201`: The lint rule is "considered experimental or unstable", but the associated autofix is _safe_
* `E703`: The lint rule and associated autofix are both _stable_ (perfect)
* `RUF015`: The lint rule is _stable_ but the associated autofix is _unsafe_ (this makes sense)

It makes sense that an autofix can be considered safe (that is, it requires no changes before going out of preview), but if you attach it to a lint-rule that has, as an example, too many false-positives (which I assume falls under "experimental and unstable"), it is really _that_ safe?

I was at least under the assumption that in order to use an in-preview autofix, I would have the also specify `--unsafe-fixes`

---

_Comment by @zanieb on 2023-11-18 15:21_

Hi!

Fix safety has a different meaning than preview. Fix safety is akin to the confidence that the fix will not change the meaning of your code. Preview is used for new changes, both to explore the viability of rules and reduce the number of new diagnostics users have resolve on patch releases. The safe fixes on these preview rules are _intended_ to meet the standards of a safe fix. If they do not, we need that feedback before the rules are promoted to stable. I don't see an additional opt-in benefiting users. We use unsafe fixes for preview rules when the fix is _actually_ unsafe.

I'd recommend reading the applicability documentation at https://github.com/astral-sh/ruff/blob/main/crates/ruff_diagnostics/src/fix.rs#L12-L24
The preview documentation at https://docs.astral.sh/ruff/preview/
And the versioning discussion at https://github.com/astral-sh/ruff/discussions/6998

---

_Label `question` added by @zanieb on 2023-11-18 15:21_

---

_Comment by @magnuslarsen on 2023-11-18 18:01_

Thanks for the fast and detailed response!

The terminology just confused me. Being in preview and still considered "safe" was not working in my head :-)

Again thanks, also for ruff!



---

_Closed by @magnuslarsen on 2023-11-18 18:01_

---
