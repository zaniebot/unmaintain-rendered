---
number: 16139
title: RUF054 checks only the first form feed on a physical line
type: issue
state: open
author: dscorbett
labels:
  - bug
  - preview
assignees: []
created_at: 2025-02-13T15:16:29Z
updated_at: 2025-03-01T16:55:49Z
url: https://github.com/astral-sh/ruff/issues/16139
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF054 checks only the first form feed on a physical line

---

_Issue opened by @dscorbett on 2025-02-13 15:16_

### Description

In Ruff 0.9.6, [`indented-form-feed` (RUF054)](https://docs.astral.sh/ruff/rules/indented-form-feed/) only checks the first form feed on a line. If the first form feed is okay, but a later one appears within the leading white space, there is a false negative.
```console
$ printf 'if True:\n\f \f print("!")\n' | ruff --isolated check --preview - --select RUF054
All checks passed!
$ printf 'if True:\n\f\f print("!")\n' | ruff --isolated check --preview - --select RUF054
All checks passed!
```
RUF054 checks physical lines. The restriction on form feeds only applies within indentation, so the relevant lines are logical lines. If a form feed appears within leading white space after a line continuation, there is a false positive.
```console
$ printf 'if True:\\\n    \fprint("!")\n' | ruff --isolated check --preview - --select RUF054 --output-format concise
-:2:5: RUF054 Indented form feed
Found 1 error.
```

---

_Label `bug` added by @MichaReiser on 2025-02-13 15:29_

---

_Label `preview` added by @MichaReiser on 2025-02-13 15:29_

---

_Comment by @VascoSch92 on 2025-02-23 22:21_

Can i give a shot?;)

---

_Assigned to @VascoSch92 by @ntBre on 2025-02-24 00:06_

---

_Comment by @VascoSch92 on 2025-02-24 19:02_

@dscorbett I have just a couple of questions to be sure that I understand good the problems.

1. `'if True:\n\f \f print("!")\n'` -> this should raise a violation as there is a white space before the second` \f`. Correct?
2. `'if True:\n\f\f print("!")\n'` -> this should not raise a violation as there is a `\f `before the second `\f`. Correct?
3. `'if True:\\\n    \fprint("!")\n'` Why is this a False Positive? The `\\\n` means a logical line, but the `\f` has a white space before, so it should raise a violation. Right?

Thanks :-) 

---

_Comment by @dscorbett on 2025-02-24 19:42_

1. Correct.
2. This should raise a violation. The Python Language Reference says “A formfeed character may be present at the start of the line; it will be ignored for the indentation calculations above. Formfeed characters occurring elsewhere in the leading whitespace have an undefined effect”. The first form feed is at the start of the line, so it fine. The second form feed is _not_ at the start of the line, because it follows the first form feed, which is leading white space. I admit this is an edge case and I am not sure what the original intent was for a case like this, but it seems safest to warn against it. If someone really needs two consecutive form feeds they can interpose a newline.
3. This code is two physical lines but one logical line, equivalent to `'if True:    \fprint("!")\n'`. The form feed does not follow indentation, so it is a false positive. Spaces are only indentation at the start of a logical line.

---

_Comment by @VascoSch92 on 2025-02-25 13:35_

@dscorbett Thank you very much for your answer. Now is much clear.

So a situation where I have:` 'if True:    \t\fprint("!")\n'` should rise an error as the form feed comes after a tab. 


---

_Comment by @dscorbett on 2025-02-25 14:00_

No, `'if True:    \t\fprint("!")\n'` should not get an error: the form feed does not appear after leading white space. A form feed is only problematic after leading white space, not just any white space.

---

_Comment by @VascoSch92 on 2025-02-25 15:03_

I think that the two bugs

```
$ printf 'if True:\n\f \f print("!")\n' | ruff --isolated check --preview - --select RUF054
All checks passed!
$ printf 'if True:\n\f\f print("!")\n' | ruff --isolated check --preview - --select RUF054
All checks passed!
```

are easily fixable, but the problem with logical lines is much harder and require changes outside the crate (or at least I think). Let take as example the snippet

```python
if True:\
    print("!")
```
which is the third example provided in the first post of the issue, i.e., 'if True:\\\n    \fprint("!")'.
In the current setting: this will be always a False Positive. 

Why ?
The `pub(crate) fn indented_form_feed(line: &Line)` will receive first the line `Line { text: "if True:\\\n", offset: 519 }` and then the line `Line { text: "    \u{c}print(\"!\")\n", offset: 529 }` separately. So it is not possible to assert that the second line is valid without knowing the first one. (Note that the second line alone is not valid, i.e., will raise a violation).

What do you think? I'm missing something or ?

---

_Comment by @dscorbett on 2025-02-25 21:57_

Could you fix the third example by moving the rule from physical_lines.rs to logical_lines.rs, and making it follow the pattern of the other rules in logical_lines.rs?

---

_Comment by @VascoSch92 on 2025-02-26 07:20_

> Could you fix the third example by moving the rule from physical_lines.rs to logical_lines.rs, and making it follow the pattern of the other rules in logical_lines.rs?

Ah yes... nice... still not so familiar with the code base and how really the Ruff works under the hood :-) Thanks for the idea. 

@MichaReiser I need some guidance: I moved the rule from `physical_lines.rs` to `logical_lines.rs`. Should I also change something to make the tests work correctly, or I have just to do that?

---

_Comment by @ntBre on 2025-02-26 16:53_

I think [the test](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/ruff/mod.rs#L437) should still work. The one thing to watch out for is that some functions, and it looks like `check_logical_lines` is one of them, have a `settings.rules.any_enabled([...])` check at the top in addition to specific rule checks farther down.

You could also add a test case that you expect to fail and make sure it fails when you run the tests, just to be sure. That's what I usually do.

---

_Comment by @VascoSch92 on 2025-02-28 15:17_

I cannot trigger the test in `logical_lines.rs`.

Precisely, I left the tests as they are as suggested and I moved the rule from `check_physical_lines` in `physical_lines.rs` to `check_logical_lines` in `logical_lines.rs`. Moreover, I enabled the rule at the top of the method with : `let enforce_indented_form_feed = settings.rules.any_enabled(&[Rule::IndentedFormFeed]);`. But when I ran the tests, the rule is not enabled. It seems that the rules in preview are not enabled in `check_logical_lines`.

Is it possible? or I'm doing something wrong?



---

_Comment by @ntBre on 2025-02-28 19:04_

> It seems that the rules in preview are not enabled in `check_logical_lines`.

Hmm, it could be something to do with this. Otherwise I'm not sure. Could you open a draft PR so that I can have a look? Or a link to a branch in a fork would work too

---

_Comment by @VascoSch92 on 2025-03-01 16:55_

You can find the draft [here](https://github.com/astral-sh/ruff/pull/16452).


---
