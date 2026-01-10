```yaml
number: 19688
title: "Set `Annotation::is_file_level` in affected lint rules"
type: issue
state: open
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
created_at: 2025-08-01T14:57:55Z
updated_at: 2025-08-04T18:34:09Z
url: https://github.com/astral-sh/ruff/issues/19688
synced_at: 2026-01-10T11:09:59Z
```

# Set `Annotation::is_file_level` in affected lint rules

---

_Issue opened by @ntBre on 2025-08-01 14:57_

The below is in reference to this check added in #19653:

https://github.com/astral-sh/ruff/blob/138188cd4fe0a39975896f8592efbb0dc73ce938/crates/ruff_linter/src/message/mod.rs#L80-L83

But this depends on the ability to cache `Diagnostic`s more fully than we currently do in [`CacheMessage`](https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/cache.rs#L454). The current caching scheme would lose the `is_file_level` flag on any `Annotation`s.

> Would it be much more work to instead change the rules that add file level diagnostics rather than doing this "global" override? 
> 
> (the rule would have to mutate the `Diagnostic` after reporting it with `report_diagnostic`).
> 
> The reason I'd prefer this is that it makes it more apparent where we rely on this behavior and it doesn't prevent other rules to use an empty range (e.g. what if we had a rule that enforces module level docstrings. That rule would probably ask you to insert the docstring at 0:0.

_Originally posted by @MichaReiser in https://github.com/astral-sh/ruff/pull/19653#discussion_r2247314964_
            

---

_Assigned to @ntBre by @ntBre on 2025-08-01 14:57_

---

_Label `internal` added by @ntBre on 2025-08-01 14:58_

---

_Label `diagnostics` added by @ntBre on 2025-08-01 14:58_

---

_Comment by @ntBre on 2025-08-04 18:34_

I dug into this a bit more today, and I think this is very closely tied to removing `is_file_level` entirely. These are my somewhat rambly notes that I was considering making into a separate issue, but recording them here seems fine for now.

<hr>

Ruff currently uses `TextRange::default` as a signal for file-level diagnostics that apply to an entire file rather than a specific range within the fiile:

https://github.com/astral-sh/ruff/blob/af8587eabf83fa38fad153bfe25f714524542c07/crates/ruff_linter/src/message/text.rs#L100-L102

We're carrying this behavior forward in https://github.com/astral-sh/ruff/pull/19653 to maintain backwards compatibility for now:

https://github.com/astral-sh/ruff/blob/138188cd4fe0a39975896f8592efbb0dc73ce938/crates/ruff_linter/src/message/mod.rs#L49-L52

In both cases, this suppresses the display of the associated code snippet.

Ruff currently uses this for three distinct cases:
- the file doesn't exist or is otherwise inaccessible (e.g. `io-error`)
- the file exists but the diagnostic applies to the entire file (e.g. [stdlib-module-shadowing (A005)](https://docs.astral.sh/ruff/rules/stdlib-module-shadowing/#stdlib-module-shadowing-a005))
- a normal diagnostic that happens to be at the start of a file (many lint rules, especially in tests)

In the new diagnostic model, we can differentiate these. For `io-error`s and other diagnostics without a file, we should omit an `Annotation` entirely. In this case, we'll need to include the filename in the message itself, as ty and rustc do:

```
$ ty check nonexistent.py
error[io]: `/tmp/tmp.gqRJ0jMRZ9/nonexistent.py`: No such file or directory (os error 2)
$ rustc nonexistent.py
error: couldn't read `nonexistent.py`: No such file or directory (os error 2)
```

In cases where the diagnostic truly applies to the whole file, we can omit the `Span::range`:

https://github.com/astral-sh/ruff/blob/dbd067c8698837044f505e7b12e1f733dedec46d/crates/ruff_db/src/diagnostic/mod.rs#L1108-L1112

This renders without a range in our `concise` output, which is the main reason that this is a breaking change:

```
example.py: error[test-diagnostic] main diagnostic message
```

We may also want to revisit the `full` rendering here because it still points to the beginning of the file, as Ruff does today:

```
error[test-diagnostic]: main diagnostic message
 --> example.py:1:1
  |                
1 | hello          
  | ^              
2 | world          
3 | 3              
  |                
```

Finally, for ranges that happen to point to the start of the file, we can continue using `Some(TextRange::default())` and render the snippet at the beginning of the file, just like the example above.

<hr>

After writing that, I think the first and second cases should probably be the same. As shown, we currently render case (2) with a snippet from the file, which doesn't really make sense either. The diagnostic for `A005`, to reuse that example, applies to the module/file name, so we should just omit an annotation entirely, just like an `io-error`.

I'm not quite sure what the use-case for an intentional `None` range on `Some(span)` would be.

In any case, I think we'll need to assess this for each lint rule, which is the main connection to this issue.

---
