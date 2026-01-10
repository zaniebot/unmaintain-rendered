```yaml
number: 18882
title: Fix f-string interpolation escaping
type: pull_request
state: merged
author: MeGaGiGaGon
labels:
  - fixes
assignees: []
merged: true
base: main
head: fix-fstring-interpolation-escaping
created_at: 2025-06-23T05:53:33Z
updated_at: 2025-06-25T15:49:14Z
url: https://github.com/astral-sh/ruff/pull/18882
synced_at: 2026-01-10T18:39:09Z
```

# Fix f-string interpolation escaping

---

_Pull request opened by @MeGaGiGaGon on 2025-06-23 05:53_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #18742

Since I wasn't very clear about the actual problem in #18742, here's it re-explained:

When ever an f-string contains a slash, it will get re-escaped, causing quadratic slash growth depending on the depth of f-string nesting.

Note: For all these examples I'll use `SIM201` since it's very easy to trigger, but this applies to any rule that uses AST-based changes and can contain f-strings.
Note: I'm using the playground instead of the command line to get these examples, since they generate syntax errors and thus don't show the broken code.
Note: I'm targeting python 3.12 for all of this, since all multiline-string-inside-f-string newline -> `\n` conversions instantly cause a syntax error pre-3.12, and this PR does not fix that issue.

[Examples in the playground](https://play.ruff.rs/6189667c-b3d8-44b2-a9c9-8b4b4489d2ef)
```py
not f"{"\\"}" == 1
# Fixes to
f'{"\\\\"}' != 1

not f"{f"{"\\"}"}" == 1
# Fixes to
f"{f'{\"\\\\\\\\\"}'}" != 1

not f"{f"{f"{"\\"}"}"}" == 1
# Fixes to
f"{f\"{f'{\\\"\\\\\\\\\\\\\\\\\\\"}'}\"}" != 1
```

This is where the original bug I found in #18742 came from:
The code
```py
f"{1=
}"
```
Because of the `=`, it gets written verbatim to the code generator's internal buffer as `f"{1=\n}"`, and then because of the code bug gets string encoded, leading to a buffer of `f"{1=\\n}"`, which is the syntax error I originally found.

The issue came from passing the generated string of the f-string node through the normal string handler. My assumption is this was to save on code duplication/having to re-do the string prefix, but it causes any backslashes inside the f-string to be double escaped.

This PR fixes that by giving the f-string string parts their own specialized code for only outputting the string body without the quotes.

Note: It looks like there are a couple different ways of adding the output to the buffer: `self.p`, `write!(self.buffer`, and `self.buffer +=`. I chose ones I thought might fit, but I'm not sure of the difference between all of them, or if I should have chosen differently.

## Test Plan

<!-- How was it tested? -->

Added a test to the `self_documenting_fstring` section

---

_Comment by @github-actions[bot] on 2025-06-23 06:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-06-23 07:32_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1944 on 2025-06-23 07:32_

```suggestion
            ); // https://github.com/astral-sh/ruff/issues/18742
```

`assert_roundtrip` already replaces the newlines with the platform's defualt.

---

_@MeGaGiGaGon reviewed on 2025-06-23 08:12_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_python_codegen/src/generator.rs`:1944 on 2025-06-23 08:12_

That's what I initially started out with, but it fails with
```
thread 'generator::tests::self_documenting_fstring' panicked at crates\ruff_python_codegen\src\generator.rs:1939:9:
assertion `left == right` failed
  left: "f\"{1=\n}\""
 right: "f\"{1=\r\n}\""
```
I also tried
```rs
        assert_eq!(
            round_trip_with(
                &Indentation::new("".to_string()),
                LineEnding::default(),
                &r#"
f"{1=
}"
"#
                .trim(),
            ),
            r#"
f"{1=
}"
"#
            .trim()
            .replace('\n', LineEnding::default().as_str())
        ); // https://github.com/astral-sh/ruff/issues/18742
```
but that fails with the same error, and also the same thing if I remove the end `.replace`

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1944 on 2025-06-23 08:25_

The failing test after removing all the `LineEnding` conversions before calling `assert_round_trip` looks correct to me. It reveals that the round-tripped code uses `\n` instead of windows line endings. 

I think the reason for this is that `debug_text` gets normalized by the parser. Meaning, ut sues `\n` everywhere. 

---

_@MichaReiser reviewed on 2025-06-23 08:25_

---

_@MeGaGiGaGon reviewed on 2025-06-23 08:32_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_python_codegen/src/generator.rs`:1944 on 2025-06-23 08:32_

I figured out part of what was going wrong, I had a bunch of duplicate tests everywhere due to coding on my phone. 
I don't think that's the case? Since in the original issue on the playground which uses CRLF endings the code was ending up as `\\r\\n`

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1951 on 2025-06-23 08:39_

```suggestion
				// https://github.com/astral-sh/ruff/issues/18742
        assert_eq!(
            round_trip(
                r#"
f"{1=
}"
"#
            ),
            r#"
f"{1=
}"
"#
            .trim()
        );
```

---

_@MichaReiser reviewed on 2025-06-23 08:39_

---

_@MichaReiser reviewed on 2025-06-23 08:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1944 on 2025-06-23 08:40_

Right, the debug text preserves the line ending. Can you explain why we can't use `assert_round_trip!`?

---

_@MeGaGiGaGon reviewed on 2025-06-23 16:54_

---

_Review comment by @MeGaGiGaGon on `crates/ruff_python_codegen/src/generator.rs`:1944 on 2025-06-23 16:54_

I think it's because round trip has the `.replace('\n', LineEnding::default().as_str())`. On the windows machine, `LineEnding::default().as_str()` is `\r\n`, but inside multiline strings/debug f-strings the encoding of the file is used instead without normalization, so trying to normalize the right side at all will cause the tests to fail, since it needs to be the exact same line endings as source, at least inside those two elements.
```
PS ~\Desktop\New_folder>Set-Content issue.py @"
>> not """`n""" == 1
>> not f"{1=`n}" == 1
>> not """`r`n""" == 1
>> not f"{1=`r`n}" == 1
>> not """`r""" == 1
>> not f"{1=`r}" == 1
>> "@
```
```
PS ~\Desktop\New_folder>Get-Escaped-Content issue.py
```
```
not """\n""" == 1\nnot f"{1=\n}" == 1\nnot """\r\n""" == 1\nnot f"{1=\r\n}" == 1\nnot """\r""" == 1\nnot f"{1=\r}" == 1\r\n
```
```
PS ~\Desktop\New_folder>uvx ruff check issue.py --isolated --select SIM
```
```snap
issue.py:1:1: SIM201 Use `"""\n""" != 1` instead of `not """\n""" == 1`
  |
1 | / not """
2 | | """ == 1
  | |________^ SIM201
3 |   not f"{1=
4 |   }" == 1
  |
  = help: Replace with `!=` operator

issue.py:3:1: SIM201 Use `f"{1=\n}" != 1` instead of `not f"{1=\n}" == 1`
  |
1 |   not """
2 |   """ == 1
3 | / not f"{1=
4 | | }" == 1
  | |_______^ SIM201
5 |   not """
6 |   """ == 1
  |
  = help: Replace with `!=` operator

issue.py:5:1: SIM201 Use `"""\r\n""" != 1` instead of `not """\r\n""" == 1`
  |
3 |   not f"{1=
4 |   }" == 1
5 | / not """
6 | | """ == 1
  | |________^ SIM201
7 |   not f"{1=
8 |   }" == 1
  |
  = help: Replace with `!=` operator

issue.py:7:1: SIM201 Use `f"{1=\r\n}" != 1` instead of `not f"{1=\r\n}" == 1`
  |
5 |   not """
6 |   """ == 1
7 | / not f"{1=
8 | | }" == 1
  | |_______^ SIM201
""" == 1t """
  |
  = help: Replace with `!=` operator

issue.py:9:1: SIM201 Use `"""\r""" != 1` instead of `not """\r""" == 1`
   |
 7 | not f"{1=
 8 | }" == 1
""" == 1 """
   | ^^^^^^^^^^^^^^^ SIM201
}" == 1t f"{1=
   |
   = help: Replace with `!=` operator

issue.py:11:1: SIM201 Use `f"{1=\r}" != 1` instead of `not f"{1=\r}" == 1`
   |
""" == 1 """
}" == 1t f"{1=
   | ^^^^^^^^^^^^^^^^ SIM201
   |
   = help: Replace with `!=` operator

Found 6 errors.
No fixes available (6 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

---

_Label `fixes` added by @MichaReiser on 2025-06-25 08:04_

---

_Comment by @MichaReiser on 2025-06-25 08:04_

Thanks, this makes sense!

---

_Merged by @MichaReiser on 2025-06-25 08:04_

---

_Closed by @MichaReiser on 2025-06-25 08:04_

---

_Branch deleted on 2025-06-25 15:49_

---
