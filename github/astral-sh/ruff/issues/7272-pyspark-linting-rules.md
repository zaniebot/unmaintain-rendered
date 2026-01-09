---
number: 7272
title: Pyspark Linting Rules
type: issue
state: open
author: sbrugman
labels:
  - plugin
assignees: []
created_at: 2023-09-11T16:32:18Z
updated_at: 2025-07-15T07:26:19Z
url: https://github.com/astral-sh/ruff/issues/7272
synced_at: 2026-01-07T13:12:15-06:00
---

# Pyspark Linting Rules

---

_Issue opened by @sbrugman on 2023-09-11 16:32_

Apache Spark is widely used in the python ecosystem for distributed computing. As user of spark I would like for ruff to lint problematic behaviours. The automation that ruff offers is especially useful in projects with various levels of software engineering skills, e.g. where people has more of a statistics background.

There exists a [pyspark style guide](https://github.com/AstrumU/pylint-pyspark) and [pylint extension](https://pypi.org/project/pylint-pyspark/).

I would like to start contributing a rule that checks for repeated use of [withColumn](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.withColumn.html):
> This method introduces a projection internally. Therefore, calling it multiple times, for instance, via loops in order to add multiple columns can generate big plans which can cause performance issues and even StackOverflowException. To avoid this, use [select()](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql/api/pyspark.sql.DataFrame.select.html#pyspark.sql.DataFrame.select) with multiple columns at once.

Are you ok with a PR introducing "Spark-specific rules" (e.g. `SPK`)? 

`ruff` includes rules that are specific to third party libraries: numpy, pandas and airflow. Spark support would be a nice addition. 

I would like to close with the following thought: supporting third-party packages may at first seem to be effort in the long tail of possible rules to add to `ruff`. Why not focus only on rules that affect all Python users? I hope that adding these will lead to creating helper functions that make adding new rules easier. I also think that these libraries will end up with similar API design patterns, that can be linted across the ecosystem. As an example, [call chaining](https://github.com/dosisod/refurb/issues/286) is common for many packages that perform transformations. 


---

_Comment by @charliermarsh on 2023-09-12 01:10_

I'm generally open to adding package-specific rule sets for extremely popular packages (as with Pandas, NumPy, etc.), and Spark would fit that description. However, it'd be nice to have a few rules lined up before we move forward and add any one of them. Otherwise, we run the risk that we end up with really sparse categories that only contain a rule or two.

---

_Label `plugin` added by @charliermarsh on 2023-09-12 01:10_

---

_Comment by @sbrugman on 2023-09-12 06:25_

Super. I've updated the issue with a couple of rules that we can track.

---

_Comment by @guilhem-dvr on 2024-02-09 09:30_

Hi, I was looking for such a thread.

To add to the proposed list, here are some rules we wish we had at my company:

- unnecessary `drop` followed by a `select`
- use `unionByName` instead of `union` / `unionAll`
- use `df.writeTo(...).append()` instead of `df.write.insertInto(...)`
- use `df.writeTo(...).overwritePartitions()` instead of `df.write.insertInto(..., overwrite=True)`
- replace `udf` with native spark functions
- alias `pyspark.sql.functions` to `F` -> `from pyspark.sql import ..., functions as F, ...`

---

_Comment by @amadeuspzs on 2024-02-28 15:34_

Just to add that I would be interested in this functionality.

Also, the first link in the original post is broken, and the pylint extension looks unmaintained?

* https://github.com/palantir/pyspark-style-guide is a written PySpark guide, and contains some pylint implementations under https://github.com/palantir/pyspark-style-guide/tree/develop/src/checkers
* https://nhsdigital.github.io/rap-community-of-practice/training_resources/pyspark/pyspark-style-guide/ is based off the guide above

---

_Comment by @stkrzysiak on 2024-06-25 16:55_

> Super. I've updated the issue with a couple of rules that we can track. I'll kick off with SPK001-3

Did you get going with this? Thinking about jumping on it

---

_Referenced in [astral-sh/ruff#12349](../../astral-sh/ruff/issues/12349.md) on 2024-07-16 16:42_

---

_Comment by @Rexeh on 2024-08-21 13:41_

Currently looking at recommending Ruff for data team and this feature would be great.

---

_Referenced in [astral-sh/ruff#13038](../../astral-sh/ruff/issues/13038.md) on 2024-08-23 05:33_

---

_Comment by @aran159 on 2024-09-06 05:22_

> > Super. I've updated the issue with a couple of rules that we can track. I'll kick off with SPK001-3
> 
> 
> 
> Did you get going with this? Thinking about jumping on it

Hey! Did you end up jumping on it? We're also considering starting it, so I'd love to hear how it went for you!

---

_Comment by @sbrugman on 2024-09-06 09:15_

I'm working on getting a first set of rules out there :)

---

_Comment by @sbrugman on 2024-09-06 12:45_

@guilhem-dvr Thanks for your suggestions!
Would you have an example for the following?

> unnecessary drop followed by a select

Also note that the import convention is already possible via:
https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_aliases

---

_Comment by @guilhem-dvr on 2024-09-09 08:35_

Sure, here's what I had in mind:
```python
df = spark.createDataFrame(
    [("John", 25, "Engineer"), ("Jane", 30, "Doctor"), ("Jim", 35, "Teacher")],
    ["Name", "Age", "Profession"],
)

# Uncessary drop
df.drop("Age").select("Name", "Profession")

# Same statement without the drop
df.select("Name", "Profession")
```

But now I see that there's a pattern that shouldn't be flagged: where an 'anti' select is performed, i.e. drop some columns then select all the remaining ones with select("*"):

```python
# Reusing the previous df schema
df.drop("Age").select("*")
```

Edit: this is still a bad pattern because drop already returns the whole dataframe - minus the dropped column - so you should never be chaining drop and select anyway.

> Also note that the import convention is already possible via: https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_aliases

Lol, I had completely skimmed over the settings, thank you!


---

_Comment by @sbrugman on 2024-09-09 08:45_

Thanks for clarifying! There is not as much Spark open-source code available as there is for other libraries, so it's hard to tell how frequent this error is - but a good addition nonetheless. Funny enough, Polars uses this in their tests: https://github.com/pola-rs/polars/blob/main/py-polars/tests/unit/lazyframe/test_lazyframe.py#L1090

---

_Comment by @stkrzysiak on 2024-09-09 10:55_

> > > Super. I've updated the issue with a couple of rules that we can track. I'll kick off with SPK001-3
> > 
> > 
> > Did you get going with this? Thinking about jumping on it
> 
> Hey! Did you end up jumping on it? We're also considering starting it, so I'd love to hear how it went for you!

No, but looks like @sbrugman has, which is exciting!

---

_Comment by @montanarograziano on 2024-11-26 21:05_

I'd love to contribute to this one!
For Pyspark style guide references, I'll suggest you considering also [this one](https://github.com/mrpowers-io/spark-style-guide/blob/main/PYSPARK_STYLE_GUIDE.md) from @mrpowers

---

_Comment by @sbrugman on 2024-11-27 18:17_

@montanarograziano do you have specific rules in mind from that style guide?

---

_Comment by @montanarograziano on 2024-11-28 10:56_

> [@montanarograziano](https://github.com/montanarograziano) do you have specific rules in mind from that style guide?

Apart from those already mentioned, I was thinking on something related to naming conventions. Here's what the guide suggests:
- Variables pointing to DataFrames should be suffixed with `df`
- Variables pointing to RDDs should be suffixed with `rdd`
- `with` precedes transformations that add columns:
- `filter` precedes transformations that remove rows
- `explode` precedes transformations that add rows to a DataFrame by "exploding" a row into multiple rows.

---

_Comment by @sbrugman on 2024-11-28 11:15_

Thanks. These are good candidates to become linting rules, however would need type information to determine if a variable is a DataFrame, RDD or transformation reliably. The Astral team is working on that, but it's not available yet.

---

_Comment by @lauragalera on 2024-12-10 15:30_

I am also used to `glueContext = GlueContext(SparkContext.getOrCreate())` and  `from pyspark.sql import functions as F` but I had to ignore N816 and N812 from [pep8-naming](https://pypi.org/project/pep8-naming/) because they gave errors in every pull request. 

---

_Comment by @magdanowak on 2025-03-21 07:54_

`pyspark.sql.functions` is also aliased as `sf` in Apache Spark docs, so enforcing aliasing it as uppercase `F` doesn't seem justified. 

See here:
https://spark.apache.org/docs/latest/api/python/_modules/pyspark/sql/functions.html

---

_Comment by @laurence-kuhlburger-taod on 2025-05-06 10:50_

Hi there, I'd be super interested in using the linting set here and was wondering what the status? Are there already linting pyspark rules on a release version? :) Thanks a ton!

---

_Comment by @MarthinusBosman on 2025-07-15 07:26_

I'd love to be able to have a warning/info rule for spark actions just to easily spot them in a codebase. Perhaps even a specific rule for when they're used only for a log statement.

---
