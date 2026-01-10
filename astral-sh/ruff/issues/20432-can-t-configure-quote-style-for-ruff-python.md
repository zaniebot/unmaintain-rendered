---
number: 20432
title: "Can't configure quote style for `ruff_python_codegen::Generator`"
type: issue
state: closed
author: ShaharNaveh
labels:
  - question
assignees: []
created_at: 2025-09-16T07:53:13Z
updated_at: 2025-09-18T10:57:22Z
url: https://github.com/astral-sh/ruff/issues/20432
synced_at: 2026-01-10T01:23:01Z
---

# Can't configure quote style for `ruff_python_codegen::Generator`

---

_Issue opened by @ShaharNaveh on 2025-09-16 07:53_

There's support for creating a `Generator` from a `Stylist` instance:

https://github.com/astral-sh/ruff/blob/2a6dde4acb6cb6a8ef4426ca3d792bdb1c477f21/crates/ruff_python_codegen/src/stylist.rs#L11-L17

https://github.com/astral-sh/ruff/blob/2a6dde4acb6cb6a8ef4426ca3d792bdb1c477f21/crates/ruff_python_codegen/src/generator.rs#L76-L87


And while `indentation` and `line_ending` are being respected, `quote` is ignored:/

---

Motivation for issue: https://github.com/RustPython/RustPython/pull/6124#discussion_r2320979701

---

_Label `question` added by @MichaReiser on 2025-09-16 08:23_

---

_Comment by @MichaReiser on 2025-09-16 08:25_

This is intentional in that we try to preserve the quote styles whenever possible rather than enforcing a fixed quote style. This ensures that fixes don't introduce unnecessary changes.

I'm not sure what it would require to have a global preferred quote style that overrides the AST node quote style.

---

_Comment by @ShaharNaveh on 2025-09-16 08:37_

> This is intentional in that we try to preserve the quote styles whenever possible rather than enforcing a fixed quote style. This ensures that fixes don't introduce unnecessary changes.

I see, would it make sense to keep the signature of `Generator::new` the same, and add a method that can change the quote style. something like:
```rust
impl Generator {
 pub const fn new(indent: &'a Indentation, line_ending: LineEnding) -> Self {
        Self {
            // Style preferences.
            indent,
            line_ending,
            quote: Quote::default(), // Added this
            // Internal state.
            buffer: String::new(),
            indent_depth: 0,
            num_newlines: 0,
            initial: true,
        }
    }

  pub fn with_quote(mut self, quote: Quote) -> Self {
    self.quote = quote;
    self
  }

}
```

This makes it so it's backwards compatible, while giving the user the ability to configure the quote style if so desired.

> I'm not sure what it would require to have a global preferred quote style that overrides the AST node quote style.

Me neither, the purpose of this question is to see if this is something that the ruff team are willing to merge once there's a PR for it


---

_Comment by @MichaReiser on 2025-09-16 08:39_

> Me neither, the purpose of this question is to see if this is something that the ruff team are willing to merge once there's a PR for it

I'm not oposed to it, but it depends on the level of complexity it introduces and requires that someone finds the time to review 



---

_Comment by @ShaharNaveh on 2025-09-16 08:43_

> > Me neither, the purpose of this question is to see if this is something that the ruff team are willing to merge once there's a PR for it
> 
> I'm not oposed to it, but it depends on the level of complexity it introduces and requires that someone finds the time to review

Fair enough. ty for the swift response:)
Will poke the code a bit and see what's the effort

---

_Referenced in [astral-sh/ruff#20434](../../astral-sh/ruff/pulls/20434.md) on 2025-09-16 10:13_

---

_Closed by @MichaReiser on 2025-09-18 10:57_

---
