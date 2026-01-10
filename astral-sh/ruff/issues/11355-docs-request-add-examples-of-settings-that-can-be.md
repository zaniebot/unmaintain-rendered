```yaml
number: 11355
title: "Docs Request: Add examples of settings that can be turned on and off"
type: issue
state: closed
author: DeflateAwning
labels:
  - documentation
assignees: []
created_at: 2024-05-09T22:21:49Z
updated_at: 2024-07-26T01:28:50Z
url: https://github.com/astral-sh/ruff/issues/11355
synced_at: 2026-01-10T11:09:53Z
```

# Docs Request: Add examples of settings that can be turned on and off

---

_Issue opened by @DeflateAwning on 2024-05-09 22:21_

I wish there were way more examples of the possible configuration settings in ruff. For example, it would be great to see an example of how to flip between these two auto-format modes:

```python
# option 1 (default):
df = df.rename(
	{
		'source_name': 'second_merge_source_name',
	}
)

# option 2 (how I wish it was, by configuring it):
df = df.rename({
	'source_name': 'second_merge_source_name',
})
```

---

_Comment by @dhruvmanila on 2024-05-10 05:27_

I think this style is available in `--preview` mode of the formatter.

Agreed that a list of currently active preview style in the formatter should be visible.

---

_Label `documentation` added by @dhruvmanila on 2024-05-10 05:27_

---

_Comment by @charliermarsh on 2024-05-13 01:54_

It's all just under `--preview`, so there aren't really other settings to document or provide examples around. I'd vote to close, I think.

---

_Comment by @DeflateAwning on 2024-05-13 02:00_

Oh neat! I think that should be added to the README and the "Configuring Ruff" section(s) of the docs then. 

---

_Comment by @charliermarsh on 2024-05-13 02:01_

There's a separate section here: https://docs.astral.sh/ruff/preview/. Let me look into adding references to it elsewhere...

---

_Comment by @DeflateAwning on 2024-05-13 02:05_

Oh sorry, I think the point of my feature request might be being missed.

I'm glad that my specific formatting example is supported in the preview version! That's great.

**My real requests here though** is that the list of settings that can be enabled/disabled should be clearly laid out somewhere in the documentation, with examples.

---

_Comment by @charliermarsh on 2024-05-13 02:07_

We enumerate al the settings here (https://docs.astral.sh/ruff/settings/#format), but the issue is that for the formatter, we don't really support many settings, and the ones that we _do_ support are either already documented in such a way or are self-explanatory.

---

_Comment by @DeflateAwning on 2024-05-13 02:17_

Perhaps the settings section could be split into 3 ~~sections~~ _sub-pages_: "General", "Formatting Rules", and "Linting Rules". That page is quite long, and I missed that the section half-way down was the actual formatting rules. 

Also, where are Preview formatting rules enumerated? 

---

_Comment by @charliermarsh on 2024-05-13 02:20_

The page is an API reference though. It's intended to mirror the schema of the configuration file.

There are no preview formatting rules, per se. The _linter_ is composed of _rules_, while the _formatter_ just implements a single consistent style. For the formatter, you can either enable preview (and opt-in to style changes that haven't yet stabilized) or not, but there's no fine-grained control, and that's intentional. You should try running preview and see if you prefer the style.


---

_Comment by @dhruvmanila on 2024-05-13 07:57_

I think what my suggestion here was to create a list of formatting styles which will be enabled in preview mode.

For example, we could just link it to the issue / PR (like https://github.com/astral-sh/ruff/issues/8897) or provide a brief description on the style with an example.

---

_Comment by @charliermarsh on 2024-05-13 14:18_

I don't know that it's worth it given that we have to keep it up-to-date, and yet preview is all-or-nothing, so users can't leverage that information to change their code style. What do you think?

---

_Comment by @dhruvmanila on 2024-05-13 14:53_

As a user, I think I'd find it useful even if it's just a list of issue / PR list. If I enable preview formatting, I'm not sure what I'm going to see unless I run the formatter.

And, I think it should be pretty low effort as the list is already available here: https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_formatter/src/preview.rs

---

_Comment by @DeflateAwning on 2024-05-13 18:17_

What's the timescale of features making it from "preview" to regular mode (where the settings can be adjusted)? Are preview features ever scrapped?

---

_Comment by @dhruvmanila on 2024-05-14 08:35_

> What's the timescale of features making it from "preview" to regular mode (where the settings can be adjusted)?

Are you specifically referring to preview rules in the linter or the preview mode in the formatter? I'm assuming it's the latter. If so, it's usually inline with Black's preview to stable upgrade schedule which is at the start of a new year. You can read more about preview mode [here](https://docs.astral.sh/ruff/preview/) and in the [versioning docs](https://docs.astral.sh/ruff/versioning/#preview-mode).

---

_Closed by @charliermarsh on 2024-07-26 01:28_

---
