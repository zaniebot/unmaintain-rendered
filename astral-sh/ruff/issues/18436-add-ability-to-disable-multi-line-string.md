---
number: 18436
title: Add ability to disable multi-line string concatenation with ruff format
type: issue
state: open
author: seanbudd
labels:
  - formatter
  - style
assignees: []
created_at: 2025-06-03T05:38:56Z
updated_at: 2025-11-19T18:14:37Z
url: https://github.com/astral-sh/ruff/issues/18436
synced_at: 2026-01-10T01:22:59Z
---

# Add ability to disable multi-line string concatenation with ruff format

---

_Issue opened by @seanbudd on 2025-06-03 05:38_

### Summary

Please change ruff format's behaviour for concatenating multi-line strings.

For example:

```py
outputFile.write(
	"</notes>\n"
	f"<segment>\n"
	f"<source>{xmlEscape(source)}</source>\n"
	"</segment>\n"
	"</unit>\n",
)
```

Should not concatenate to 

```py
outputFile.write(
	f"</notes>\n<segment>\n<source>{xmlEscape(source)}</source>\n</segment>\n</unit>\n",
)
```

I cannot find a way to disable this behaviour - or disabling string concatenation in general when using ruff format.
Any suggestions on how to use ruff format without multiline string concatenation would be helpful.

### Reasoning

Multi-line strings split over new lines is usually a very intended form of syntax.
Splitting strings over multiple lines at fixed positions helps with readability.
I think there is more value in disabling this feature for ruff format than having it enabled.


---

_Referenced in [nvaccess/nvda#17671](../../nvaccess/nvda/pulls/17671.md) on 2025-06-03 05:39_

---

_Label `formatter` added by @MichaReiser on 2025-06-03 06:57_

---

_Label `style` added by @MichaReiser on 2025-06-03 06:57_

---

_Comment by @MichaReiser on 2025-06-03 07:00_

There's currently no option to disable implicit string concatenation and you're right, it doesn't respect the trailing comma. I don't think it should respect the magic trailing comma, because this would make this transformation useless in most cases (e.g., you have an array that already has a trailing comma but you added an implicit string concatenation).

What's the reason you want to keep the lines separate? Is it because they all end with a `\n`? We could explore a heuristic that prevents joining implicit concatenated strings if they all end with a newline and were already formatted over mutliple lines (in which case the formatter would preserve the formatting as is)

---

_Comment by @seanbudd on 2025-06-03 08:39_

It's more that it's quite useful to split strings over new lines for readability. Just like how nested python calls crammed into one line is not as ideal as splitting logic over multiple lines. Using `fmt: skip` each time is an option, but the trailing comma pattern is cleaner. I'd probably prefer to turn the whole thing concatenation off than have it on by default and using `fmt: skip`

---

_Comment by @seanbudd on 2025-06-03 08:48_

Also I understand the case you mention as to why trailing commas might not work. I guess an alternative marker or just a way to toggle it off in general would be nice.

---

_Comment by @MichaReiser on 2025-06-03 08:50_

Do you have other examples where you would want to turn it off? I'm open to adding a heuristic; it's less likely that we'll add an option unless there's big demand for it (please upvote the issue)

---

_Comment by @seanbudd on 2025-06-03 08:56_

There's no real special use case, it's more the general desire to keep multiline strings as such. Generally splitting strings like this is quite intentional. I really don't see how compacting intentional newlines improves code readability. The fixes for implicit same line strings is still useful. 

---

_Renamed from "Respect magic trailing comma when considering multi-line string concatenation with ruff format" to "Add ability to disable multi-line string concatenation with ruff format" by @seanbudd on 2025-06-04 02:09_

---

_Referenced in [dask/dask#12126](../../dask/dask/pulls/12126.md) on 2025-11-13 21:52_

---

_Referenced in [astral-sh/ruff#21526](../../astral-sh/ruff/issues/21526.md) on 2025-11-19 16:13_

---

_Comment by @KerberosMorphy on 2025-11-19 18:14_

> There's no real special use case, it's more the general desire to keep multiline strings as such. Generally splitting strings like this is quite intentional. I really don't see how compacting intentional newlines improves code readability. The fixes for implicit same line strings is still useful.

I confirm, in my team, multi-line strings are written intentionally for clearer readability and therefore maintainability.

---
