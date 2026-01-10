```yaml
number: 9984
title: "Colorize `format --diff` output"
type: issue
state: closed
author: MichaReiser
labels:
  - good first issue
  - cli
  - help wanted
assignees: []
created_at: 2024-02-14T13:01:02Z
updated_at: 2024-03-01T09:04:42Z
url: https://github.com/astral-sh/ruff/issues/9984
synced_at: 2026-01-10T11:09:52Z
```

# Colorize `format --diff` output

---

_Issue opened by @MichaReiser on 2024-02-14 13:01_

### Discussed in https://github.com/astral-sh/ruff/discussions/9983

We should colorize the diff output (added and removed lines). 

The relevant code is in https://github.com/astral-sh/ruff/blob/a7d1f7e1ec6d2d4443eaec4bee5faebd97f95c66/crates/ruff_linter/src/source_kind.rs#L103-L111

We probably would need to extract the `to_writer` function and change it so that we set the right color depending on if it's an addition or deletion:

```rust
 let mut header = self.header.as_ref();
        for hunk in self.iter_hunks() {
            if let Some((old_file, new_file)) = header.take() {
                writeln!(w, "--- {}", old_file)?;  // <-- add color here
                writeln!(w, "+++ {}", new_file)?;
            }
            write!(w, "{}", hunk)?;
        }
        Ok(())
```

<div type='discussions-op-text'>

<sup>Originally posted by **ssenior45** February 14, 2024</sup>
If I run `ruff check` on a file then the output is colored which leads me to believe that my terminal is configured to display colored output from the tool correctly.

However when I run `ruff format --diff` the output is not colored. Is this expected? If so is there anything that can be done about it?

Thanks in advance.</div>

---

_Label `good first issue` added by @MichaReiser on 2024-02-14 13:01_

---

_Label `cli` added by @MichaReiser on 2024-02-14 13:01_

---

_Label `help wanted` added by @MichaReiser on 2024-02-14 13:01_

---

_Comment by @charliermarsh on 2024-02-14 14:01_

Def a good first issue!

---

_Comment by @senadev42 on 2024-02-14 14:02_

Hey @MichaReiser, I'm interested on working on this

Edit: But I've also noticed you tagged OP in the original discussion

---

_Comment by @MichaReiser on 2024-02-14 14:13_

@senadev42 nice. I think the original OP doesn't mind if the feature gets implemented by someone else. I assign the issue to you. Let me know if you need any help. 

Note: We use https://docs.rs/colored/latest/colored/ for colonizing the CLI output. 

---

_Assigned to @senadev42 by @MichaReiser on 2024-02-14 14:13_

---

_Comment by @ssenior45 on 2024-02-14 23:08_

Just catching up with this now. Thanks for moving it to an issue. I would like to see this implemented and am more than happy for @senadev42 to work on it.

---

_Comment by @MichaReiser on 2024-02-23 21:19_

@senadev42 is there something I can help you with or would you prefer if someone else takes a look at it?

---

_Comment by @senadev42 on 2024-02-23 21:32_

@MichaReiser Oh I've just been unexpectedly busy lately but I'm picking away at it, just kind of slowly since I assumed it wasn't an urgent/critical task.

I can let go if it is though, since I'm using this as an opportunity to dip my toes into rust.



---

_Comment by @MichaReiser on 2024-02-23 21:35_

@senadev42 it's not urgent. I just wanted to make sure you've all you need :)

---

_Comment by @senadev42 on 2024-02-24 13:26_

Hey @MichaReiser 

So I've made a draft PR at #10110 and I'd like some help interpreting why and how some of the tests are failing

---

_Comment by @MichaReiser on 2024-03-01 09:04_

Closed by https://github.com/astral-sh/ruff/pull/10110

---

_Closed by @MichaReiser on 2024-03-01 09:04_

---
