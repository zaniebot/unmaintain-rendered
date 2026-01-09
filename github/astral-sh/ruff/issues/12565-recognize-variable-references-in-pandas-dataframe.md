---
number: 12565
title: "Recognize variable references in pandas Dataframe.query (don't trigger F841)"
type: issue
state: open
author: astrowonk
labels:
  - needs-decision
assignees: []
created_at: 2024-07-29T11:27:48Z
updated_at: 2025-08-28T03:44:21Z
url: https://github.com/astral-sh/ruff/issues/12565
synced_at: 2026-01-07T13:12:15-06:00
---

# Recognize variable references in pandas Dataframe.query (don't trigger F841)

---

_Issue opened by @astrowonk on 2024-07-29 11:27_

This is essentially the same as this open [pylint issue](https://github.com/pylint-dev/pylint/issues/3903).

Pandas has string [queries](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.query.html) that can refer to variables prefaced with the @ symbol. So if one creates a variable and then only uses it in a pandas query, ruff flags the variable as unused (as does pylint and probably Flake). Here's a toy example:

```{python}

df = pd.DataFrame({'row': [1, 2], 'X': [3, 4]})


def my_func(x):
    my_row = x**2
    df.query('row == @my_row')

```

The only package that seems to address this is [Sonar Python](https://sonarsource.atlassian.net/browse/SONARPY-1166) (admittedly I'm unfamiliar with the particulars, but I found that issue googling). Suffice to say this is an edge case and library specific, but if possible it would be great if ruff could acknowledge that these variables are used. I can noqa the lines of course, but that's never ideal.

Based on the linked issue this sounds too hard for pylint to implement (the issue was opened in 2020) but maybe it's easier to handle with ruff? Thanks!



---

_Comment by @AlexWaygood on 2024-07-29 13:47_

Interesting! Thanks for opening the issue.

My instinct is that this would be _possible_ for us to implement... but that we probably shouldn't. To implement this, we'd have to parse all strings passed to `df.query()` calls according to the mini-language that pandas uses for string queries. The reasons why I think this might be a bad idea are:
- I assume pandas's mini-language isn't governed by any kind of standard, and that it could change at any time. They _probably_ aren't going to introduce any hugely breaking changes, but that's not necessarily something we can rely on. Also, if they introduce new features to this mini-language, we'd be expected to keep up with them and implement them here as well.
- It feels like something of a slippery slope. If we start special-casing the mini-language pandas uses for their query strings, we'll probably have to start doing similarly for competing frameworks as well.
- It feels like it would add a fair amount of complexity and might also be expensive.

---

_Label `needs-decision` added by @AlexWaygood on 2024-07-29 13:47_

---

_Comment by @astrowonk on 2024-07-29 16:08_

I agree it may be too complex, and/or too niche. But I don't _think_ you would need to understand the entire query "mini language" (I believe the expression gets evaluated by a pandas implementation of [eval](https://pandas.pydata.org/docs/reference/api/pandas.eval.html#pandas.eval)) but rather scan the query strings for the `@variable_name` pattern for any declared variables that otherwise seem unused. I think that would be sufficient? There's no other way to access a variable from within the query. 

I do understand the general point about this being a special case, and that it may be too expensive even if it's doable : but given pandas prevalence in the data science world, and the utility of using the  `col = @var_name` pattern in queries, I thought I'd open a discussion. Thanks for engaging!

---

_Comment by @gitriff on 2024-10-22 08:49_

For what it s worth the same happens when using duckdb and polars (or some other df library such as pandas)

```
import polars as pl
import duckdb

def main():
    df = pl.DataFrame({
        "name": ["Alice", "Bob", "Charlie"],
        "age": [25, 30, 35],
        "city": ["New York", "Los Angeles", "Chicago"]
    })

    duckdb_conn = duckdb.connect()
    query_result = duckdb_conn.execute("select * from df").fetchall()
    print(query_result)

main()
```

```
F841 Local variable `df` is assigned to but never used
```

---

_Comment by @DSGeoff on 2025-08-28 03:44_

I ran across this today when using Ruff.  I don't claim to know even a fraction of what it would take to add this, but I think within .query, the only way to reference an outside variable is via @variable_name or @`variable_name`.  Well, you can use other methods, like an f-string or .format, but Ruff would catch those already.

You'd only need to look in .query if the variable wasn't already found elsewhere.  In that case, you could find the @ and go from there. It's somewhat niche, because you can filter without .query, but also pandas is the most popular package for working with tabular data in Python.

---

_Referenced in [astral-sh/ruff#20339](../../astral-sh/ruff/issues/20339.md) on 2025-09-10 21:12_

---
