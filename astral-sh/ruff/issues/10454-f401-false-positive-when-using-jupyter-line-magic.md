```yaml
number: 10454
title: "F401 false positive when using Jupyter line magic (`%timeit imported_thing()`)"
type: issue
state: open
author: flying-sheep
labels:
  - documentation
  - linter
  - notebook
assignees: []
created_at: 2024-03-18T11:04:42Z
updated_at: 2024-03-20T12:43:13Z
url: https://github.com/astral-sh/ruff/issues/10454
synced_at: 2026-01-10T11:09:52Z
```

# F401 false positive when using Jupyter line magic (`%timeit imported_thing()`)

---

_Issue opened by @flying-sheep on 2024-03-18 11:04_

I searched for “timeit F401” and didn’t find anything relevant.

Reproducer:

```python
import os

%timeit os.getcwd()
```

Will show a false positive of F401 (Unused import) for the `import os` line:

<img width="395" alt="image" src="https://github.com/astral-sh/ruff/assets/291575/6cf0fda6-118d-4ec3-912f-2f6bf726a1da">


---

_Label `rule` added by @AlexWaygood on 2024-03-18 11:59_

---

_Label `notebook` added by @AlexWaygood on 2024-03-18 11:59_

---

_Label `linter` added by @AlexWaygood on 2024-03-18 12:00_

---

_Comment by @MichaReiser on 2024-03-18 14:50_

@dhruvmanila I assume this is due to today's limitation where ruff doesn't parse out anything coming after the magic command because it could, literally, be anything (a shell command).  Do you know if this is documented somewhere, if not, should we add it to the FAQ section?

---

_Comment by @flying-sheep on 2024-03-18 17:01_

That’s not true for all magics, some contain statements. E.g. `%time` only has a statement following, and `%timeit` is defined like

> `%timeit [-n<N> -r<R> [-t|-c] -q -p<P> -o] statement`

So the statement could be parsed out.

Apart from [`%time`](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-time) and [`%timeit`](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit), the same applies only to [`%prun`](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-prun) and [`%debug`](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-debug) if I didn’t miss anything.

So all in all not too daunting.

---

_Comment by @dhruvmanila on 2024-03-19 08:41_

> I assume this is due to today's limitation where ruff doesn't parse out anything coming after the magic command because it could, literally, be anything (a shell command).

Yes, that's correct.

> Do you know if this is documented somewhere, if not, should we add it to the FAQ section?

I don't think it's documented anywhere except for in my comment here (https://github.com/astral-sh/ruff/issues/8354#issuecomment-1786380263).

We did introduce support for transparent cell magics (https://github.com/astral-sh/ruff/pull/8911), which means we only ignore the line where the magic command is defined, not the subsequent lines which might contain valid Python code.

> That’s not true for all magics, some contain statements. E.g. `%time` only has a statement following, and `%timeit` is defined like

Thank you for pointing that out. We could potentially support selectively parsing out the code after a line magic but only if it doesn't contain any options. This means that we could support `%time` but not `%timeit` for the reasons mentioned in the linked comment above.

> Apart from [`%time`](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-time) and [`%timeit`](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-timeit), the same applies only to [`%prun`](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-prun) and [`%debug`](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-debug) if I didn’t miss anything.

`%prun` would be difficult because it can contain options, and the same goes for `%debug`, although it only has a single option (`--breakpoint`).

I quickly scanned through the line magic lists and the ones you've mentioned are the only ones which can have a statement or expression after the magic command.

While we could potentially support %time, it would necessitate significant changes in the AST representation to store the parsed-out statement or expression, as well as adjustments to the semantic model and any affected lint rules. However, I don't believe it's worth the effort for a single command, especially one typically used as a one-off command.

---

_Comment by @dhruvmanila on 2024-03-19 08:41_

We should update the documentation to make this clear.

---

_Label `rule` removed by @dhruvmanila on 2024-03-19 08:41_

---

_Label `documentation` added by @dhruvmanila on 2024-03-19 08:41_

---

_Comment by @flying-sheep on 2024-03-19 09:40_

> but only if it doesn't contain any options

why add this limitation if we could just parse the options?

---

_Comment by @charliermarsh on 2024-03-19 13:19_

We can do that, but it requires writing a parser for the options and keeping that up-to-date with any changes in Jupyter.

---

_Comment by @flying-sheep on 2024-03-19 16:12_

That could end up being easy, depending on two factors:

1. are we able to reliably grab a magic expression from the Python parser and then call a function on the resulting slice? Then we can just run one of Rust’s existing high-quality CLI parsers, let it tell us how long the prefix of the slice is that’s options, and then try to parse the remainder as Python.
2. Does jupyter change these magics frequently enough that that’s a concern? And even if so, they probably just add options, so we could simply wait for someone running into a problem and then updating the option parser.

---

_Comment by @charliermarsh on 2024-03-19 17:08_

I support doing it, it's just work that someone needs to do :)

---

_Comment by @dhruvmanila on 2024-03-20 03:09_

One concern with this approach, and maybe it's not really a big problem, is that how do we differentiate between an option and something which looks similar to an option in the Python statement (`%timeit -n 10 x-n`).

---

_Comment by @flying-sheep on 2024-03-20 12:42_

By doing what I proposed 😉

there's no ambiguity if we know which options exist and which ones take a parameter and which ones don't.

---
