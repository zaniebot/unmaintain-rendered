```yaml
number: 2600
title: "Feature: Support isort multi_line_output option"
type: issue
state: open
author: mikelane
labels:
  - isort
  - needs-decision
assignees: []
created_at: 2023-02-06T01:41:27Z
updated_at: 2025-08-04T06:57:23Z
url: https://github.com/astral-sh/ruff/issues/2600
synced_at: 2026-01-10T11:09:45Z
```

# Feature: Support isort multi_line_output option

---

_Issue opened by @mikelane on 2023-02-06 01:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

The isort module provides many different ways of accomplishing the wrapping for a given project. From my experience, this is a widely used option in projects that use isort, so it would be nice to have this support in ruff.

Ideally, the ruff setting would be something like this:

```python
[tool.ruff.isort]
multi_line_output = 3  # Vertical Hanging Indent
```

The output options would follow the [options settings](https://pycqa.github.io/isort/docs/configuration/options#multi-line-output) and [option values](https://pycqa.github.io/isort/docs/configuration/multi_line_output_modes.html) listed in the isort documentation.

---

_Label `isort` added by @charliermarsh on 2023-02-06 15:46_

---

_Comment by @NeilGirdhar on 2023-02-16 22:26_

If we're re-engineering this, a compact grid mode would be really nice.  That is, try:
- putting everything on one line,
- grid, and
- hanging grid.

Choose the one with the fewest lines that fit within the maximum line width.  I contributed [something like this](https://github.com/PyCQA/isort/pull/695) for isort.  But that contribution only bumps to hanging grid when grid is impossible.  It would be nice to also bump when hanging grid is shorter.

(Also, just my opinion, but mode 2 that uses line continuation `\` should probably be deprecated?)

---

_Comment by @spaceone on 2023-04-14 04:48_

We need `multi_line_output=5` (`vert-grid-grouped`).

---

_Comment by @nicwolff on 2023-05-02 16:29_

Same, we'd have moved from `isort` to `ruff` if it supported `multi_line_output=5`.

---

_Comment by @NeilGirdhar on 2023-08-08 21:22_

Thinking about this more, I would like to see the options in Ruff be something like:
```toml
imports_allow_grid = true
imports_allow_vertical = true
imports_parenthesis_on_newline = false
```

Then the most common ISort modes correspond to:

* 0: (true, true, false)
* 1: (false, true, false)
* 3: (false, false, true)
* 4: (true, false, false)
* 5: (true, false, true)

And add an option to `imports_choose_shortest` that chooses the shortest representation allowed that fits the options.  So just be cause you chose (true, true, false), doesn't mean that it should do somethign crazy like:
```python 
from alksdjflks_flaksjdflkajsdfljka_laskjdf_alksjdf_alksjdflaskd_skldj import (some_symbol,
                                                                               some_other_symbol,
                                                                               some_third_symbol,
                                                                               some_fourth_symbol)
# Instead:
from alksdjflks_flaksjdflkajsdfljka_laskjdf_alksjdf_alksjdflaskd_skldj import (
    some_symbol, some_other_symbol, some_third_symbol, some_fourth_symbol)
```

I'd love to stop using isort, but I don't want to lose my nice imports.

---

_Comment by @ThiefMaster on 2023-11-13 22:45_

Would love to see support for `multi_line_output=0`, can't ditch isort until that's available :)

---

_Comment by @NeilGirdhar on 2023-11-13 23:04_

@ThiefMaster That's the one I'm waiting for too.  But really, what we need is for Ruff to support [both _vertical_ and _grid_ formatting](https://github.com/astral-sh/ruff/issues/8197).  If we had that, we would have it in the code and the imports.

---

_Comment by @dhirschfeld on 2023-11-13 23:52_

I used to use `black`, `ruff` and `isort` to format my code. Now I just use `ruff` and `isort`.

When `ruff` supports `multi_line_output=3` I'll be able to just use `ruff`.

Until such time... I'll continue to monitor this issue 😜 


---

_Comment by @Aeron on 2023-11-24 12:30_

@dhirschfeld, wait a second. The default formatting behavior is `multi_line_output=3`, right? It is the vertical hanging indent mode. Am I missing something? I briefly tested it against `isort` and saw no difference.

Full disclosure: I started working on this feature yesterday since I have some time and attention to spare, but I don’t want to promise anything yet.

---

_Label `needs-decision` added by @MichaReiser on 2023-11-27 02:46_

---

_Comment by @MichaReiser on 2023-11-27 02:51_

I understand that we have different preferences when it comes to formatting code. Black and ruff's formatter have sofar been leaning towards opinionated formatting with few options whereas Ruff's linter is extremely configurable. Ruff's `isort` is somewhere in between. I believe @charliermarsh expressed once that he intentionally didn't implement all isort options and we're considering making `isort` its own tool or integrating it into the formatter. This is why I think we should be very deliberate about supporting new isort options that are incompatible with the formatter because we either need to deprecate them again in the future, or they force us to change the direction of the formatter. 


---

_Comment by @NeilGirdhar on 2023-11-27 03:08_

> nd we're considering making `isort` its own tool or integrating it into the formatter.

I think integrating it into the formatter makes perfect sense.  After all, all of these "isort modes" that people are discussing are actually special cases of various formatter output preferences.  Ruff currently supports only three output types (single line, hanging single line, and hanging multi-line).

> This is why I think we should be very deliberate about supporting new isort options 

I agree.  I think it makes sense for the import statement formatting to be consistent with code formatting.  Therefore, I don't think there should be any isort options at all.  What we are really asking for is generalization of the formatter.  I understand that that may not be a priority today, but hopefully it will come eventually. 

---

_Comment by @Aeron on 2023-11-27 14:45_

I had an idea those two could be separate things. The new built-in formatter may have a few opinionated options to organize imports or none at all, and the `ruff-isort` formatter can be its own thing for people who want a backward-compatible way of doing it.

But yeah, you’re right—formatting kicks after the import organization and ruins the latter if the result is incompatible with those three output types. So the option I described above only would work if users of `ruff-isort` ditch the new formatter, which is highly unlikely, considering everybody wants a complete package.

I’m good with the current vertical hanging indent approach since I’m using it anyway. But people want grids. Yet meddling with the new formatter is a different beast than adding support for `isort` modes separately.

Ok, I put my implementation on pause then. It’ll wait until a decision.

---

_Comment by @nineteendo on 2024-02-10 14:27_

I believe this would also benefit [RUF022](https://docs.astral.sh/ruff/rules/unsorted-dunder-all):
```python
__all__ = [
    "a", "b", "c",
]
```

---

_Comment by @eli-schwartz on 2024-02-22 19:44_

> I understand that we have different preferences when it comes to formatting code. Black and ruff's formatter have sofar been leaning towards opinionated formatting with few options whereas Ruff's linter is extremely configurable. Ruff's `isort` is somewhere in between. I believe @charliermarsh expressed once that he intentionally didn't implement all isort options and we're considering making `isort` its own tool or integrating it into the formatter. This is why I think we should be very deliberate about supporting new isort options that are incompatible with the formatter because we either need to deprecate them again in the future, or they force us to change the direction of the formatter.

As someone who is currently uninterested in the formatter, but interested in the linter, I would be happy if `ruff check --select I` supported all isort modes (mode 5 for me personally), and don't have an opinion on whether `ruff format` should.

Maybe it could raise a fatal error that says those modes aren't allowed in the formatter?

---

_Comment by @vkbo on 2024-04-24 10:09_

I would love to see this feature added as well. A lack of mode 5 support is pretty much the only thing preventing me from using ruff. The current formatting option for imports is incredibly wasteful for code with a lot of short imports in few modules, like my Qt5-based project where you import a lot of GUI components in every file.

---

_Comment by @dhirschfeld on 2025-06-02 02:03_

> [@dhirschfeld](https://github.com/dhirschfeld), wait a second. The default formatting behavior is `multi_line_output=3`, right? It is the vertical hanging indent mode. Am I missing something? I briefly tested it against `isort` and saw no difference.

Circling back, `ruff` does what I want iff the imports are longer than the line length.

AFAICT `ruff` will leave the below unchanged:
```python
from utils import func1, func2
```

But, to get cleaner diffs I want it to be:
```python
from utils import (
    func1,
    func2,
)
```

I think, for my usecase, what is missing from `ruff` is `force_grid_wrap`:
- https://github.com/astral-sh/ruff/issues/2601

---

_Comment by @miktuy on 2025-07-29 17:04_

Hi! Could you support option multi_line_output=4 like in isort?
We have huge amount imports in our projects's tests and this option provides more readable import's section in files with tests. But isort is so slow and we would prefer to use ruff to sort the imports.

---
