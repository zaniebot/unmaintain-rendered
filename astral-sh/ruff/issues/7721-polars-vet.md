```yaml
number: 7721
title: polars vet?
type: issue
state: open
author: MarcoGorelli
labels:
  - plugin
assignees: []
created_at: 2023-09-30T12:21:35Z
updated_at: 2025-10-07T11:04:26Z
url: https://github.com/astral-sh/ruff/issues/7721
synced_at: 2026-01-10T11:09:50Z
```

# polars vet?

---

_Issue opened by @MarcoGorelli on 2023-09-30 12:21_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hello,

I've noticed that `ruff` has a `pandas-vet` plugin. Would you open to adding a Polars-vet one?

It could make suggestions such as
```diff
- pl.read_csv(file_name).lazy()
+ pl.scan_csv(file_name)
```
or
```diff
- df.select(pl.col('a').map_elements(lambda lst: ' '.join([str(x) for x in lst])))
+ df.select(pl.col('a').list.join(' '))
```
which can have a real impact on performance

I could try putting something together if you'd be open to it

---

_Comment by @charliermarsh on 2023-09-30 17:13_

I'm happy to add something like this! I'd prefer if we had a non-trivial set of rules in mind (e.g., at least five or so?) before we started to add them. I want to avoid a situation in which we create a new category, add one rule, then fail to expand it to a meaningful set.


---

_Label `plugin` added by @charliermarsh on 2023-09-30 17:13_

---

_Comment by @MarcoGorelli on 2023-09-30 18:08_

Sure, thanks! For a start, there's all the rewrites from https://github.com/pola-rs/polars/issues/9968, such as
```diff
- pl.col('a').map_elements(lambda x: np.sin(x))
+ pl.col('a').sin()

- pl.col('a').map_elements(lambda x: x+1)
+ (pl.col('a') + 1)

- pl.col('a').map_elements(lambda x: json.loads(x))
+ pl.col("a").str.json_extract()

- pl.col('a').map_elements(lambda x: dt.datetime.strptime(x, "%Y-%m-%d"))
+ pl.col('a').str.to_datetime(format='%Y-%m-%d')

- pl.col('a').map_elements(lambda x: x.upper())
+ pl.col("a").str.to_uppercase()
```

. Within Polars, warnings are emitted for some of these by parsing the bytecode of the passed function - but as Ruff deals with the AST, then I'd expect it to be possible to cover a lot more from that list

The full list of test cases is here, there's quite a few already:

https://github.com/pola-rs/polars/blob/f3142ccd321873d7be5b339fd6ec5536e8c3153e/py-polars/tests/test_udfs.py#L28-L144

---

_Comment by @ritchie46 on 2023-09-30 18:35_

Any read operation followed by a lazy is very fishy.

E.g. `pl.read_parquet(..).lazy()` should suggest `pl.scan_parquet(..)`.

And that for all our scan supported file types.

---

_Comment by @stinodego on 2023-10-19 11:05_

One more suggestion in the 'lazy' category:

```diff
- DataFrame(...).lazy()
+ LazyFrame(...)
```

Maybe one for assertions (the equality statements would result in an error):

```diff
- assert s1 == s2
+ assert_series_equal(s1, s2)

- assert df1 == df2
+ assert_frame_equal(df1, df2)

- assert lf1 == lf2
+ assert_frame_equal(lf1, lf2)

- assert s1 != s2
+ assert_series_not_equal(s1, s2)
...

```

One for `select`/`with_columns`:

```diff
- df.select(pl.all(), ...)
+ df.with_columns(...)

- df.select(pl.col("*"), ...)
+ df.with_columns(...)
```

Keyword syntax in `select`/`with_columns`:

```diff
- df.select(pl.col('a').abs().alias('abs'))
+ df.select(abs=pl.col('a').abs())
```

Keyword syntax in `filter`:

```diff
- df.filter(pl.col('a') == 'foo')
+ df.filter(a='foo')
```

Using positional args instead of lists where possible:

```diff
- df.sort(['a', 'b'])
+ df.sort('a', 'b')
```

...I'm sure I can come up with more :smile: 
@MarcoGorelli Is this enough input?

---

_Comment by @MRigal on 2025-10-07 11:04_

In 2025, with increasing polars adoption, this would still be very cool!

---
