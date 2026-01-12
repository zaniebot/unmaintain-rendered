```yaml
number: 15009
title: "`pydocstyle` rules should follow naming convention"
type: issue
state: closed
author: dylwil3
labels:
  - documentation
  - good first issue
  - docstring
assignees: []
created_at: 2024-12-16T03:44:27Z
updated_at: 2024-12-23T21:49:53Z
url: https://github.com/astral-sh/ruff/issues/15009
synced_at: 2026-01-12T15:54:54Z
```

# `pydocstyle` rules should follow naming convention

---

_@dylwil3_

The following rules should be renamed to conform to Ruff's [naming convention](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#rule-naming-convention):

- `fits-on-one-line` (`D200`)
- `no-blank-line-before-function` (`D201`)
- `no-blank-line-after-function` (`D202`)
- `one-blank-line-before-class` (`D203`)
- `one-blank-line-after-class` (`D204`)
- `blank-line-after-summary` (`D205`)
- `indent-with-spaces` (`D206`)
- `section-not-overindented` (`D214`)
- `section-underline-not-overindented` (`D215`)
- `ends-in-period` (`D400`)
- `no-signature` (`D402`)
- `capitalize-section-name` (`D405`)
- `new-line-after-section-name` (`D406`)
- `dashed-underline-after-section` (`D407`)
- `section-underline-after-name` (`D408`)
- `section-underline-matches-section-length` (`D409`)
- `blank-line-after-last-section` (`D413`)
- `ends-in-punctuation` (`D415`)
- `section-name-ends-in-colon` (`D416`)

[Apologies if any of these are in error - double negating on the fly may have spun my head around]

---

_Label `documentation` added by @dylwil3 on 2024-12-16 03:44_

---

_Label `good first issue` added by @dylwil3 on 2024-12-16 03:44_

---

_Comment by @dhruvmanila on 2024-12-16 07:26_

We'll also need to add redirects for the documentation website.

---

_Label `docstring` added by @AlexWaygood on 2024-12-16 08:00_

---

_Comment by @enochkan on 2024-12-20 23:27_

Hi all! I'd like to work on this issue. 

Is the idea to rename `no-blank-line-before-function` to `blank-line-before-function` since it highlights the pattern being linted against? 

---

_Assigned to @enochkan by @dylwil3 on 2024-12-21 04:41_

---

_Comment by @dylwil3 on 2024-12-21 04:41_

Right! You want it to be grammatically correct to say "allow (rule name)" if you want to turn off the lint rule. 

And great, thanks for your help!

---

_Comment by @enochkan on 2024-12-21 15:20_

No problem @dylwil3 happy to help :) 

I'll confirm the naming conventions with you before making any changes. 

---

_Comment by @enochkan on 2024-12-21 17:15_

| **Code** | **Old Name**                             | **New Name**                              | **Rationale**                                                                                                                      |
|----------|------------------------------------------|-------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------|
| D200     | fits-on-one-line                         | unnecessary-multiline-docstring           | Flags docstrings that should fit on one line but are written over multiple lines.                                                  |
| D201     | no-blank-line-before-function            | blank-line-before-function                | Flags the presence of a blank line before a function definition.                                                                   |
| D202     | no-blank-line-after-function             | blank-line-after-function                 | Flags the presence of a blank line immediately after a function definition.                                                        |
| D203     | one-blank-line-before-class              | incorrect-blank-line-before-class         | Flags incorrect blank lines (missing or extra) before a class definition.                                                          |
| D204     | one-blank-line-after-class               | incorrect-blank-line-after-class          | Flags incorrect blank lines (missing or extra) after a class definition.                                                           |
| D205     | blank-line-after-summary                 | missing-blank-line-after-summary          | Flags a docstring summary line that is not followed by a blank line.                                                               |
| D206     | indent-with-spaces                       | docstring-tab-indentation                           | Flags the use of tabs where spaces are required (e.g., in docstring indentation).                                                  |
| D214     | section-not-overindented                 | overindented-section                      | Flags a docstring section that is indented more than the surrounding docstring lines.                                             |
| D215     | section-underline-not-overindented       | overindented-section-underline            | Flags a section underline that is indented more than the section text itself.                                                      |
| D400     | ends-in-period                           | missing-trailing-period                   | Flags a docstring that does not end with a period.                                                                                 |
| D402     | no-signature                             | signature-in-docstring                    | Flags function signatures that appear inside docstrings.                                                                           |
| D405     | capitalize-section-name                  | non-capitalized-section-name              | Flags docstring section names that do not start with a capital letter.                                                             |
| D406     | new-line-after-section-name              | missing-new-line-after-section-name       | Flags the absence of a newline immediately after a docstring section name.                                                         |
| D407     | dashed-underline-after-section           | missing-dashed-underline-after-section    | Flags a docstring section without the required dashed underline beneath it.                                                        |
| D408     | section-underline-after-name             | missing-section-underline-after-name      | Flags a missing section underline directly after a docstring section name.                                                         |
| D409     | section-underline-matches-section-length | mismatched-section-underline-length       | Flags an underline whose length does not match its section name length.                                                            |
| D413     | blank-line-after-last-section            | missing-blank-line-after-last-section     | Flags the lack of a blank line after the final docstring section.                                                                  |
| D415     | ends-in-punctuation                      | missing-terminal-punctuation              | Flags docstrings that do not end with the required punctuation (e.g., a period).                                                   |
| D416     | section-name-ends-in-colon               | missing-section-name-colon                | Flags a docstring section name that does not end in a colon.                                                                       |



@dylwil3 thoughts?

---

_Comment by @dylwil3 on 2024-12-22 18:35_

Looks good to me, very thorough @enochkan ! I'd welcome a PR - let me know if you need help sorting out where to change the name. Thanks again!

---

_Comment by @enochkan on 2024-12-22 21:58_

@dylwil3 actually I probably need some help :P 

did a quick search and found most occurrences in `faq.md`? 

<img width="603" alt="Image" src="https://github.com/user-attachments/assets/2c89c160-81cf-4963-92a8-d6966e3762ca" />



---

_Comment by @enochkan on 2024-12-22 22:03_

oh no, I'm guessing all the occurrences in test cases need to change as well:

<img width="1020" alt="Image" src="https://github.com/user-attachments/assets/9de69316-269a-4a7f-b6af-282efaccfe85" />


---

_Comment by @enochkan on 2024-12-22 22:18_

I've found occurrences in these places so far:

| **Code** | **Old Name**                             | **Occurring in**                                                       |
|----------|------------------------------------------|------------------------------------------------------------------------|
| D200     | fits-on-one-line                         | codes.rs, definitions.rs, mod.rs, one_liner.rs                         |
| D201     | no-blank-line-before-function            | codes.rs, definitions.rs, mod.rs, one_liner.rs, blank_before_after_function.rs, check_docs_formatted.py |
| D202     | no-blank-line-after-function             | codes.rs, definitions.rs, mod.rs, one_liner.rs, blank_before_after_function.rs, D202.py |
| D203     | one-blank-line-before-class              | format.rs, codes.rs, definitions.rs, mod.rs, settings.rs, blank_before_after_class.rs, integration_test.rs, registry.rs, faq.md, check_docs_formatted.py |
| D204     | one-blank-line-after-class               | codes.rs, definitions.rs, mod.rs, settings.rs, blank_before_after_class.rs, faq.md, check_docs_formatted.py      |
| D205     | blank-line-after-summary                 | codes.rs, definitions.rs, mod.rs, blank_after_summary.rs                        |
| D206     | indent-with-spaces                       | format.rs, codes.rs, definitions.rs, mod.rs, indent.rs, formatter.md, check_docs_formatted.py                          |
| D214     | section-not-overindented                 | codes.rs, definitions.rs, mod.rs, settings.rs, sections.rs, faq.md                      |
| D215     | section-underline-not-overindented       | codes.rs, definitions.rs, mod.rs, settings.rs, sections.rs, faq.md                      |
| D400     | ends-in-period                           | codes.rs, definitions.rs, mods.rs, settings.rs, ends_with_period.rs, faq.md             |
| D402     | no-signature                             | codes.rs, definitions.rs, mods.rs, settings.rs, no_signature.rs, faq.md                 |
| D405     | capitalize-section-name                  | codes.rs, definitions.rs, mods.rs, settings.rs, sections.rs                     |
| D406     | new-line-after-section-name              | codes.rs, definitions.rs, mods.rs, settings.rs, sections.rs, faq.md                     |
| D407     | dashed-underline-after-section           | codes.rs, definitions.rs, mods.rs, settings.rs, sections.rs, faq.md                     |
| D408     | section-underline-after-name             | codes.rs, definitions.rs, mods.rs, settings.rs, sections.rs, faq.md                     |
| D409     | section-underline-matches-section-length | codes.rs, definitions.rs, mods.rs, settings.rs, sections.rs, faq.md                     |
| D413     | blank-line-after-last-section            | codes.rs, definitions.rs, mods.rs, settings.rs, sections.rs                     |
| D415     | ends-in-punctuation                      | codes.rs, definitions.rs, mods.rs, settings.rs, ends_with_punction.rs, faq.md           |
| D416     | section-name-ends-in-colon               | codes.rs, definitions.rs, mods.rs, settings.rs, sections.rs, faq.md                    |




@dylwil3 anything else?

---

_Comment by @dylwil3 on 2024-12-22 22:32_

@enochkan Sure! Let's walk through the steps to change `fits-on-one-line` to `unnecessary-multiline-docstring`.

1. Change all occurrences of this struct name https://github.com/astral-sh/ruff/blob/3b27d5dbad18b2849834fe5c3736ed8b9c9ae2da/crates/ruff_linter/src/rules/pydocstyle/rules/one_liner.rs#L36
to `UnnecessaryMultilineDocstring`. Hopefully you have an IDE that will do most of this for you, otherwise you'll have to go (rip)grep hunting. (Or you can just keep running `cargo check` and letting it yell at you).
2. Optionally, change the name of the function that checks for the violation if it seems confusing (in this case the function's name is `one_liner` so it already isn't tied to the name of the rule, you can leave it as-is.)
3. A small selection of the documentation website is not auto-generated and may reference rules, which you found in this case in the FAQ. So you'll have to change those as well.
4. Probably a good idea to (rip)grep for other occurences of the name, both in hyphenated and camel case form, to see if anything was missed. It looks like you did this and found some spurious references in other files (e.g. scripts), so that's great.

If it helps here's an example PR implementing a name change: #12399

In any event, maybe it's easiest from here just to open a PR and we can hunt for any missing changes from there - the code compiling will already be a positive signal.

Thanks for your diligence!

---

_Comment by @enochkan on 2024-12-22 22:47_

@dylwil3 added https://github.com/astral-sh/ruff/pull/15102.

---

_Closed by @dylwil3 on 2024-12-23 21:48_

---

_Closed by @dylwil3 on 2024-12-23 21:48_

---

_Comment by @dylwil3 on 2024-12-23 21:49_

@charliermarsh I think you'll have to do the redirects, right? I believe you can do a bulk redirect by uploading the attached CSV

[redirects.csv](https://github.com/user-attachments/files/18233630/redirects.csv)


---
