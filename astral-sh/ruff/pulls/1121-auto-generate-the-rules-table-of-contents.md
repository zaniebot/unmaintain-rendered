```yaml
number: 1121
title: Auto-generate the rules table of contents
type: pull_request
state: merged
author: phillipuniverse
labels: []
assignees: []
merged: true
base: main
head: generate-rules-links
created_at: 2022-12-07T14:29:53Z
updated_at: 2022-12-07T15:04:24Z
url: https://github.com/astral-sh/ruff/pull/1121
synced_at: 2026-01-12T15:55:05Z
```

# Auto-generate the rules table of contents

---

_@phillipuniverse_

Follow-up to https://github.com/charliermarsh/ruff/pull/1109#discussion_r1041740034

---

_@phillipuniverse reviewed on 2022-12-07 14:31_

---

_Review comment by @phillipuniverse on `ruff_dev/src/generate_rules_table.rs`:82 on 2022-12-07 14:31_

I wasn't sure what the "right" way to handle multiple results here. I wanted to do something like:

```rust
return Result(toc_ok, table_ok)
```

or pythonic-like:

```python
return toc_ok and table_ok
```

---

_Review comment by @phillipuniverse on `ruff_dev/src/generate_rules_table.rs`:110 on 2022-12-07 14:31_

I removed the 2nd newline because rendering it inside the list caused an extra line to show up between list elements and it looked weird.

---

_@phillipuniverse reviewed on 2022-12-07 14:31_

---

_Review comment by @charliermarsh on `ruff_dev/src/generate_rules_table.rs`:82 on 2022-12-07 14:32_

I think here, you probably want:

```rust
replace_readme_section(toc_out, TOC_BEGIN_PRAGMA, TOC_END_PRAGMA)?;
replace_readme_section(table_out, TABLE_BEGIN_PRAGMA, TABLE_END_PRAGMA)?;
```

That way, if either call returns an `Err`, this caller function will stop immediately and return that error.

(As-written, the code will basically ignore / drop the errors.)

---

_@charliermarsh reviewed on 2022-12-07 14:32_

---

_@phillipuniverse reviewed on 2022-12-07 14:32_

---

_Review comment by @phillipuniverse on `ruff_dev/src/generate_rules_table.rs`:79 on 2022-12-07 14:32_

Whoops - fixing this

---

_@phillipuniverse reviewed on 2022-12-07 14:33_

---

_Review comment by @phillipuniverse on `ruff_dev/src/generate_rules_table.rs`:82 on 2022-12-07 14:33_

Got it, thanks!

---

_@charliermarsh reviewed on 2022-12-07 14:34_

---

_Review comment by @charliermarsh on `ruff_dev/src/generate_rules_table.rs`:82 on 2022-12-07 14:34_

See my other comment!

If you're calling a function that returns `Result<T>` from a function that returns `Result<T>` (as is the case here), you can always do `let ok_value = foo()?`, which will leave `ok_value` as type `T` but return immediately if `foo` returned an error.

---

_Review requested from @charliermarsh by @phillipuniverse on 2022-12-07 14:37_

---

_Comment by @charliermarsh on 2022-12-07 14:39_

Seeing a few minor lint errors when running `cargo +nightly clippy --workspace --all-targets --all-features -- -W clippy::pedantic` but otherwise good to go:

```
warning: variables can be used directly in the `format!` string
  --> ruff_dev/src/generate_rules_table.rs:33:29
   |
33 |         table_out.push_str(&format!("### {} ({})", check_category.title(), codes_csv));
   |                             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#uninlined_format_args
   = note: `-W clippy::uninlined-format-args` implied by `-W clippy::pedantic`
help: change this to
   |
33 -         table_out.push_str(&format!("### {} ({})", check_category.title(), codes_csv));
33 +         table_out.push_str(&format!("### {} ({codes_csv})", check_category.title()));
   |

warning: single-character string constant used as pattern
  --> ruff_dev/src/generate_rules_table.rs:41:59
   |
41 |             check_category.title().to_lowercase().replace(" ", "-"),
   |                                                           ^^^ help: try using a `char` instead: `' '`
   |
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#single_char_pattern
   = note: `#[warn(clippy::single_char_pattern)]` on by default

warning: single-character string constant used as pattern
  --> ruff_dev/src/generate_rules_table.rs:42:64
   |
42 |             codes_csv.to_lowercase().replace(",", "-").replace(" ", "")
   |                                                                ^^^ help: try using a `char` instead: `' '`
   |
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#single_char_pattern

warning: single-character string constant used as pattern
  --> ruff_dev/src/generate_rules_table.rs:42:46
   |
42 |             codes_csv.to_lowercase().replace(",", "-").replace(" ", "")
   |                                              ^^^ help: try using a `char` instead: `','`
   |
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#single_char_pattern

warning: this argument is passed by value, but not consumed in the function body
  --> ruff_dev/src/generate_rules_table.rs:88:36
   |
88 | fn replace_readme_section(content: String, begin_pragma: &str, end_pragma: &str) -> Result<()> {
   |                                    ^^^^^^ help: consider changing the type to: `&str`
   |
   = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#needless_pass_by_value
   = note: `-W clippy::needless-pass-by-value` implied by `-W clippy::pedantic`

warning: using `write!()` with a format string that ends in a single newline
   --> ruff_dev/src/generate_rules_table.rs:110:5
    |
110 |     write!(f, "{prefix}\n")?;
    |     ^^^^^^^^^^^^^^^^^^^^^^^
    |
    = help: for further information visit https://rust-lang.github.io/rust-clippy/master/index.html#write_with_newline
    = note: `#[warn(clippy::write_with_newline)]` on by default
help: use `writeln!` instead
    |
110 -     write!(f, "{prefix}\n")?;
110 +     writeln!(f, "{prefix}")?;
    |
```


---

_Comment by @phillipuniverse on 2022-12-07 14:41_

@charliermarsh 1 more thing, I validated the final rendering at https://github.com/phillipuniverse/ruff/tree/generate-rules-links#table-of-contents and the HTML comments screw up the sub-list.

<img width="695" alt="image" src="https://user-images.githubusercontent.com/684275/206208081-b7e487f3-ba4a-4447-a10d-5dbc012a78c2.png">

We could change the start/end pragma to:

```
const TOC_BEGIN_PRAGMA: &str = "1. [Supported Rules](#supported-rules)";
const TOC_END_PRAGMA: &str = "1. [Editor Integrations](#editor-integrations)";
```

And that will work great, but has the downside that there isn't a clear reference that it's auto-generated. WDYT?

---

_Comment by @charliermarsh on 2022-12-07 14:45_

Ohh interesting, because it doesn't like the HTML comment?

What about this?

![Screen Shot 2022-12-07 at 9 44 54 AM](https://user-images.githubusercontent.com/1309177/206209440-f6c492ec-4cbc-4704-9043-8adf1e675abb.png)


---

_Comment by @phillipuniverse on 2022-12-07 14:52_

@charliermarsh great success!

<img width="686" alt="image" src="https://user-images.githubusercontent.com/684275/206211158-37f6b3ee-5c62-4104-969a-ff5d902a6a07.png">


---

_Merged by @charliermarsh on 2022-12-07 15:03_

---

_Closed by @charliermarsh on 2022-12-07 15:03_

---

_Comment by @charliermarsh on 2022-12-07 15:03_

Awesome, thank you!

---

_Branch deleted on 2022-12-07 15:04_

---
