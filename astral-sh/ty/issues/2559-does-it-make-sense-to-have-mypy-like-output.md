```yaml
number: 2559
title: "Does it make sense to have mypy-like `--output-format concise` as the default?"
type: issue
state: open
author: ksnnsr
labels:
  - question
  - diagnostics
assignees: []
created_at: 2026-01-19T07:33:30Z
updated_at: 2026-01-19T11:12:18Z
url: https://github.com/astral-sh/ty/issues/2559
synced_at: 2026-01-19T11:30:09Z
```

# Does it make sense to have mypy-like `--output-format concise` as the default?

---

_@ksnnsr_

### Question

Hi team,

I just tried out `ty`. To get the output I expect, coming from mypy and having still a few type issues, I had to run:
```
uvx ty check --output-format concise
```

Else, there was a lot of helpful output, but it looked very overwhelming.

To me, the following makes more sense:
- Make `concise` the default
- Add a message after the output, along the lines
> To see a more verbose / detailed output, add `--output-format full` 

Does this sound sensible to you?

### Version

ty 0.0.12

---

_Label `question` added by @ksnnsr on 2026-01-19 07:33_

---

_Comment by @AlexWaygood on 2026-01-19 11:08_

Hey, thanks for trying out ty and opening the issue!

I think we're pretty unlikely to change our default to be `--output-format=concise`. One of our key selling points over other type checkers is that we try to give you enough information in the diagnostic message so that you can easily understand the cause of the problem and how you might fix the error. If we defaulted to concise output, most users wouldn't know that we're able to offer that information (most users don't think to research disabled-by-default output-format flags!), which I think would be a big shame. Using concise output by default would also be inconsistent with our other tools such as Ruff, which switched to use rich diagnostics by default in https://github.com/astral-sh/ruff/pull/12010. And it's the opposite direction to the direction mypy is probably going in (see https://github.com/python/mypy/issues/19108).

That said, I do agree that the output most first-time users see is probably pretty overwhelming at the moment, and I don't think that's a great first impression of the tool. Partly this is because first-time users are more likely than other users to have _lots_ of diagnostics when they use ty for the first time. One way to make that less overwhelming would be to automatically print all diagnostics to a pager if there are more than 2 or 3, so that you don't get a huge amount of information immediately printed to the terminal.

I do also agree that our verbose diagnostics are possibly a bit _too_ verbose at the moment. I've commented [elsewhere](https://github.com/astral-sh/ty/issues/1808#issuecomment-3628172011) that it looks like we're using a much larger context window for our code annotations at the moment than e.g. rustc does in its diagnoistcs. I think we could consider making it smaller, so that we give a more focussed view on the code we're trying to highlight, and so that diagnostics take up less vertical space in the terminal.

---

_Label `diagnostics` added by @AlexWaygood on 2026-01-19 11:09_

---
