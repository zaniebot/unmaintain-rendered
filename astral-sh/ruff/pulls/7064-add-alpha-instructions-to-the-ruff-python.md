```yaml
number: 7064
title: "Add alpha instructions to the `ruff_python_formatter` README"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/formatter
created_at: 2023-09-02T16:12:59Z
updated_at: 2023-09-06T12:01:33Z
url: https://github.com/astral-sh/ruff/pull/7064
synced_at: 2026-01-12T02:45:38Z
```

# Add alpha instructions to the `ruff_python_formatter` README

---

_Pull request opened by @charliermarsh on 2023-09-02 16:12_

## Summary

Adds instructions to cover the formatter's goals, current status, and how-to for using it (in its alpha state) via the CLI and VS Code extension.

I haven't yet documented the known deviations from Black. I will fill those in.


Edit Micha: [Rendered Readme](https://github.com/astral-sh/ruff/blob/charlie/formatter/crates/ruff_python_formatter/README.md)


---

_Label `formatter` added by @charliermarsh on 2023-09-02 16:13_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-02 16:28_

---

_Review requested from @zanieb by @charliermarsh on 2023-09-02 16:28_

---

_Review requested from @konstin by @charliermarsh on 2023-09-02 16:28_

---

_@charliermarsh reviewed on 2023-09-02 16:35_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/README.md`:133 on 2023-09-02 16:35_

What do you think is the right format here? Just copy over the deviations we started cataloging in Notion, and then use this as the source of truth? It's nice to set this up such that we can link to sections here when folks file issues with known deviations.

---

_Review comment by @charliermarsh on `.pre-commit-config.yaml`:27 on 2023-09-03 22:05_

This is causing problems since we now have intentional non-Black-formatted snippets in the README. It seems like we'll run into this often now that we use our own formatter. I'm partial to removing, but also happy to add a file exclusion here if there's any hesitancy.

---

_@charliermarsh reviewed on 2023-09-03 22:05_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-09-04 09:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:12 on 2023-09-04 09:10_

We may need to be careful with the framing around *direct integration* with Ruff depending on the outcome of our outstanding discussion on aligning the configuration / CLI interface.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:15 on 2023-09-04 09:11_

TODO: Add explanation link for Jaccard similarity

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:22 on 2023-09-04 09:11_

```suggestion
filed in the issue tracker. If you've identified a new deviation, please [file an issue](https://github.com/astral-sh/ruff/issues/new).
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:49 on 2023-09-04 09:13_

What's the use case for this option? What gets written to the file when using `ruff format --check --o file.txt`?

The default isn't correct. The default is to overwrite the input file.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:51 on 2023-09-04 09:13_

The formatter only supports [Python version that supports parenthesized with items, Py311]. I think we should remove this option for now.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:78 on 2023-09-04 09:14_

I would not make this promise today because there's an outstanding discussion about whether we want to do this (we're leaning towards yes, but it comes back to how tightly we want to integrate the formatter into ruff)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:82 on 2023-09-04 09:15_

TODO for the future: Rename the extension to `astral.ruff`?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:98 on 2023-09-04 09:16_

Should we add a note that we plan to add range formatting in future releases?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:114 on 2023-09-04 09:16_

The *on the CLI* is unclear to me. How can I provide the line length via the CLI?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:117 on 2023-09-04 09:17_

Depending on how complete the list should be:

* missing semicolons
* line ending
* tab width

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:172 on 2023-09-04 09:19_

```suggestion
Pragma comments (`# type:`, `# noqa`, `pyright:`, and `pylint:`) are ignored when computing the width of a line.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:179 on 2023-09-04 09:20_

Black's behavior for `type: ignore` is different: It avoids splitting any line that has a type ignore comment, limiting formatting mainly to normalizing spaces. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:207 on 2023-09-04 09:21_

It may be worth specifying the main difference is that Ruff uses the unicode width for identifiers and comments in addition to strings (Black)

---

_@MichaReiser reviewed on 2023-09-04 09:26_

This is a nice write-up! I think we need to revisit the format CLI options (CC: @konstin)

---

_@MichaReiser reviewed on 2023-09-04 09:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:304 on 2023-09-04 09:27_

Is the goal to document all deviations or the most prominent once? There are e.g. some more string formatting deviations https://www.notion.so/astral-sh/Black-Deviations-65977bec23b64aee9d28cc1787badfdb?pvs=4#df068d4a5638465ab95e944f45daac55

---

_@konstin reviewed on 2023-09-04 09:46_

---

_Review comment by @konstin on `crates/ruff_python_formatter/README.md`:15 on 2023-09-04 09:46_

i'm fine with stating this as ">99.9% of lines on projects [...] are formatted identical to black", the exact metric chosen doesn't correlate strongly with the user experience.

---

_Review comment by @konstin on `crates/ruff_python_formatter/README.md`:7 on 2023-09-04 09:49_

Do we want to make a dedicated discussion/issue/form/etc. for feedback?

---

_@charliermarsh reviewed on 2023-09-04 10:13_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/README.md`:49 on 2023-09-04 10:13_

Ah yeah, we should probably remove this. It may write the formatter output (but not the changed file), or nothing at all.

---

_@charliermarsh reviewed on 2023-09-04 10:13_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/README.md`:51 on 2023-09-04 10:13_

Is it the case that we would add parentheses that weren’t already present here?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/README.md`:304 on 2023-09-04 10:15_

My hope was to document all intentional deviations here so that we have something to link to when issues come in (and we don’t have to re-explain them every time). Meanwhile, unintentional deviations (which we intend to fix) would be tracked in the issue tracker. So I do think it’d be useful to track all intentional deviations here.

---

_@charliermarsh reviewed on 2023-09-04 10:15_

---

_Review comment by @konstin on `crates/ruff_python_formatter/README.md`:51 on 2023-09-04 11:58_

I'm "Currently has no effect" without removing it

---

_Review comment by @konstin on `crates/ruff_python_formatter/README.md`:119 on 2023-09-04 12:00_

```suggestion
### Excluding code from formatting
```

---

_Review comment by @konstin on `crates/ruff_python_formatter/README.md`:172 on 2023-09-04 12:02_

I like `# type: ignore`, you shouldn't be using other type comments anymore (they were a python 2 compatibility layer)

---

_@konstin approved on 2023-09-04 12:04_

---

_@MichaReiser reviewed on 2023-09-04 14:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/README.md`:172 on 2023-09-04 14:48_

The reason I would change it is because the formatter excludes all comments starting with `type: ` from the measured width, not just `type: ignore`

---

_Comment by @charliermarsh on 2023-09-04 16:16_

Thanks all. Since there aren’t any objections to the overall structure, I’ll probably do one pass on this based on feedback, then merge and we can follow-up in subsequent PRs.

I’ll also make some edits to the CLI (remove output file for the formatted, tweak the comment on version), but anyone is welcome to beat me to those.

---

_@tmke8 reviewed on 2023-09-05 10:19_

---

_Review comment by @tmke8 on `crates/ruff_python_formatter/README.md`:261 on 2023-09-05 10:19_

I think this docstring should be single-quoted, given the above description?

---

_@T-256 reviewed on 2023-09-05 16:08_

---

_Review comment by @T-256 on `crates/ruff_python_formatter/README.md`:252 on 2023-09-05 16:08_

```suggestion
### Insert newlines on all docstrings literals
```
IIUC here by `single-quoted` you mean `"` and `'`.
`single-quoted` word has problem for others to understand, first time I thought it means just `'`.

---

_@charliermarsh reviewed on 2023-09-05 17:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/README.md`:261 on 2023-09-05 17:05_

Thank you!

---

_@charliermarsh reviewed on 2023-09-05 17:08_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/README.md`:7 on 2023-09-05 17:08_

This would be a good use-case for issue templates...

---

_Merged by @charliermarsh on 2023-09-06 11:55_

---

_Closed by @charliermarsh on 2023-09-06 11:55_

---

_Branch deleted on 2023-09-06 11:55_

---

_Comment by @github-actions[bot] on 2023-09-06 12:01_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
