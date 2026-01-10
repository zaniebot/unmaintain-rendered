```yaml
number: 9126
title: "Docstring formatting does not always respect line length in `dynamic` mode"
type: issue
state: closed
author: stinodego
labels:
  - bug
  - docstring
  - formatter
assignees: []
created_at: 2023-12-14T09:39:54Z
updated_at: 2023-12-14T14:53:46Z
url: https://github.com/astral-sh/ruff/issues/9126
synced_at: 2026-01-10T11:09:51Z
```

# Docstring formatting does not always respect line length in `dynamic` mode

---

_Issue opened by @stinodego on 2023-12-14 09:39_

First off, thanks so much for the great work on the docstring formatter! I'm very much looking forward to using it for the Polars code base.

When running the new docstring formatter on our code base, I found a number of examples where dynamic line length mode did not produce the expected result. See the three examples below:

```python
def example1():
    """
    Docstring example containing a class.

    Examples
    --------
    >>> @pl.api.register_dataframe_namespace("split")
    ... class SplitFrame:
    ...     def __init__(self, df: pl.DataFrame):
    ...         self._df = df
    ...
    ...     def by_first_letter_of_column_values(self, col: str) -> list[pl.DataFrame]:
    ...         return [
    ...             self._df.filter(pl.col(col).str.starts_with(c))
    ...             for c in sorted(
    ...                 set(df.select(pl.col(col).str.slice(0, 1)).to_series())
    ...             )
    ...         ]
    """


class Nested:
    def example2():
        """
        Regular docstring of class method.

        Examples
        --------
        >>> df = pl.DataFrame(
        ...     {"foo": [1, 2, 3], "bar": [6, 7, 8], "ham": ["a", "b", "c"]}
        ... )
        """


def example3():
    """
    Pragma comment.

    Examples
    --------
    >>> af1, af2, af3 = pl.align_frames(
    ...     df1, df2, df3, on="dt"
    ... )  # doctest: +IGNORE_RESULT
    """

```

In ruff `0.1.8`, running the ruff docstring formatter on these with `line-length = 88` will produce line lengths over 88.

I think examples 1 and 2 are clear violations. Example 3 is debatable whether the result is desired or not.

---

_Label `docstring` added by @MichaReiser on 2023-12-14 10:12_

---

_Label `formatter` added by @MichaReiser on 2023-12-14 10:12_

---

_Comment by @MichaReiser on 2023-12-14 10:15_

Thanks for the feedback! 

I wonder if this is happening because we only subtract $indent.level * indent.width$ but not necessarily the space required by ` >>> `? 

@BurntSushi would you mind taking a look

---

_Label `bug` added by @MichaReiser on 2023-12-14 10:17_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-12-14 10:17_

---

_Assigned to @BurntSushi by @MichaReiser on 2023-12-14 10:17_

---

_Comment by @stinodego on 2023-12-14 10:40_

> I wonder if this is happening because we only subtract  but not necessarily the space required by `>>> `?

That might be it actually. I figured it might be because of edge cases with classes or something, but indeed a simple example like this also exceeds the line length, but only by 4 characters:

```
def example3():
    """
    ...

    Examples
    --------
    >>> af1, af2, af3 = pl.align_frames(
    ...     df1, df2, df3, on="dt", foo="looooooooooong_string"
    ... )
    """
```


---

_Comment by @BurntSushi on 2023-12-14 13:12_

Nice find! I put up a fix in #9129.

@MichaReiser You were exactly correct! At least for this bug, it was indeed that the line length setting on the nested call to the formatter wasn't taking the extra spaces from the doctest prompt into account.

---

_Closed by @BurntSushi on 2023-12-14 14:53_

---
