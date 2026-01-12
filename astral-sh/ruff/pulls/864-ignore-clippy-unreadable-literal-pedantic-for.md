```yaml
number: 864
title: "Ignore clippy::unreadable-literal (pedantic) for CONFUSABLES"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: clippy-unreadable-literal
created_at: 2022-11-21T19:39:58Z
updated_at: 2022-11-21T23:37:11Z
url: https://github.com/astral-sh/ruff/pull/864
synced_at: 2026-01-12T05:48:46Z
```

# Ignore clippy::unreadable-literal (pedantic) for CONFUSABLES

---

_Pull request opened by @andersk on 2022-11-21 19:39_

“long literal lacking separators”

https://rust-lang.github.io/rust-clippy/master/index.html#unreadable_literal

This occurs 838 times and isn’t worth fixing here.

---

_Merged by @charliermarsh on 2022-11-21 21:00_

---

_Closed by @charliermarsh on 2022-11-21 21:00_

---

_Comment by @charliermarsh on 2022-11-21 21:00_

Should we enforce `cargo clippy -- -W clippy::pedantic` on CI now?

---

_Branch deleted on 2022-11-21 21:07_

---

_Comment by @andersk on 2022-11-21 21:12_

We still have lints from `clippy::pedantic` to consider first:

- 215 [default_trait_access](https://rust-lang.github.io/rust-clippy/master/index.html#default_trait_access)
- 121 [match_same_arms](https://rust-lang.github.io/rust-clippy/master/index.html#match_same_arms)
- 83 [semicolon_if_nothing_returned](https://rust-lang.github.io/rust-clippy/master/index.html#semicolon_if_nothing_returned)
- 73 [must_use_candidate](https://rust-lang.github.io/rust-clippy/master/index.html#must_use_candidate)
- 29 [missing_errors_doc](https://rust-lang.github.io/rust-clippy/master/index.html#missing_errors_doc)
- 29 [too_many_lines](https://rust-lang.github.io/rust-clippy/master/index.html#too_many_lines)
- 29 [uninlined_format_args](https://rust-lang.github.io/rust-clippy/master/index.html#uninlined_format_args)
- 19 [redundant_closure_for_method_calls](https://rust-lang.github.io/rust-clippy/master/index.html#redundant_closure_for_method_calls)
- 17 [missing_panics_doc](https://rust-lang.github.io/rust-clippy/master/index.html#missing_panics_doc)
- 16 [unnecessary_wraps](https://rust-lang.github.io/rust-clippy/master/index.html#unnecessary_wraps)
- 13 [module_name_repetitions](https://rust-lang.github.io/rust-clippy/master/index.html#module_name_repetitions)
- 12 [map_unwrap_or](https://rust-lang.github.io/rust-clippy/master/index.html#map_unwrap_or)
- 11 [manual_string_new](https://rust-lang.github.io/rust-clippy/master/index.html#manual_string_new)
- 10 [doc_markdown](https://rust-lang.github.io/rust-clippy/master/index.html#doc_markdown)
- 9 [explicit_iter_loop](https://rust-lang.github.io/rust-clippy/master/index.html#explicit_iter_loop)
- 9 [implicit_hasher](https://rust-lang.github.io/rust-clippy/master/index.html#implicit_hasher)
- 7 [if_not_else](https://rust-lang.github.io/rust-clippy/master/index.html#if_not_else)
- 6 [similar_names](https://rust-lang.github.io/rust-clippy/master/index.html#similar_names)
- 5 [inline_always](https://rust-lang.github.io/rust-clippy/master/index.html#inline_always)
- 5 [needless_pass_by_value](https://rust-lang.github.io/rust-clippy/master/index.html#needless_pass_by_value)
- 4 [explicit_deref_methods](https://rust-lang.github.io/rust-clippy/master/index.html#explicit_deref_methods)
- 3 [match_bool](https://rust-lang.github.io/rust-clippy/master/index.html#match_bool)
- 3 [struct_excessive_bools](https://rust-lang.github.io/rust-clippy/master/index.html#struct_excessive_bools)
- 2 [cast_lossless](https://rust-lang.github.io/rust-clippy/master/index.html#cast_lossless)
- 2 [from_iter_instead_of_collect](https://rust-lang.github.io/rust-clippy/master/index.html#from_iter_instead_of_collect)
- 2 [items_after_statements](https://rust-lang.github.io/rust-clippy/master/index.html#items_after_statements)
- 2 [single_match_else](https://rust-lang.github.io/rust-clippy/master/index.html#single_match_else)
- 2 [unreadable_literal](https://rust-lang.github.io/rust-clippy/master/index.html#unreadable_literal)
- 1 [cast_possible_truncation](https://rust-lang.github.io/rust-clippy/master/index.html#cast_possible_truncation)
- 1 [cloned_instead_of_copied](https://rust-lang.github.io/rust-clippy/master/index.html#cloned_instead_of_copied)
- 1 [let_underscore_drop](https://rust-lang.github.io/rust-clippy/master/index.html#let_underscore_drop)
- 1 [mut_mut](https://rust-lang.github.io/rust-clippy/master/index.html#mut_mut)
- 1 [redundant_else](https://rust-lang.github.io/rust-clippy/master/index.html#redundant_else)


---

_Comment by @charliermarsh on 2022-11-21 21:14_

Fun :)

---

_Comment by @charliermarsh on 2022-11-21 22:37_

Is it possible to selectively enable the ones you've fixed, or no?

---

_Comment by @andersk on 2022-11-21 23:37_

Yes, we can add options like `-W clippy::inefficient_to_string` for a denylist or `-W clippy::pedantic -A clippy::default_trait_arms` for an allowlist ([documentation](https://doc.rust-lang.org/nightly/clippy/usage.html#command-line)).

---
