```yaml
number: 15867
title: "[red-knot] Assert full diagnostic output in mdtests"
type: issue
state: closed
author: MichaReiser
labels:
  - testing
  - ty
  - diagnostics
assignees: []
created_at: 2025-02-01T08:55:10Z
updated_at: 2025-02-05T18:02:56Z
url: https://github.com/astral-sh/ruff/issues/15867
synced_at: 2026-01-10T11:09:57Z
```

# [red-knot] Assert full diagnostic output in mdtests

---

_Issue opened by @MichaReiser on 2025-02-01 08:55_

### Description

See my comment in https://github.com/astral-sh/ruff/pull/15856/files#r1938233453

We should extend the mdtest framework to support assert on and capture the full diagnostic output

---

_Label `diagnostics` added by @MichaReiser on 2025-02-01 08:55_

---

_Label `red-knot` added by @MichaReiser on 2025-02-01 08:55_

---

_Label `testing` added by @MichaReiser on 2025-02-01 08:55_

---

_Comment by @AlexWaygood on 2025-02-01 13:44_

Do we want to do this as part of mdtest? Wouldn't snapshot testing be a better fit for this?

Not all our tests need to be mdtests. I think the format is best suited for writing lots of quick and easy tests regarding type inference. Maybe we just need to get into the habit of adding snapshot tests as well as mdtests whenever we add new lints, so that we also record how the diagnostic will be rendered for the end user?

---

_Comment by @MichaReiser on 2025-02-01 20:19_

I'm not sure if we want an entirely different testing framework just to capture diagnostics. It feels an unnecessary barrier for writing full-diagnostic tests and I'd expect that people wont to it because of it.

 I do agree that the testing framework should use some form of snapshot mechanism. I don't want to write those by hand. One option is to have a special comment that instructs the mdtest framework to expect and snapshot an error, e.g by using `# error [snapshot] "message".  Whether the snapshots are inline (by updating the mdtest file itself) or in a separate file is another design question

---

_Comment by @MichaReiser on 2025-02-01 23:02_

We could, for example, create a snapshot (external file) that includes the source of every file, the diagnostic, and, in the future, a possible fix. I think that could be fairly ergonomic to review and maintain because I'd envision that linter tests would be smaller (a single test case for each condition you want to test), unlike how our linter tests are today where we have one big file with all tests

---

_Comment by @AlexWaygood on 2025-02-02 10:32_

Ahh, now I understand what you're proposing. Yes, this sounds like a nice idea!

---

_Comment by @BurntSushi on 2025-02-04 13:12_

I note that there appears to be an alternative design for testing full diagnostic output in the mdtest doc:

https://github.com/astral-sh/ruff/blob/15dd3b5ebdd2d1c25da3815afb0b8172aef9c812/crates/red_knot_test/README.md#L346-L381

I think the `output` blocks suggested in that doc appeal to me a bit more. But I confess that I think I don't fully understand @MichaReiser's idea here. Could you say a bit more about how you envision this working?

---

_Comment by @MichaReiser on 2025-02-04 13:24_

@BurntSushi yes, I find the "inline snapshot" using `output` blocks also more appealing but I suspect that it's a little more work because we can't use insta for it (at least, it's not clear to me how we'd do that). 

I haven't thought this through fully but a first solution could be to use regular insta snapshots (and later evolve to support inline snapshots, similar to how insta started without inline snapshots). The basic idea is that we construct a "markdown string" similar to what we do for [the formatter](https://github.com/astral-sh/ruff/blob/c847cad389f202edae868765e690cb7042e88264/crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__allow_empty_first_line.py.snap#L25-L24). The constructed markdown file could include:

* The source code
* The one diagnostic
* Possibly: Settings
* Future: Possible fix


I'd include the source code so that the snapshot file can be reviewed in isolation. The main downside of including the snapshot or settings is that any change to either results in snapshot changes. 

---

_Comment by @BurntSushi on 2025-02-04 13:38_

@MichaReiser Aye. Are you envisioning one snapshot file per `.md` file? Or one snapshot file per test within each `.md` file?

---

_Comment by @MichaReiser on 2025-02-04 13:40_

I'm leaning toward one snapshot file per fully asserted diagnostic or at least one snapshot per test in an md file. 

I'm not quiet sure what the right balance is. I do find Ruff's snapshots very noisy but that's probably mainly because we smash all test cases into a single file without any comments. I envision this to be different for mdtests where it's easy to isolate different test cases and prose comments. TLDR: One snapshot per test case is probably a good start (resulting in multiple snapshots for each mdtest file)

---

_Closed by @BurntSushi on 2025-02-05 18:02_

---
