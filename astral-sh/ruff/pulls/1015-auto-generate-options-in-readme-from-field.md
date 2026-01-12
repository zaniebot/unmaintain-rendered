```yaml
number: 1015
title: Auto-generate options in README from field attributes
type: pull_request
state: merged
author: squiddy
labels: []
assignees: []
merged: true
base: main
head: auto-generate-options-readme
created_at: 2022-12-03T20:31:36Z
updated_at: 2022-12-06T05:20:56Z
url: https://github.com/astral-sh/ruff/pull/1015
synced_at: 2026-01-12T15:55:05Z
```

# Auto-generate options in README from field attributes

---

_@squiddy_

This implements a proc macro to derive metadata from the structs that define all available options. I'm opening this pull request primarly to get feedback on whether this is a sensible approach.

`src/settings/options.rs` shows how the attributes are used to add additional information to options.
`ruff_dev/src/generate_options.rs` shows how that is used to autogenerate the options in the readme.

I'm using macros here mostly to learn about them (which is why the macro implementation is pretty rough), but also because I think they fit well here. And they might help us structuring all the lints better, similar to clippy does it?

- [x] macro: cleanup code
- [x] macro: proper error handling
- [x] doc: complete root options
- [x] doc: complete flake8_annotations
- [x] doc: complete flake8_bugbear
- [x] doc: complete flake8_quotes
- [x] doc: complete flake8_tidy_imports
- [x] doc: complete eisort
- [x] doc: complete mccabe
- [x] doc: complete pep8_naming
- [x] doc: complete pyupgrade


---

_Renamed from "WIP Auto-generate options in README from field attributes" to "Auto-generate options in README from field attributes" by @squiddy on 2022-12-03 20:31_

---

_Comment by @charliermarsh on 2022-12-03 21:56_

This is really interesting! Learning a lot just from reading it (I'm not a macro expert by any means).

---

_Comment by @charliermarsh on 2022-12-03 22:01_

I think this makes a lot of sense. I have some small nits on the code but I'd def encourage you to follow this through to its conclusion (e.g., we need to handle all the various `Options` structs, support multiple sections, and so on), and I'm happy to merge it as soon as it generates the existing docs.

---

_Comment by @squiddy on 2022-12-04 09:35_

Thanks for the feedback.

The macro code itself is still a mess, but I started attributing more options. Groups are also supported now. I'm quite confident in the output right now, the diff in the README is small. Most changes result from different line wrapping and the order of options being different in the struct vs. the README.

---

_Comment by @charliermarsh on 2022-12-04 14:54_

If you want to focus on macro clean-up, I can commit to translating all of the remaining options to use that syntax once the PR is good to go.

---

_Comment by @charliermarsh on 2022-12-04 14:54_

(I don't mind the grunt work.)

---

_Comment by @squiddy on 2022-12-04 15:28_

Oh yeah, that would be great. Might also be a good opportunity to spot things that I could still improve.

I think I made some good steps with the macro cleanup already (learning on the go), will push that later.

---

_Comment by @squiddy on 2022-12-05 07:03_

This is ready for a closer review. Cleaned up the macro as far as my current abilities go. I had some other ideas, but those are perhaps not necessary in this PR.

**automated default and type**

Given something like `Option<bool>` or `Option<Vec<Something>>`, automatically assume the default is `false` or `[]` - unless explicitely overriden.

**parse default directly as tokens, not as a string**

Instead of writing ```default = "`[]`"``` or ```default = r#"`"text"`"#```, allow to just write `default = []` or `default = "text"`.

**validate example**

We already have a dependency on toml, so we could try to parse the example code to verify it's actually valid syntax.

---

And perhaps a CI step to run those commands that update the README and ensure there is no diff, i.e. when adding options / lints make sure the PR already ran those commands?

---

_Marked ready for review by @squiddy on 2022-12-05 07:47_

---

_Comment by @charliermarsh on 2022-12-05 15:12_

Cool, will give this a full review and then hopefully merge today after converting the remaining options.

---

_Comment by @charliermarsh on 2022-12-05 22:23_

(Doing this now :))

---

_Comment by @charliermarsh on 2022-12-06 00:14_

Will finish this tonight. I'd like to tweak the ordering of the options. It'd be nice, too, if `per-file-ignores` could appear at the top, despite being at the bottom of the struct for dumb TOML reasons.

---

_@eddyg reviewed on 2022-12-06 03:06_

---

_Review comment by @eddyg on `src/settings/options.rs`:30 on 2022-12-06 03:06_

Is the documentation for this option missing, or am I just not understanding this PR? 😃 

---

_@charliermarsh reviewed on 2022-12-06 03:08_

---

_Review comment by @charliermarsh on `src/settings/options.rs`:30 on 2022-12-06 03:08_

The documentation for that option is missing, thank you :)

(Bad rebase, it was added after.)

---

_@eddyg reviewed on 2022-12-06 03:18_

---

_Review comment by @eddyg on `README.md`:1299 on 2022-12-06 03:18_

this should say: 

> ... and the asterisk-operator (U+2217)

(assuming you want to use `"*"` and `"∗"` in the example) 😉 

---

_Review comment by @charliermarsh on `README.md`:1299 on 2022-12-06 03:24_

Thanks!

---

_@charliermarsh reviewed on 2022-12-06 03:24_

---

_Merged by @charliermarsh on 2022-12-06 03:34_

---

_Closed by @charliermarsh on 2022-12-06 03:34_

---

_Branch deleted on 2022-12-06 05:20_

---
