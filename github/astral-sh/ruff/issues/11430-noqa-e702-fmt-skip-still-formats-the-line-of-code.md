---
number: 11430
title: "`# noqa: E702 # fmt: skip` still formats the line of code"
type: issue
state: open
author: ngoctnq
labels:
  - suppression
  - formatter
assignees: []
created_at: 2024-05-15T07:21:13Z
updated_at: 2024-06-21T23:39:42Z
url: https://github.com/astral-sh/ruff/issues/11430
synced_at: 2026-01-07T13:12:15-06:00
---

# `# noqa: E702 # fmt: skip` still formats the line of code

---

_Issue opened by @ngoctnq on 2024-05-15 07:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Input code:
```python
print(); print() # noqa # fmt: skip
```

After formatting:
```python
print()
print()  # noqa # fmt: skip
```

`# fmt: on/off` works fine though, but I want to have just one line of short boilerplate code.

I'm using the Ruff VS Code extension, which is said to ship with `ruff==0.4.1`


---

_Comment by @dhruvmanila on 2024-05-15 08:20_

I think it's because there are two statements on the same line and `fmt: skip` is being applied only for the last statement. This can be seen by adding a single-quoted string (https://play.ruff.rs/edf37109-eb34-4b5c-9867-dd5587bef085), notice that the formatter only changes the quote for the first print call and not the second one. I don't think `RUF028` will indicate here because the suppression comment is actually being used by the formatter.

I'm not exactly sure if this is the intended behavior, @konstin any thoughts here? @MichaReiser  will be able to provide more context here but he's currently on PTO until the end of next week. I can possibly look into it to confirm whether this is the intended behavior.

---

_Label `suppression` added by @dhruvmanila on 2024-05-15 08:20_

---

_Label `formatter` added by @dhruvmanila on 2024-05-15 08:20_

---

_Comment by @konstin on 2024-05-15 11:40_

Iirc the end-of-line format off comment applies to the statement preceding it, and the two prints are two separate statements, but micha will know better

---

_Comment by @MichaReiser on 2024-05-27 08:41_

Oh, that's interesting and not something I have thought of when implementing suppression comments. 

> `# fmt: skip` comments suppress formatting for a preceding statement, case header, decorator, function definition, or class definition:

Today's behavior matches our documentation. However, Black doesn't format simple statements that end with a `fmt: skip` comment. So it is at least a Black incompatibility (and possibly not flagged by the ruff lint rule).

I'm open to changing our implementation to support this use case if it doesn't add too much complexity. I otherwise recommend documenting the deviation because it's possible to suppress the example using `fmt: off` (although it is slightly more verbose).

```python
# fmt: off
print(); print() # noqa 
# fmt: on

print( )
```



---

_Comment by @Gatsby-Lee on 2024-06-18 07:18_

> Oh, that's interesting and not something I have thought of when implementing suppression comments.
> 
> > `# fmt: skip` comments suppress formatting for a preceding statement, case header, decorator, function definition, or class definition:
> 
> Today's behavior matches our documentation. However, Black doesn't format simple statements that end with a `fmt: skip` comment. So it is at least a Black incompatibility (and possibly not flagged by the ruff lint rule).
> 
> I'm open to changing our implementation to support this use case if it doesn't add too much complexity. I otherwise recommend documenting the deviation because it's possible to suppress the example using `fmt: off` (although it is slightly more verbose).
> 
> ```python
> # fmt: off
> print(); print() # noqa 
> # fmt: on
> 
> print( )
> ```

yep. basically, I banged my head a few hours to figure this out.
And, I finally just changed to Black formatter again.

Black doesn't format the line which has more than one statement and tagged with "# fmt: skip" 

---

_Comment by @Gatsby-Lee on 2024-06-18 07:18_

@ngoctnq 

thank you for writing this issue.

---

_Comment by @Peiffap on 2024-06-21 23:39_

Related: #11216.

---

_Referenced in [astral-sh/ruff#13371](../../astral-sh/ruff/issues/13371.md) on 2024-09-16 16:19_

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-09-19 13:37_

---

_Referenced in [astral-sh/ruff#20633](../../astral-sh/ruff/pulls/20633.md) on 2025-10-06 20:40_

---

_Referenced in [astral-sh/ruff#21504](../../astral-sh/ruff/pulls/21504.md) on 2025-11-18 18:04_

---
