```yaml
number: 14748
title: "[`pycodestyle`] Handle f-strings properly for `invalid-escape-sequence (W605)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: escape-f
created_at: 2024-12-03T05:54:22Z
updated_at: 2024-12-04T12:59:20Z
url: https://github.com/astral-sh/ruff/pull/14748
synced_at: 2026-01-12T15:55:48Z
```

# [`pycodestyle`] Handle f-strings properly for `invalid-escape-sequence (W605)`

---

_@dylwil3_

When fixing an invalid escape sequence in an f-string, each f-string element is analyzed for valid escape characters prior to creating the diagnostic and fix. This allows us to safely prefix with `r` to create a raw string if no valid escape characters were found anywhere in the f-string, and otherwise insert backslashes.

This fixes a bug in the original implementation: each "f-string part" was treated separately, so it was not possible to tell whether a valid escape character was or would be used elsewhere in the f-string.

Progress towards #11491 but format specifiers are not handled in this PR.



---

_Label `bug` added by @dylwil3 on 2024-12-03 05:57_

---

_Label `fixes` added by @dylwil3 on 2024-12-03 05:57_

---

_Comment by @github-actions[bot] on 2024-12-03 06:00_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:271 on 2024-12-03 09:46_

Nit: The method name is somewhat long and doesn't tell me much. I'd probably just inline the code into `check`, considering that `check` only contains one additional function call

---

_@MichaReiser approved on 2024-12-03 09:48_

This makes sense and roughly matches what we do in the implicit concatenated string formatting. 

Do we need to have any special handling for escapes in format specifiers or the debug text parts of an f-string? We don't have to fix this as part of this PR but format specifiers and debug texts were something that caused me a a lot of pain when working on f-string formatting 

---

_@dylwil3 reviewed on 2024-12-03 15:35_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:271 on 2024-12-03 15:35_

Happy to change the method name!

I don't think I can inline this into `check` though. Right now `check` is the composite of `analyze_escape_chars` and `check_from_escape_chars_state`. For ordinary strings we can just do `check` all at once, but the purpose of the refactoring was that, for f-strings, I need to do `analyze_escape_chars` on each f-string part _before_ doing `check_from_escape_chars_state`. 

Sorry if I'm misunderstanding the suggestion!

I'll try to add some comments around this and think of a better name.

---

_@MichaReiser reviewed on 2024-12-03 22:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:271 on 2024-12-03 22:01_

Ah I see. I'd be inclined to inline `check` into the string literal and byte literal branches (it's really just that one `analyze` call). Actually, I'd probably rewrite it to 


```rust
				let state = match part {
            StringLikePart::String(_) | StringLikePart::Bytes(_) => {
							analyze_escape_chars(locator, part.range(), part.flags())
            }
            StringLikePart::FString(f_string) => {
                let flags = AnyStringFlags::from(f_string.flags);
                let mut escape_chars_state = EscapeCharsState::default();
                for element in &f_string.elements {
                    match element {
                        FStringElement::Literal(literal) => {
                            escape_chars_state.update(analyze_escape_chars(
                                locator,
                                literal.range(),
                                flags,
                            ));
                        }
                        FStringElement::Expression(expression) => {
                            let Some(format_spec) = expression.format_spec.as_ref() else {
                                continue;
                            };
                            for literal in format_spec.elements.literals() {
                                escape_chars_state.update(analyze_escape_chars(
                                    locator,
                                    literal.range(),
                                    flags,
                                ));
                            }
                        }
                    }

								escape_chars_state
                }
            }
				};

        check(
            &mut checker.diagnostics,
            locator,
            part.start(),
            part.range(),
            part.flags(),
						state
        );

```

---

_@MichaReiser reviewed on 2024-12-03 22:01_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:271 on 2024-12-03 22:01_

Finger crossed that part has all these methods

---

_@dylwil3 reviewed on 2024-12-04 02:08_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/pycodestyle/rules/invalid_escape_sequence.rs`:271 on 2024-12-04 02:08_

perfect, much better!

---

_@MichaReiser approved on 2024-12-04 07:25_

---

_Comment by @dylwil3 on 2024-12-04 12:34_

> Do we need to have any special handling for escapes in format specifiers or the debug text parts of an f-string? We don't have to fix this as part of this PR but format specifiers and debug texts were something that caused me a a lot of pain when working on f-string formatting

As far as I can tell you can't really put an escape in either of those places: for format specifiers I think that's a [syntax error](https://docs.python.org/3/library/string.html#format-specification-mini-language) and for debug text I think you could only do that inside a nested string (e.g. `f"{'\InHere'=}"`), which is handled fine (added test to fixture).

But maybe I missed something so if someone comes up with an example I can do a followup PR.

Thank you for the thoughtful review!


---

_Comment by @MichaReiser on 2024-12-04 12:42_

```
>>> a = 10
>>> f"{a:\xFF}"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ValueError: Unknown format code '\xff' for object of type 'int'
```

I think you can have arbitrary escapes but the above is recognized as a hex string. What you linked to is only the list of standard format specifiers but an object can implement support for arbitrary format specifiers

---

_Comment by @dylwil3 on 2024-12-04 12:58_

> What you linked to is only the list of standard format specifiers but an object can implement support for arbitrary format specifiers

Terrifying.

Yes, this doesn't seem quite right when I make the escape invalid:

```console
➜  cargo run -p ruff -- check tmp.py --no-cache --select W605 --fix --diff
--- tmp.py
+++ tmp.py
@@ -1,2 +1,2 @@
 a = 10
-f"{a:\Xff}"
+rf"{a:\Xff}"

Would fix 1 error.
```
I'll leave the linked issue open and do a followup PR once I understand better how these things work...

Thank you!


---

_Merged by @dylwil3 on 2024-12-04 12:59_

---

_Closed by @dylwil3 on 2024-12-04 12:59_

---

_Branch deleted on 2024-12-04 12:59_

---
