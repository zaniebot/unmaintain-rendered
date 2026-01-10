```yaml
number: 14553
title: "[`ruff`] `if k in d: del d[k]` (`RUF051`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF041
created_at: 2024-11-23T04:29:43Z
updated_at: 2024-12-11T12:54:32Z
url: https://github.com/astral-sh/ruff/pull/14553
synced_at: 2026-01-10T20:42:26Z
```

# [`ruff`] `if k in d: del d[k]` (`RUF051`)

---

_Pull request opened by @InSyncWithFoo on 2024-11-23 04:29_

## Summary

Resolves #7537.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-23 04:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+16 -0 violations, +0 -0 fixes in 6 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/DisnakeDev/disnake">DisnakeDev/disnake</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/cog.py#L177'>disnake/ext/commands/cog.py:177:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/cog.py#L179'>disnake/ext/commands/cog.py:179:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/DisnakeDev/disnake/blob/2dab45c84f4454ccf2c2a2efd7e819e06f1551c0/disnake/ext/commands/cog.py#L181'>disnake/ext/commands/cog.py:181:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+2 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/core/migrate.py#L134'>rasa/core/migrate.py:134:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/nlu/classifiers/diet_classifier.py#L781'>rasa/nlu/classifiers/diet_classifier.py:781:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/f0ec10e2f5283207c220ff2e21065e0c01454f6b/docs/build_docs.py#L604'>docs/build_docs.py:604:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/apache/airflow/blob/f0ec10e2f5283207c220ff2e21065e0c01454f6b/docs/build_docs.py#L606'>docs/build_docs.py:606:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/apache/airflow/blob/f0ec10e2f5283207c220ff2e21065e0c01454f6b/providers/src/airflow/providers/google/cloud/operators/dataproc.py#L666'>providers/src/airflow/providers/google/cloud/operators/dataproc.py:666:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/apache/airflow/blob/f0ec10e2f5283207c220ff2e21065e0c01454f6b/providers/src/airflow/providers/google/cloud/operators/dataproc.py#L680'>providers/src/airflow/providers/google/cloud/operators/dataproc.py:680:21:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/superset/blob/d8fbaa4cbe736e827dd777dbb98f2cf04dd1431d/superset/dashboards/schemas.py#L185'>superset/dashboards/schemas.py:185:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/corporate/tests/test_stripe.py#L662'>corporate/tests/test_stripe.py:662:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/lib/markdown/__init__.py#L2456'>zerver/lib/markdown/__init__.py:2456:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/lib/test_runner.py#L103'>zerver/lib/test_runner.py:103:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/tornado/event_queue.py#L542'>zerver/tornado/event_queue.py:542:13:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
+ <a href='https://github.com/zulip/zulip/blob/38ad1e8bdc2eaf59e7393e61b470519d8c03a12b/zerver/tornado/handlers.py#L42'>zerver/tornado/handlers.py:42:9:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/82e9f41ceb1006697ccc9e43224ecb2ace139cd2/astropy/modeling/core.py#L189'>astropy/modeling/core.py:189:17:</a> RUF051 [*] Use `pop` instead of `key in dict` followed by `delete dict[key]`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF051 | 16 | 16 | 0 | 0 | 0 |

</p>
</details>




---

_@dylwil3 reviewed on 2024-11-24 22:51_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:21 on 2024-11-24 22:51_

I think this gives the impression that `pop` is more performant, but that's not necessarily true. Here's a test stolen from [this stackoverflow comment](https://stackoverflow.com/questions/5713218/what-is-difference-between-pop-vs-del-for-deleting-a-dictionary-item-python#comment91857000_5713303) but run on Python 3.12:

```console
Python 3.12.7 (main, Oct 16 2024, 07:12:08) [Clang 18.1.8 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> import timeit
>>> timeit.timeit(stmt="if 'two' in a: del a['two']", setup="a={'one': 1, 'two': 2, 'three': 3}", number=1000000)
0.023885874994448386
>>> timeit.timeit(stmt="a.pop('two', None)", setup="a={'one': 1, 'two': 2, 'three': 3}", number=1000000)
0.034923375002108514
```
So I think this is more of a style/readability lint than a performance lint (but I still like the lint!)

---

_@InSyncWithFoo reviewed on 2024-11-25 06:34_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:21 on 2024-11-25 06:34_

Well, `LOAD_ATTR` is typically slow, calling a function is expensive, and I'm terrible at writing descriptions. What do you suggest?

---

_Label `rule` added by @MichaReiser on 2024-11-25 08:08_

---

_Label `preview` added by @MichaReiser on 2024-11-25 08:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:21 on 2024-11-25 08:14_

I agree that this rule primarily suggests simplifying (reducing complexity). It's less about the performance itself (although that falls out of it). 

That's why I would also write the motivation this way. I'm also not good at writing descriptions. That's why I often search for similar rules to get inspired (copy paste :P). In this case, the flake8-simplify rules are a good place to start https://docs.astral.sh/ruff/rules/#flake8-simplify-sim

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:41 on 2024-11-25 08:15_

This message isn't very descriptive. How about *Use `pop` instead of `x in dict` followed by `delete dict[x]`*

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:71 on 2024-11-25 08:17_

we should mark the fix as unsafe if either the `if` or `delete` statement contains any comments

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:117 on 2024-11-25 08:19_

What's the motivation for restricting the dictionary to name expressions only? Is it for the fix safety or is it to avoid false-positives?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:135 on 2024-11-25 08:22_

Could we use `ComparableExpr` here, or are there cases where this rule needs to be stricter than `CompareExpr`? 

---

_@MichaReiser reviewed on 2024-11-25 08:24_

Thanks.

This rule would fit well into the flake8-simplify group. Would you be interested in creating an upstream issue to see if they're interested in such a rule? 

---

_Label `needs-decision` added by @MichaReiser on 2024-11-25 08:24_

---

_@InSyncWithFoo reviewed on 2024-11-25 08:57_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:117 on 2024-11-25 08:57_

`is_known_to_be_of_type_dict()` only accepts name expressions. I think I copied this from [`SIM118`](https://github.com/astral-sh/ruff/blob/9f6147490b68a9e6373ebec35293f96274bc8681/crates/ruff_linter/src/rules/flake8_simplify/rules/key_in_dict.rs#L126-L135).

---

_Comment by @InSyncWithFoo on 2024-11-25 09:22_

Created upstream issue [here](https://github.com/MartinThoma/flake8-simplify/issues/195). The repo has been inactive for nearly a year, so I won't hold my breath.

---

_@InSyncWithFoo reviewed on 2024-11-25 10:01_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:71 on 2024-11-25 10:01_

I'm thinking about using `locator.slice()`, but surely there must be a better way to do this?

---

_@MichaReiser reviewed on 2024-11-25 10:11_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:71 on 2024-11-25 10:11_

There is :) 

https://github.com/astral-sh/ruff/blob/e25e7044ba365d6a6b9f3088a1ad0545bcbd7ae0/crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_cast_value.rs#L59-L61

---

_@InSyncWithFoo reviewed on 2024-11-25 11:57_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:135 on 2024-11-25 11:57_

`ComparableExpr` does have a much wider range than as desired for this rule. I think I could add `Expr::Attribute` to the bunch, but that would further complicate the fix and require it to be marked as unsafe (non-pure property and whatnot).

---

_Comment by @MichaReiser on 2024-11-27 09:21_

Thanks for opening the upstream issue. 

We discussed this internally. Let's give `flake8-simplify` about a week to get back to the issue. If we don't hear back in a week, then let's implement this rule as part of ruf (as you did).

@InSyncWithFoo feel free to ping me after a week 

---

_Renamed from "[`ruff`] `if k in d: del d[k]` (`RUF041`)" to "[`ruff`] `if k in d: del d[k]` (`RUF051`)" by @InSyncWithFoo on 2024-12-04 16:01_

---

_Comment by @InSyncWithFoo on 2024-12-04 16:10_

@MichaReiser It has been a week. What do you say?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:21 on 2024-12-10 09:25_

I don't fully understand this part of the documentation? What are you trying to point out here?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:72 on 2024-12-10 09:28_

I suggest that you move the 

```
    let [Stmt::Delete(StmtDelete { targets, .. })] = body else {
        return None;
    };
```

out from `extract_dict_and_key_from_del` so that you can pass the delete statement to `replace_with_dict_pop_fix`, removing the need for this comment

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:110 on 2024-12-10 09:29_

Nit: The `verified` suffix isn't telling me much and you also don't use it for all other `extract` methods. 

I'd suggest renaming it to `extract_dict_and_key` to align the naming with your other extract methods

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:110 on 2024-12-10 09:35_

All our ast nodes have the same lifetime, it shouldn't be necessary to use two lifetimes here
```suggestion
fn dict_and_key_verified<'ast>(dict: &'ast Dict, key: &'ast Key) -> Option<(&'ast Dict, &'ast Key)> {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:110 on 2024-12-10 09:35_

The use of the `Dict` and `Key` types for the arguments is confusing here because the main purpose of this function is to assert that `dict` and `key` are of the right type. I suggest using `&Expr` here

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:131 on 2024-12-10 09:36_

Using `ComparableExpr` here seems overkill. Two `NoneLiteral`, `EllipsisLiteral` are always equal. Implementing equality for `BooleanLiteral` also only requires comparing the value, same for bytes and string literals

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:138 on 2024-12-10 09:39_

This should use the `Dict` type
```suggestion
fn is_same_dict(test: &Dict, del: &Dict) -> bool {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:148 on 2024-12-10 09:39_

Same
```suggestion
fn is_known_to_be_of_type_dict(semantic: &SemanticModel, dict: &Dict) -> bool {
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:15 on 2024-12-10 09:40_

Using `ExprName` as `Dict` type greatly simplifies the implementation
```suggestion
type Key = Expr;
type Dict = ExprName;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:8 on 2024-12-10 09:41_

I'd suggest to create an enum for the `Key`. It makes it easier to reason about the rule and gives you a good place to implement some operations (e.g. implement `Eq` manually)

```
enum Key<'ast> {
	Name(&'ast ExprName),
	None,
	Ellipsis, 
	StringLiteral(&'ast StringLiteral),
	BytesLiteral(&'ast BytesLiteral)
}
```

---

_@MichaReiser reviewed on 2024-12-10 09:44_

Thanks. I left a few comments related to the code. 

The main question to me is if we want to add this rule to Ruff today. I see that @charliermarsh mark the rule as accepted. 

To me, this seems to be mainly a stylistic rule and I personally would prefer `if x in k: del k[x]` in many places. But maybe that's just because I'm unused to a `dict` type to not have a `remove` method. I'd be on board merging this rule if we consider using `pop` is a more idiomatic writting of this pattern. @AlexWaygood what's your take on this?

---

_Comment by @AlexWaygood on 2024-12-10 13:25_

> To me, this seems to be mainly a stylistic rule and I personally would prefer `if x in k: del k[x]` in many places. But maybe that's just because I'm unused to a `dict` type to not have a `remove` method. I'd be on board merging this rule if we consider using `pop` is a more idiomatic writting of this pattern. @AlexWaygood what's your take on this?

I agree that using `.pop(..., None)` is the idiomatic way to do this in Python. The rule makes sense to me. While I agree that it reads a little bit awkwardly if you're new to the language, it's much more efficient than `if x in k: del k[x]`, and it's more concise. I feel like pointing out better idioms in a certain language that may not exist in other languages is one of the most helpful things linters can do for beginners in that language. So I support the rule.

---

_@MichaReiser requested changes on 2024-12-10 13:30_

Perfect, thanks @AlexWaygood for the extra context (and educating me :))

I'm requesting changes for the code review feedback that I submitted earlier today

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:131 on 2024-12-10 20:06_

I'm pretty sure [it was you](https://github.com/astral-sh/ruff/pull/14553#discussion_r1856081417) who suggested using `ComparableExpr` instead of manual comparison... Anyway, reverted.

---

_@InSyncWithFoo reviewed on 2024-12-10 20:06_

---

_@InSyncWithFoo reviewed on 2024-12-10 20:19_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:21 on 2024-12-10 20:19_

Basically "Prefer EAFP over LBYL". If the end results are the same (no such key in the dictionary after the operation), then there is no need to check if the key even exists in the first place.

---

_@InSyncWithFoo reviewed on 2024-12-10 22:13_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:8 on 2024-12-10 22:13_

That seems a tad too bloated, considering I also need the ranges for comment checking. The current `match` should be enough.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/if_key_in_dict_del.rs`:8 on 2024-12-11 10:29_

You can also store all nodes if you need the ranges

---

_@MichaReiser reviewed on 2024-12-11 10:29_

---

_Label `needs-decision` removed by @MichaReiser on 2024-12-11 11:08_

---

_Merged by @MichaReiser on 2024-12-11 11:12_

---

_Closed by @MichaReiser on 2024-12-11 11:12_

---

_Branch deleted on 2024-12-11 12:54_

---
