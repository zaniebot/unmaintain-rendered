---
number: 2065
title: Lines in help output can be wider than the terminal width
type: issue
state: open
author: mkantor
labels:
  - C-bug
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2020-08-14T00:15:43Z
updated_at: 2021-12-10T22:19:19Z
url: https://github.com/clap-rs/clap/issues/2065
synced_at: 2026-01-07T13:12:19-06:00
---

# Lines in help output can be wider than the terminal width

---

_Issue opened by @mkantor on 2020-08-14 00:15_

### Code

```rust
use clap::App;

fn main() {
    let mut app = App::new("This string is 34 characters long.")
        .version("v12.34.56")
        .term_width(35);

    app.print_help();
}
```

### Steps to reproduce the issue

1. Run `cargo run`
2. Observe that the first line of the output (`This string is 34 characters long. v12.34.56`) is wider than the desired width (35), even though there are places where it could have been wrapped.

See [this diff](https://github.com/clap-rs/clap/compare/master...mkantor:demonstrate-incorrect-wrapping) for additional test cases.

### Version

* **Rust**: `rustc 1.45.2 (d3fb005a3 2020-07-31)`
* **Clap**: `master`

### Actual Behavior Summary

When getting help output that has been line-wrapped to fit the terminal width the output can be wider than the terminal, even if there are places where wrapping could have been applied.

### Expected Behavior Summary

Lines should never be wider than specified terminal width unless they contain no word breaks.

### Additional context

Wrapping is currently applied to individual bits and pieces of the overall help message without taking into account their context. For example, as demonstrated above the version number and binary name are displayed on the same line by default, but the binary name is always wrapped as if its last character is at the end of the line. Similarly, some template tags have their output wrapped, but without preceding/trailing characters being accounted for.

---

_Label `T: bug` added by @mkantor on 2020-08-14 00:15_

---

_Referenced in [clap-rs/clap#2066](../../clap-rs/clap/pulls/2066.md) on 2020-08-14 00:16_

---

_Comment by @mkantor on 2020-08-14 00:19_

One way to solve this would be to buffer the entire help message into a string, then apply wrapping to it all at once. This could have a performance impact (I haven't tried it).

A more sophisticated approach could be to buffer the help message in chunks, flushing it to output whenever wrapping occurs (or when an explicit newline is encountered). That could be fairly complex, though.

---

_Comment by @mkantor on 2020-08-14 00:25_

Expanding on the second paragraph in my previous comment, here's a sketch of what a "buffer in chunks" wrapping algorithm could look like. This hasn't been tested and there are likely edge cases I've missed:

- When writing chunks of the help message, append them to an internal buffer.
- If the buffer's width ever exceeds the terminal width, start wrapping:
  - Scan from the beginning of the buffer up to the terminal width, then backtrack to the previous word break, then flush everything up to that point plus a newline.
  - Repeat until the buffer's width is less than the terminal width.
  - This process would also need to handle lines that are wider than the terminal but which can't be broken up (e.g. the whole line is one long word).
  - It'd also need to handle strings with literal newlines, I guess by stopping each scan early and flushing immediately if a newline is encountered.
- At the end of the help message, flush whatever is left in the buffer.

The `HelpWriter::Normal` vs `HelpWriter::Buffer` distinction complicates this. Maybe `Normal` wouldn't get wrapping, just like it doesn't get colors currently? ¯\\\_(ツ)\_/¯

Anyway, it's probably worth measuring the performance impact of a "buffer it all" strategy first as it should be much simpler. Or maybe there's a better approach than either of these which I've missed.

---

_Referenced in [clap-rs/clap#2067](../../clap-rs/clap/pulls/2067.md) on 2020-08-14 01:00_

---

_Comment by @CreepySkeleton on 2020-08-14 01:13_

If you don't mind, a couple of questions:

* Why on earth would you create an app with `"horrifyingly and obscenely long binary name"` in the first place?
* How did you end up with a display width this tiny? Some sort of mobile device?

That was out of curiosity. For the record, I'd like to see this fixed, not ignored, even if the decided fix will be "don't support width less than N and assert that". 

> This hasn't been tested and there are likely edge cases I've missed:

Aha. Word wrap.

> Maybe Normal wouldn't get wrapping, just like it doesn't get colors currently

__No__. I don't see any reason why lack of color should imply lack of wrapping.

> Anyway, it's probably worth measuring the performance impact of a "buffer it all" strategy first as it should be much simpler. Or maybe there's a better approach than either of these which I've missed.

And this is not-too-far description of my "Refactor" PR. I've done only some preliminary measurements, but they didn't show any significant difference. 

---

_Comment by @mkantor on 2020-08-14 01:23_

> Why on earth would you create an app with `"horrifyingly and obscenely long binary name" `in the first place?

I realized this issue existed while hacking on #2067. At first I thought it was only a problem with custom templates, but then I noticed the default help can also exhibit this behavior (albeit only in pathological cases like the super long binary name). Custom templates can put an arbitrary amount of stuff on a single line which makes this issue more pronounced (it can affect all tag(s), not just binary name & version).

> How did you end up with a display width this tiny? Some sort of mobile device?

The tiny width was just to make the example short. I don't set an explicit width in any of my real uses of clap.

---

_Comment by @mkantor on 2020-08-14 01:25_

>  > Maybe Normal wouldn't get wrapping, just like it doesn't get colors currently
>
> **No.** I don't see any reason why lack of color should imply lack of wrapping.

I agree. I think the variant names are throwing me off; since it's `Buffered` vs `Normal` it sounds like the latter shouldn't do any buffering (even though it already does).

---

_Added to milestone `3.1` by @pksunkara on 2020-08-23 04:50_

---

_Label `C: help message` added by @pksunkara on 2020-08-23 04:50_

---

_Comment by @pksunkara on 2020-10-26 08:11_

@mkantor I think this has been fixed with the recent changes to master. Do you think you can look into the test cases?

---

_Referenced in [mkantor/clap#4](../../mkantor/clap/pulls/4.md) on 2020-10-27 00:29_

---

_Comment by @mkantor on 2020-10-27 18:48_

@pksunkara I rebased [the tests](https://github.com/clap-rs/clap/compare/master...mkantor:demonstrate-incorrect-wrapping) and [they still fail](https://github.com/mkantor/clap/runs/1312044427?check_suite_focus=true), so it appears this issue hasn't been fixed.

---

_Comment by @pksunkara on 2020-10-28 06:21_

`wrapping_items_on_same_line` test case should be easy to fix now if you want to look into it.

---

_Referenced in [foriequal0/git-trim#180](../../foriequal0/git-trim/issues/180.md) on 2021-06-03 01:20_

---

_Referenced in [epage/clapng#162](../../epage/clapng/issues/162.md) on 2021-12-06 20:18_

---

_Label `S-waiting-on-design` added by @epage on 2021-12-10 22:17_

---

_Removed from milestone `3.1` by @epage on 2021-12-10 22:17_

---

_Comment by @epage on 2021-12-10 22:19_

So it looks like we are wrapping individual fields in places (e.g. bin name, version, etc) rather than wrapping the result.

Part of this comes from the fact that we are processing a template.  Resolving this would take some work in balancing how to wrap the aggregation of fields without interfering with areas that might not want wrapping.

---
