```yaml
number: 8854
title: "config: add new `docstring-code-format` knob"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/fmt/user-config
created_at: 2023-11-27T15:36:24Z
updated_at: 2023-12-18T13:07:51Z
url: https://github.com/astral-sh/ruff/pull/8854
synced_at: 2026-01-10T23:31:11Z
```

# config: add new `docstring-code-format` knob

---

_Pull request opened by @BurntSushi on 2023-11-27 15:36_

This PR does the plumbing to make a new formatting option, `docstring-code-format`, available in the configuration for end users. It is disabled by default (opt-in). It is opt-in at least initially to reflect a conservative posture. The intent is to make it opt-out at some point in the future.

This was split out from #8811 in order to make #8811 easier to merge. Namely, once this is merged, docstring code snippet formatting will become available to end users. (See comments below for how we arrived at the name.)

Closes #7146

## Test Plan

Other than the standard test suite, I ran the formatter over the CPython and polars projects to ensure both that the result looked sensible and that tests still passed. At time of writing, one issue that currently appears is that reformatting code snippets trips the long line lint: https://github.com/BurntSushi/polars/actions/runs/7006619426/job/19058868021

---

_Comment by @github-actions[bot] on 2023-11-27 15:48_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @BurntSushi on 2023-11-27 16:07_

There's also a question of whether this ought to be a boolean option at all. @konstin raised this here: https://github.com/astral-sh/ruff/pull/8811#discussion_r1403255043

That is, we might want the formatter to specifically raise an error if it comes across a snippet that is invalid Python. I somewhat think this would be better as a lint, but it would likely be much easier to implement in the formatter. If it were a lint, then we'd need to refactor the code snippet logic out of the formatter into something more generalized.

---

_@MichaReiser approved on 2023-11-27 23:48_

The change itself looks good to me but I would prefer changing the option name (also on `PyFormatOptions`). 

Rustfmt uses [`format_code_in_doc_comments`](https://rust-lang.github.io/rustfmt/?version=v1.6.0&search=#format_code_in_doc_comments) which we could change to `format_code_in_docstrings`. 

I prefer the name because

* It's unclear to me what `snippets` means without prefixing it with `code-` which I find to verbose
* `format-embedded` might be ambiguous if we start formatting embedded language, let's say SQL or CSS in your Python code (the other way round)
* I'm okay with `format-docstring-code`, although I find rustfmt's naming clearer.

I'm okay with using a boolean. I don't think we should bail formatting just because of an incorrect example. Ideally we would emit a warning, but that's tracked as a separate issue. 

The other question: Should this be gated behind preview mode (sorry if this is part of your other PR)? If so, should we emit a warning if it is enabled but the formatter is run without preview mode?

---

_Comment by @zanieb on 2023-11-28 00:25_

I like `format-code-in-docstrings`. I prefer clarity in settings, a little verbosity goes a long way. As I suggested before, I'm also into `format-docstring-code`. Another option is `format-docstring-code-snippets`. I think we can consider non-boolean options in the future, but we should start with the simple case.

> Should this be gated behind preview mode (sorry if this is part of your other PR)? If so, should we emit a warning if it is enabled but the formatter is run without preview mode?

The idea is opt-in without preview opt-out (i.e. on by default) with preview. Having a "double" opt-in seems overkill.

---

_@konstin approved on 2023-11-28 08:49_

`format-code-in-docstrings` sounds good

---

_Comment by @BurntSushi on 2023-11-28 15:22_

We have a winner. `format-code-in-docstrings` it is!

---

_Comment by @charliermarsh on 2023-11-28 22:07_

Works for me :)

---

_Renamed from "config: add new 'docstring-code' knob" to "config: add new `format-code-in-docstrings` knob" by @BurntSushi on 2023-12-05 17:41_

---

_Renamed from "config: add new `format-code-in-docstrings` knob" to "config: add new `docstring-code-format` knob" by @BurntSushi on 2023-12-11 14:56_

---

_Comment by @BurntSushi on 2023-12-11 14:58_

I think this was talked about internally and not here on the PR, but we ended up settling on `docstring-code-format` for the name instead. I think we all originally liked `format-code-in-docstrings`, but once we realized we were going to add another option for controlling the line width in docstring code snippets, the naming became a little trickier. With `docstring-code-format`, there's a natural name for line width: `docstring-code-line-width`. That is, the `docstring-code-` prefix acts as a namespace of sorts with the noun first and the verb second. But `format-code-in-docstrings` doesn't really have that property, with the verb first and the noun coming afterwards.

---

_Comment by @MichaReiser on 2023-12-12 02:33_

Note: `docstring-code-line-width` should be named `docstring-code-line-length` to align with the global `line-length` setting. We only use `line-width` internally (and we may want to perform an internal rename to use consistent naming internally and externally)

---

_@zanieb approved on 2023-12-12 04:54_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-12 17:01_

---

_Review requested from @MichaReiser by @BurntSushi on 2023-12-12 17:01_

---

_Review requested from @konstin by @BurntSushi on 2023-12-12 17:01_

---

_Review requested from @zanieb by @BurntSushi on 2023-12-12 17:01_

---

_Comment by @BurntSushi on 2023-12-12 17:03_

All righty, I think this PR is now in good spot where another review would be good. The probable changes since the last time you looked:

* The `docstring-code-line-length` option has been added. **It defaults to `dynamic`**, but users can also set a fixed integer too.
* I've added an integration-level test to sanity check that both docstring code options are actually working.
* I've polished up the docs at the setting level and made some small edits to the existing prose formatter docs. I stopped short of adding a new section because I kind of felt like the docs on the setting itself might cover things, but happy to write more if folks think it would be useful.

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:2902 on 2023-12-12 17:13_

Nit: I'd remove "in the format of doctests", and just say, "Python code within docstrings", since this also applies to reStructuredText and Markdown.

---

_@charliermarsh reviewed on 2023-12-12 17:13_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:2934 on 2023-12-12 17:13_

I randomly tend to add ellipses to the start of a sentence like this, but it's totally personal preference :)

---

_@charliermarsh reviewed on 2023-12-12 17:13_

---

_@charliermarsh reviewed on 2023-12-12 17:13_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:2968 on 2023-12-12 17:13_

Nit: Oxford comma for consistency :joy: (after "literal blocks")

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:3055 on 2023-12-12 17:14_

Excellent.

---

_@charliermarsh reviewed on 2023-12-12 17:14_

---

_@charliermarsh reviewed on 2023-12-12 17:15_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:2986 on 2023-12-12 17:15_

I would say "This value has the effect" rather than "This setting has the effect", since the setting is `docstring-code-line-length`. Or, like:

```
The default value for this setting is `"dynamic"`, which has the effect of ensuring...
```

---

_@charliermarsh reviewed on 2023-12-12 17:15_

---

_Review comment by @charliermarsh on `docs/configuration.md`:87 on 2023-12-12 17:15_

I think these need to be removed, since the configuration here is meant to show the _default_ configuration.

---

_@BurntSushi reviewed on 2023-12-12 17:16_

---

_Review comment by @BurntSushi on `crates/ruff_workspace/src/options.rs`:2968 on 2023-12-12 17:16_

Oof. That's a knife to the heart. In the name of consistency we do evil things.

---

_@charliermarsh reviewed on 2023-12-12 17:17_

This looks good, but can I request that we add a section like:


```markdown
## Docstring formatting

The Ruff Formatter is capable of formatting Python code within docstrings...
```

...to `formatter.md`, just after its `## Configuration` block?

---

_@BurntSushi reviewed on 2023-12-12 17:20_

---

_Review comment by @BurntSushi on `docs/configuration.md`:87 on 2023-12-12 17:20_

Is it worth keeping them here, but changing `docstring-code-format` to its current default value of `false`?

---

_@charliermarsh reviewed on 2023-12-12 17:21_

---

_Review comment by @charliermarsh on `docs/configuration.md`:87 on 2023-12-12 17:21_

Yeah, I think that's fine for visibility. You can mention that it's opt-in for now.

---

_Review comment by @BurntSushi on `crates/ruff_workspace/src/options.rs`:2934 on 2023-12-12 17:23_

Yeah I think I do that sometimes. I'll add it here.

---

_@BurntSushi reviewed on 2023-12-12 17:23_

---

_Comment by @BurntSushi on 2023-12-12 17:50_

@charliermarsh I've added prose docs and fixed up your comments. A review of the prose docs would be great! Thanks.

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-12 17:50_

---

_@charliermarsh approved on 2023-12-12 17:52_

Excellent, thanks!

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:2902 on 2023-12-12 23:46_

```suggestion
    /// When this is enabled, Python code examples within docstrings are
```

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:3018 on 2023-12-12 23:49_

Do I remember correctly that we assume a code snipped uses Python when using Markdown or restructured text without specifying the language? If so, should we mention it in the documentation? While we aren't erroring or warning about invalid code snippets just yet, we may want to do so in the future.

---

_@MichaReiser approved on 2023-12-12 23:52_

---

_@charliermarsh reviewed on 2023-12-13 00:22_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:3018 on 2023-12-13 00:22_

I believe it's mentioned here: https://github.com/astral-sh/ruff/pull/8854/files#diff-d5764d76a6dcbb6934375ce2aec4a33aff239beab6ea71c447e624f9c973b63dR151

---

_@BurntSushi reviewed on 2023-12-13 13:06_

---

_Review comment by @BurntSushi on `crates/ruff_workspace/src/options.rs`:3018 on 2023-12-13 13:06_

I've also added a mention of that here in the setting docs.

---

_Merged by @BurntSushi on 2023-12-13 16:02_

---

_Closed by @BurntSushi on 2023-12-13 16:02_

---

_Branch deleted on 2023-12-13 16:02_

---

_Comment by @cpcloud on 2023-12-18 13:07_

This feature is most excellent, thank you for working on it.

---
