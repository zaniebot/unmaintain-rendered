```yaml
number: 21513
title: Show partial fixability indicator in statistics output
type: pull_request
state: merged
author: tsvikas
labels:
  - cli
assignees: []
merged: true
base: main
head: statistics-partial-fixable
created_at: 2025-11-18T16:21:10Z
updated_at: 2025-11-28T19:12:10Z
url: https://github.com/astral-sh/ruff/pull/21513
synced_at: 2026-01-12T15:57:26Z
```

# Show partial fixability indicator in statistics output

---

_@tsvikas_

## Summary

Fixes #20921

When running `ruff --statistics`, the fixability indicator now correctly shows whether violations are fully, partially, or not fixable:
- `[*]` when **all** violations of a rule are fixable
- `[~]` when **some** but not all violations are fixable  
- `[ ]` when **no** violations are fixable

The bug occurred because diagnostics were sorted by fixability before grouping, causing the first (unfixable) diagnostic to always be used as the representative for the group.

### JSON API Change

The JSON statistics output now includes `fixable_count` instead of `fixable` boolean:

```json
{
  "code": "UP035",
  "name": "deprecated-import",
  "count": 2,
  "fixable_count": 1
}
```

**Question for maintainers:** Should we keep the old `fixable` boolean field for backwards compatibility? If so, should it indicate "any fixable" (`fixable_count > 0`) or "all fixable" (`fixable_count == count`)?

## Test plan

- Updated existing snapshot tests for `show_statistics_json` and `show_statistics_json_unsafe_fixes`

**Question:** Are additional tests needed, e.g., for the partial fixability indicator `[~]`?

🤖 Generated with [Claude Code](https://claude.ai/code)

---

_Comment by @tsvikas on 2025-11-18 16:27_

a simple one-liner to see the effect: 
```
echo 'from typing import List, AsyncGenerator' | ruff check --select UP035 --statistics -
```

---

_Label `cli` added by @amyreese on 2025-11-18 17:17_

---

_Label `breaking` added by @MichaReiser on 2025-11-18 17:29_

---

_Comment by @MichaReiser on 2025-11-18 17:31_

Thank you for this contribution

Can you say more about why this PR also changes the field name and type? This change is not backward compatible and it's not clear to me how it's related.

I see that you wrote this PR with claude. We don't have a policy against using AI but I just want to ask you to review the PR yourself first before we spend time reviewing. I'll put it into draft, you can mark it as ready for review once you think the PR is in a good state.

---

_Converted to draft by @MichaReiser on 2025-11-18 17:31_

---

_Comment by @tsvikas on 2025-11-18 18:11_

> Can you say more about why this PR also changes the field name and type? This change is not backward compatible and it's not clear to me how it's related.

The field name and type change happen because the current `fixable: bool` field has undocumented and arguably unintuitive semantics.

**Current behavior (undocumented):**
- `fixable: true` only when **all** violations with that code are fixable
- `fixable: false` when **any** violation is not fixable

For example:
```bash
echo 'from typing import List, AsyncGenerator' | ruff check --select UP035 --statistics --output-format json -
```
Returns `"fixable": false` even though 1 of the 2 violations is fixable (run without `--statistics` to see that). Users seeing this would reasonably assume "none are fixable" and miss the opportunity to run `--fix`.

Adding `fixable_count` addresses this by explicitly showing how many issues are fixable.

**This leave us with a dilemma - what to do with `fixable` itself:**

1. **Keep both `fixable` and `fixable_count`**, preserve current behavior, add documentation
   - Pro: No breakage
   - Con: Keeps a misleading gotcha in the UX

2. **Keep both, but change `fixable` to mean "any fixable"**
   - Pro: More useful to users
   - Con: Still a breaking change, meaning still not 100% clear from name, and now redundant with `fixable_count`

3. **Remove `fixable`, keep only `fixable_count`**
   - Pro: Explicit, unambiguous, users can derive both "any" (`> 0`) and "all" (`== count`)
   - Con: Breaking change for user scripts

I believe option 3 is best in the long run - users facing an explicit break now (while Ruff is still <1.0) is better than silently getting misleading info. But you know more about Ruff's usage in the wild, so I defer to your judgment here.


---

_Comment by @tsvikas on 2025-11-18 18:24_

> I see that you wrote this PR with claude. We don't have a policy against using AI but I just want to ask you to review the PR yourself first before we spend time reviewing. I'll put it into draft, you can mark it as ready for review once you think the PR is in a good state.

Fair point. Thank you for verifying.
I did review the code before submitting. I made sure to understand any change, and compiled/ran it on examples to verify the behavior.

That said, I'm not a Rust developer, so I could easily miss Rust-specific idioms, performance considerations, or project conventions. This is my best effort given my experience level.

I'll mark it as ready for review now. Happy to iterate on any feedback.

---

_Marked ready for review by @tsvikas on 2025-11-18 18:27_

---

_Comment by @MichaReiser on 2025-11-19 07:57_

Thanks for the explanation.

I ran the command and, as a user, it isn't clear what `~` means. We'll need to update the summary too

```
﻿﻿169     F821    [ ] undefined-name
  8     F541    [*] f-string-missing-placeholders
  5     E401    [*] multiple-imports-on-one-line
  4     E402    [ ] module-import-not-at-top-of-file
  3     F401    [*] unused-import
  3     F841    [*] unused-variable
  2     D100    [ ] undocumented-public-module
  2     E722    [ ] bare-except
  2     F602    [ ] multi-value-repeated-key-variable
  2     F811    [~] redefined-while-unused
  1             [ ] invalid-syntax
  1     D103    [ ] undocumented-public-function
  1     F402    [ ] import-shadowed-by-loop-var
  1     F403    [ ] undefined-local-with-import-star
  1     F631    [ ] assert-tuple
  1     F704    [ ] yield-outside-function
Found 206 errors.
[*] 20 fixable with the `--fix` option.
```

I also think that fixing the CLI command shouldn't break the CLI output. I suggest keeping `fixable: bool` in addition to `fixable_count` and we can deprecate `fixable` in the future. It would be good to add documentation to the fields explaining how they differ. 


---

_Comment by @tsvikas on 2025-11-19 13:31_

will do. 
> I ran the command and, as a user, it isn't clear what ~ means. We'll need to update the summary too

any input about how to update the summary? some ideas here:
```
[*~] 20 fixable with the `--fix` option.
20 fixable with the `--fix` option (marked with [*] and [~]).
20 fixable with the `--fix` option ([*]=all fixable, [~]=any fixable).
```

---

_Comment by @ntBre on 2025-11-19 18:55_

Would it make any sense to report two lines for F811? As in:

```
  1     F811    [*] redefined-while-unused
  1     F811    [ ] redefined-while-unused
  ```

Just an idea to avoid having to explain another marker in the footer, but maybe it would be confusing if it wasn't 1 of each and the two entries for the same rule sorted farther apart.

---

_Comment by @tsvikas on 2025-11-19 22:33_

another alternative is adding a count
```
﻿﻿169     F821    [  ] undefined-name
  8     F541    [*8] f-string-missing-placeholders
  5     E401    [*5] multiple-imports-on-one-line
  4     E402    [  ] module-import-not-at-top-of-file
  3     F401    [*3] unused-import
  3     F841    [*3] unused-variable
  2     D100    [  ] undocumented-public-module
  2     E722    [  ] bare-except
  2     F602    [  ] multi-value-repeated-key-variable
  2     F811    [*1] redefined-while-unused
  1             [  ] invalid-syntax
  1     D103    [  ] undocumented-public-function
  1     F402    [  ] import-shadowed-by-loop-var
  1     F403    [  ] undefined-local-with-import-star
  1     F631    [  ] assert-tuple
  1     F704    [  ] yield-outside-function
Found 206 errors.
[*] 20 fixable with the `--fix` option.
```
(i'm still going with the original idea, but i'm open to inputs from maintainers about their preferred direction)

---

_Comment by @tsvikas on 2025-11-19 23:52_

Thanks for the feedback! I've addressed all the requested changes:

## Changes Made

### 1. Summary line now explains the symbols
The summary line for `--statistics` now includes a legend:
```
1 fixable with the `--fix` option ([*] = all fixable, [~] = some fixable).
```

This only appears with `--statistics`. Regular output still shows the original format (since in that case the `[*]` marks a specific error, not an aggregated group).

**Question**: Do we want to add the `fixable_count` of each error-code to the printed statistics?

### 2. Kept `fixable: bool` for backward compatibility
JSON output now includes both fields:
```json
{
  "code": "UP035",
  "name": "deprecated-import",
  "count": 2,
  "fixable": false,
  "fixable_count": 1
}
```

`fixable` preserves existing semantics (true only when all violations are fixable).

### 3. Documentation
Added field documentation to the `--statistics` CLI help text.

I looked for existing documentation to update but found that `--statistics`, `--output-format`, JSON output schemas, and their interactions are largely undocumented. Creating comprehensive documentation for these features would be out of scope for this bug fix. The CLI help addition is a net improvement over the status quo.

Note: `cargo dev generate-all` didn't update `docs/configuration.md`, so I updated it manually. Let me know if there's a specific command for that. Hopefully the GH actions will keep the file updated anyway..

## Tests
Added tests for both text and JSON output with partial fixability.

---

Re: @ntBre's suggestion about splitting into two lines - I think keeping one row per rule code is more consistent with the statistics view purpose (aggregated counts).

---

_Comment by @tsvikas on 2025-11-20 12:04_

Also, this should no more be label:breaking

---

_Comment by @MichaReiser on 2025-11-20 13:03_

> another alternative is adding a count

I sort of like this, except that I would omit the `*`.


```
﻿﻿169     F821    [ ] undefined-name
  8     F541    [8] f-string-missing-placeholders
  5     E401    [5] multiple-imports-on-one-line
  4     E402    [ ] module-import-not-at-top-of-file
  3     F401    [3] unused-import
  3     F841    [3] unused-variable
  2     D100    [ ] undocumented-public-module
  2     E722    [ ] bare-except
  2     F602    [ ] multi-value-repeated-key-variable
  2     F811    [1] redefined-while-unused
  1             [ ] invalid-syntax
  1     D103    [ ] undocumented-public-function
  1     F402    [ ] import-shadowed-by-loop-var
  1     F403    [ ] undefined-local-with-import-star
  1     F631    [ ] assert-tuple
  1     F704    [ ] yield-outside-function
Found 206 errors.
[20] fixable with the `--fix` option.
```

But I'm not a 100% convinced by it. It looks a bit odd. 



---

_Comment by @tsvikas on 2025-11-20 15:24_

we can also:
```
F821 ﻿﻿ 169     undefined-name
F541    8  8  f-string-missing-placeholders
E401    5  5  multiple-imports-on-one-line
E402    4     module-import-not-at-top-of-file
F401    3  3  unused-import
F841    3  3  unused-variable
D100    2     undocumented-public-module
E722    2     bare-except
Found 186 errors.
19 fixable with the `--fix` option.
```

or 
```
169 /    F821  undefined-name
  8 / 8  F541  f-string-missing-placeholders
  5 / 5  E401  multiple-imports-on-one-line
  4 /    E402  module-import-not-at-top-of-file
  3 / 3  F401  unused-import
  3 / 3  F841  unused-variable
  2 /    D100  undocumented-public-module
  2 /    E722  bare-except
Found 186 errors / 19 fixable with the `--fix` option.
```

---

_Comment by @tsvikas on 2025-11-20 15:29_

In my perspective, the count is useful and I also like the conciseness of [~].
In any case, what information / criteria / next step would help us make a decision here?

---

_Comment by @tsvikas on 2025-11-20 15:30_

And in case it was missed in the conversation - All other parts of the PR are complete and ready for review, including the current implementation with the [~] format.

---

_Comment by @MichaReiser on 2025-11-24 08:47_

Sorry, I had to think it over a bit. I would go with

* Use `[-]` as "partial fixable" indicator, similar to markdown
* Change the summary to `20 fixable with `--fix` ([*] all, [-] some).`

I also suggest changing `write_summary_text` to take an `enum Mode { Default, Statistics }` to make it clearer what the boolean argument means

---

_Comment by @tsvikas on 2025-11-24 22:23_

done.

example of the current output:
```
594     E501    [ ] line-too-long
 51     I001    [*] unsorted-imports
 17     E402    [ ] module-import-not-at-top-of-file
 16     UP015   [*] redundant-open-modes
 12     B007    [ ] unused-loop-control-variable
 11     B905    [ ] zip-without-explicit-strict
 10     UP045   [*] non-pep604-annotation-optional
  8     SIM115  [ ] open-file-with-context-handler
  6     E731    [ ] lambda-assignment
  6     F541    [*] f-string-missing-placeholders
  4     SIM102  [ ] collapsible-if
  4     SIM117  [-] multiple-with-statements
  4     UP024   [*] os-error-alias
  3     E722    [ ] bare-except
  3     SIM109  [ ] compare-with-tuple
  2     F401    [*] unused-import
  2     SIM108  [ ] if-else-block-instead-of-if-exp
  2     UP006   [*] non-pep585-annotation
  2     UP035   [-] deprecated-import
  1     B019    [ ] cached-instance-method
  1     E713    [*] not-in-test
  1     E741    [ ] ambiguous-variable-name
  1     F841    [*] unused-variable
  1     SIM105  [ ] suppressible-exception
  1     SIM300  [*] yoda-conditions
Found 763 errors.
97 fixable with the `--fix` option ([*] all, [-] some) (33 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

---

_Review requested from @MichaReiser by @amyreese on 2025-11-26 02:43_

---

_Comment by @MichaReiser on 2025-11-26 08:14_

I'm sorry, but I didn't realize that the summary has variants where it adds another message in parentheses and the double parenthesizing looks sort of bad.

What do you think of:

```
[*] 20 fixable with the `--fix` option.
```

and assume that users will be able to infer that `[-]` means partial fixable

or 

```
[*][-] 20 fixable with the `--fix` option.
```

---

_Comment by @tsvikas on 2025-11-26 09:29_

If you are asking for my input, I feel that a need to infer the meaning is less ideal.

What do you think about:
```
Found 763 errors.
97 fixable with the `--fix` option ([*] all, [-] some).
33 hidden fixes can be enabled with the `--unsafe-fixes` option.
```

It also shortens the line length, which is a plus.

(I don't think it's perfect, but it's the best I've found yet)

---

_Comment by @MichaReiser on 2025-11-26 09:33_

I like that

---

_Comment by @tsvikas on 2025-11-26 13:14_

Thanks for the feedback! I looked at the code more carefully and realized that I need another input before continuing.

The issue is that this change would create an inconsistency with other output modes.

i.e., compare 
```
Found 763 errors.
97 fixable with the `--fix` option ([*] all, [-] some).
33 hidden fixes can be enabled with the `--unsafe-fixes` option.
```
with those examples

#### Examples:
1. `ruff check --statistics` with no errors:
```
Found 657 errors.
No fixes available (33 hidden fixes can be enabled with the `--unsafe-fixes` option).
```

2. `ruff check` with no errors:
```
Found 1 error.
No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
```

3. `ruff check` with errors:
```
Found 763 errors.
[*] 97 fixable with the `--fix` option (33 hidden fixes can be enabled with the `--unsafe-fixes` option).
```
(note: when we don't use `--statistics`, the meaning of `[*]` is "this error is fixable" and not "all/any errors with this code are fixable", so there is no need for `[-]`) 

4. `ruff check --fix-only --statistics`:
```
...
  1     SIM105  suppressible-exception
Fixed 101 errors (33 additional fixes available with `--unsafe-fixes`).
```

### Options
Do you think we should:

1. Apply the same format (moving the hidden fixes message from parentheses to a separate line) everywhere.
   - Pros: Consistent UX across all modes + Cleaner code in `printer.rs`
   - Cons: Larger change scope + Increases the summary line from 1-2 lines to 1-3 lines

3. Limit this change to `--statistics` only (which cases exactly?  it seems that there are non-trivial states here).

5. Keep as is, find another way to represent the `[-]` symbol.

I'm leaning toward option 1, but I wanted to get your input before. WDYT?


---

_Comment by @tsvikas on 2025-11-26 15:29_

Also, in one case the output is ```1 fixable with the --fix option``` and in all the others  ```1 fixable with the `--fix` option```.
I'm happy to fix that also (add ticks). would you prefer a separate PR for that, or should i fix it here?

---

_Comment by @MichaReiser on 2025-11-27 07:51_

I'm sort of leaning towards leaving it as is (the legend) and assuming that users can infer the meaning of `[-]` and we can adjust the message based on feedback. It simplifies this PR and allows us to land this improvement.


>  I'm happy to fix that also (add ticks). would you prefer a separate PR for that, or should i fix it here?

I'm fine landing it in this PR


---

_Comment by @tsvikas on 2025-11-27 13:38_

Done :)
current output
```
594     E501    [ ] line-too-long
 51     I001    [*] unsorted-imports
 17     E402    [ ] module-import-not-at-top-of-file
 16     UP015   [*] redundant-open-modes
 12     B007    [ ] unused-loop-control-variable
 11     B905    [ ] zip-without-explicit-strict
 10     UP045   [*] non-pep604-annotation-optional
  8     SIM115  [ ] open-file-with-context-handler
  6     E731    [ ] lambda-assignment
  6     F541    [*] f-string-missing-placeholders
  4     SIM102  [ ] collapsible-if
  4     SIM117  [-] multiple-with-statements
  4     UP024   [*] os-error-alias
  3     E722    [ ] bare-except
  3     SIM109  [ ] compare-with-tuple
  2     F401    [*] unused-import
  2     SIM108  [ ] if-else-block-instead-of-if-exp
  2     UP006   [*] non-pep585-annotation
  2     UP035   [-] deprecated-import
  1     B019    [ ] cached-instance-method
  1     E713    [*] not-in-test
  1     E741    [ ] ambiguous-variable-name
  1     F841    [*] unused-variable
  1     SIM105  [ ] suppressible-exception
  1     SIM300  [*] yoda-conditions
Found 763 errors.
[*] 97 fixable with the `--fix` option (33 hidden fixes can be enabled with the `--unsafe-fixes` option).
```



---

_Comment by @tsvikas on 2025-11-27 13:54_

> > I'm happy to fix that also (add ticks). would you prefer a separate PR for that, or should i fix it here?
> 
> I'm fine landing it in this PR

it's contained in the last commit, so we can inspect it separately or move it to a different PR if needed

---

_@MichaReiser approved on 2025-11-27 16:49_

Thank you

---

_Label `breaking` removed by @MichaReiser on 2025-11-27 16:52_

---

_Comment by @astral-sh-bot[bot] on 2025-11-27 17:03_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Merged by @MichaReiser on 2025-11-27 17:03_

---

_Closed by @MichaReiser on 2025-11-27 17:03_

---

_Comment by @tsvikas on 2025-11-27 22:49_

Thank you too!

---

_Branch deleted on 2025-11-28 19:12_

---
