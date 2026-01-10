```yaml
number: 8254
title: "formatter(Jupyter): ending semicolons valid in Jupyter"
type: issue
state: closed
author: henryiii
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-10-26T15:32:40Z
updated_at: 2023-11-10T16:23:37Z
url: https://github.com/astral-sh/ruff/issues/8254
synced_at: 2026-01-10T11:09:50Z
```

# formatter(Jupyter): ending semicolons valid in Jupyter

---

_Issue opened by @henryiii on 2023-10-26 15:32_

The Jupyter formatter has a couple of issues when running on notebooks.

One is it removes semicolons at the end of lines. `black[jupyter]` does not remove semicolons at the end of lines when running on a notebook, since it's used to mark output as hidden, for example when plotting. `plt.plot();` will show the plot but not print out the repr for the Artists that the plot method returns.

Edit: On the last line of a cell. Related: #7300 which is for the linter. But that's rule based, so it's easy to turn it off for `*.ipynb` files, while there isn't a workaround for the formatter, making it a blocker.

(Fixed in main) The other is that it can't read a notebook that black and ruff-lint are able to read. The error is:

```
error: Failed to format docs/examples/HistDemo.ipynb: source contains syntax errors: ParseError { error: UnrecognizedToken(Percent, None), offset: 0, source_path: "<filename>" }
```

I'm guessing it's choking on a line magic, perhaps? The file does have this line:

```python
%config InteractiveShell.ast_node_interactivity="last_expr_or_assign"
```

Which is the only `%` in the file.


---

_Comment by @charliermarsh on 2023-10-26 15:35_

Thanks!

---

_Label `bug` added by @charliermarsh on 2023-10-26 15:35_

---

_Label `formatter` added by @charliermarsh on 2023-10-26 15:35_

---

_Comment by @dhruvmanila on 2023-10-26 16:07_

Re line magic, it's solved https://github.com/astral-sh/ruff/issues/8204, sorry for the churn!

---

_Comment by @henryiii on 2023-10-26 16:10_

Great! Probably should have checked main. So just simicolons left.

---

_Renamed from "formatter(Jupyter): semicolons and percents" to "formatter(Jupyter): ending semicolons valid in Jupyter" by @henryiii on 2023-10-26 16:10_

---

_Comment by @dhruvmanila on 2023-10-26 16:18_

Related: https://github.com/astral-sh/ruff/issues/7300

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-27 02:01_

---

_Comment by @MichaReiser on 2023-10-27 02:02_

Is my assumption correct that this only applies to expression statements or are there other statements for which semicolons need to be preserved?

---

_Comment by @henryiii on 2023-10-27 02:11_

IPython has several modes. If you set:

```
%config InteractiveShell.ast_node_interactivity="last_expr_or_assign"
```

Then either `val = expr` or just `expr` will print if it's the final statement in the cell, unless there's a semicolon (see, for example, https://hist.readthedocs.io/en/latest/examples/HistDemo.html). By default, only `expr` will display, and `val = expr` won't. I'd say simply allowing a final semicolon in cells on expressions or assignment expressions would be fine.

IIRC `a.b =` and `a[b] =` statements don't display even in the enhanced mode, but could be wrong.

---

_Comment by @MichaReiser on 2023-11-03 14:03_

Note, there's now a helper function to extract the trailing semicolon. We can use it in the assignment (including augmented and annotated) and expression statement formatting

https://github.com/astral-sh/ruff/blob/2c84f911c4d57f3b436458f4d0e34fe10655cf76/crates/ruff_python_formatter/src/statement/mod.rs#L89-L107

---

_Comment by @dhruvmanila on 2023-11-03 14:25_

Black's implementation for reference: https://github.com/psf/black/blob/c54c213d6a3132986feede0cf0525f5bae5b43d6/src/black/handle_ipynb_magics.py#L73-L102. It seems to be removing them for all cases.

---

_Comment by @henryiii on 2023-11-03 14:49_

The function after that puts them back - not sure why you'd need to remove it and then put it back, since the normal formatter also removes them - just checking to see if it's there and then putting it back seems like it would also work. Anyway, what cases are not covered by assignments and expressions? Blocks can't end lines, for example. Simicolons aren't needed for import statements - I wonder if black keeps them, actually. :)

---

_Comment by @MichaReiser on 2023-11-03 15:00_

> Black's implementation for reference: [psf/black@`c54c213`/src/black/handle_ipynb_magics.py#L73-L102](https://github.com/psf/black/blob/c54c213d6a3132986feede0cf0525f5bae5b43d6/src/black/handle_ipynb_magics.py#L73-L102). It seems to be removing them for all cases.

Nice find. Removing them and adding them back sounds complicated. We can just preserve them 😄 

---

_Assigned to @dhruvmanila by @dhruvmanila on 2023-11-09 03:09_

---

_Closed by @dhruvmanila on 2023-11-10 16:23_

---
