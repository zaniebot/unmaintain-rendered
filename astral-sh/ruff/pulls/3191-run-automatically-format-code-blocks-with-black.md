```yaml
number: 3191
title: Run automatically format code blocks with Black
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: mdformat
created_at: 2023-02-23T20:28:50Z
updated_at: 2023-02-27T15:19:19Z
url: https://github.com/astral-sh/ruff/pull/3191
synced_at: 2026-01-12T04:39:44Z
```

# Run automatically format code blocks with Black

---

_Pull request opened by @JonathanPlasse on 2023-02-23 20:28_

[mdformat](https://github.com/executablebooks/mdformat) is Python Markdown formatter.
It catches some errors that `markdownlint` does not detect.

It will:

- Use 1. everywhere
- Switch * to - bullet (it does not support `*` option, I could maintain a fork to support it if it is not acceptable)
- Remove \n inside code expression (e.g. `code expression`)
- Move footers at the end of files
- Escape characters like `except*` with `except\*` as `*` would be considered like the start of an italic section.

But most importantly is that it will run Black on code blocks.
Currently, a lot of code blocks are malformatted (e.g. bad indentation)

If a part of the code blocks should not be formatted the comments `# fmt: off` and `# fmt: on` can be used.
They will be removed during the documentation generation.

> **Warning**

Ruff docs generation does not support code inside links. (e.g. ``[`typing.Any`](…)``)
```rust
    /// ## References
    ///
    /// * [PEP 484](https://www.python.org/dev/peps/pep-0484/#the-any-type)
    /// * [`typing.Any`](https://docs.python.org/3/library/typing.html#typing.Any)
    /// * [Mypy: The Any type](https://mypy.readthedocs.io/en/stable/kinds_of_types.html#the-any-type)
```
It produces this code
```markdown
## References

* [PEP 484](https://www.python.org/dev/peps/pep-0484/#the-any-type)
* [`typing.Any`][typing.Any](https://docs.python.org/3/library/typing.html#typing.Any)
* [Mypy: The Any type](https://mypy.readthedocs.io/en/stable/kinds_of_types.html#the-any-type)
```

---

_Comment by @charliermarsh on 2023-02-23 21:30_

This looks reasonable to me. I'm fine with using `-` instead of `*`.

> But most importantly is that it will run Black on code blocks. Currently, a lot of code blocks are malformatted (e.g. bad indentation).

Where are these? Are they in the auto-generated sections that thus don't appear in the diff?

> Ruff docs generation does not support code inside links.

This is intentional. We have to write them out like that, because MkDocs does not support code inside references. Are you saying that `mdformat` effects those? Or why is the warning called out?


---

_Comment by @JonathanPlasse on 2023-02-23 21:53_

>> But most importantly is that it will run Black on code blocks. Currently, a lot of code blocks are malformatted (e.g. bad indentation).
>
> Where are these? Are they in the auto-generated sections that thus don't appear in the diff?

I did not yet push the code block formatting.
This is why this pull request is currently a draft.

---

_Comment by @JonathanPlasse on 2023-02-23 21:56_

>> Ruff docs generation does not support code inside links.
>
> This is intentional. We have to write them out like that, because MkDocs does not support code inside references. Are you saying that mdformat effects those? Or why is the warning called out?

For the `mkdocs` links, `mdformat` affects those. It should not be blocking, a regex should fix it.


---

_Comment by @JonathanPlasse on 2023-02-24 00:18_

`mdformat` pass on the entire generated docs (excluding the links, but it does not affect this pull request, it would if `mdformat` was part of the docs generation process).
I removed the `# fmt: off` as they were added to run `mdformat-black`.
A future pull request could add `mdformat` inside the docs generation process.

---

_Marked ready for review by @JonathanPlasse on 2023-02-24 00:20_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_annotations/rules.rs`:20 on 2023-02-26 03:50_

Is there any way not to enforce these empty lines? I feel like they're not super helpful especially when we show users the Markdown directly. Maybe I'm wrong though?

---

_@charliermarsh reviewed on 2023-02-26 03:50_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_annotations/rules.rs`:20 on 2023-02-26 15:10_

There is no option for this. `markdownlint,` `mdformat,` and `prettier`all add the empty lines.
An option would be to only format the code blocks content with black.
But thinking about it now it would make more sense to run ruff auto fix and formatter on it.
We would need a way to specify a ruff configuration for each code blocks.
It would signal to the developer when generating the docs that the code blocks were updated and thus incorrect.
For example, when we have the before and after examples.

---

_@JonathanPlasse reviewed on 2023-02-26 15:10_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_annotations/rules.rs`:20 on 2023-02-26 16:47_

How exactly is this being run today? Like does `mdformat` get run over these Rust files, and it picks up the Markdown?

---

_@charliermarsh reviewed on 2023-02-26 16:47_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_annotations/rules.rs`:20 on 2023-02-26 18:06_

No, I generated the files then ran mdformat on them, then edited the rust files until the generation produced valid markdown.

---

_@JonathanPlasse reviewed on 2023-02-26 18:06_

---

_@charliermarsh reviewed on 2023-02-26 19:15_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_annotations/rules.rs`:20 on 2023-02-26 19:15_

I see, so would these not be enforced on an ongoing basis?

I'm partial to not applying the empty lines. [Clippy](https://github.com/rust-lang/rust-clippy/blob/3d193fa17a7df3256d58b841a2df92af1cbdbea4/declare_clippy_lint/src/lib.rs#L100) doesn't do that, and I like to follow Clippy as a best practice.

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_annotations/rules.rs`:20 on 2023-02-26 19:38_

No, they would not be enforced.

---

_@JonathanPlasse reviewed on 2023-02-26 19:38_

---

_@charliermarsh reviewed on 2023-02-26 23:42_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_annotations/rules.rs`:20 on 2023-02-26 23:42_

Can I back out the changes to add newlines everywhere, then merge this?

---

_@JonathanPlasse reviewed on 2023-02-27 08:01_

---

_Review comment by @JonathanPlasse on `crates/ruff/src/rules/flake8_annotations/rules.rs`:20 on 2023-02-27 08:01_

I can do it if you want.

---

_Comment by @JonathanPlasse on 2023-02-27 08:58_

Rebased, revert the commit that added empty lines, and run Black on new rules.

---

_Merged by @charliermarsh on 2023-02-27 15:14_

---

_Closed by @charliermarsh on 2023-02-27 15:14_

---

_Branch deleted on 2023-02-27 15:19_

---
