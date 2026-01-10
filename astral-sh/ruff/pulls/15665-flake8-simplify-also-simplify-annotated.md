```yaml
number: 15665
title: "[`flake8-simplify`] Also simplify annotated assignments (`SIM108`)"
type: pull_request
state: open
author: InSyncWithFoo
labels:
  - rule
  - needs-decision
assignees: []
base: main
head: SIM108
created_at: 2025-01-22T00:11:02Z
updated_at: 2025-01-22T18:59:10Z
url: https://github.com/astral-sh/ruff/pull/15665
synced_at: 2026-01-10T20:05:43Z
```

# [`flake8-simplify`] Also simplify annotated assignments (`SIM108`)

---

_Pull request opened by @InSyncWithFoo on 2025-01-22 00:11_

## Summary

Resolves #15554.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @InSyncWithFoo on 2025-01-22 00:12_

I also took the liberty to do some minor refactoring. Feel free to revert if they are undesired.

---

_Comment by @github-actions[bot] on 2025-01-22 00:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+12 -0 violations, +0 -0 fixes in 4 projects; 51 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/66b1712acc548a582267b7fc5e87149ca9cbb59e/airflow/models/taskinstance.py#L2174'>airflow/models/taskinstance.py:2174:9:</a> SIM108 Use ternary operator `map_index: int | None = None if ti.map_index < 0 else ti.map_index` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/66b1712acc548a582267b7fc5e87149ca9cbb59e/airflow/utils/timeout.py#L85'>airflow/utils/timeout.py:85:1:</a> SIM108 Use ternary operator `timeout: type[TimeoutWindows | TimeoutPosix] = TimeoutWindows if IS_WINDOWS else TimeoutPosix` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/66b1712acc548a582267b7fc5e87149ca9cbb59e/providers/src/airflow/providers/amazon/aws/hooks/logs.py#L192'>providers/src/airflow/providers/amazon/aws/hooks/logs.py:192:13:</a> SIM108 Use ternary operator `token_arg: dict[str, str] = {"nextToken": next_token} if next_token is not None else {}` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/66b1712acc548a582267b7fc5e87149ca9cbb59e/providers/src/airflow/providers/amazon/aws/triggers/ecs.py#L203'>providers/src/airflow/providers/amazon/aws/triggers/ecs.py:203:13:</a> SIM108 Use ternary operator `token_arg: dict[str, str] = {"nextToken": next_token} if next_token is not None else {}` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/66b1712acc548a582267b7fc5e87149ca9cbb59e/providers/src/airflow/providers/google/cloud/triggers/bigquery.py#L438'>providers/src/airflow/providers/google/cloud/triggers/bigquery.py:438:21:</a> SIM108 Use ternary operator `first_job_row: str | None = None if not first_records else first_records.pop(0)` instead of `if`-`else`-block
+ <a href='https://github.com/apache/airflow/blob/66b1712acc548a582267b7fc5e87149ca9cbb59e/providers/src/airflow/providers/google/cloud/triggers/bigquery.py#L445'>providers/src/airflow/providers/google/cloud/triggers/bigquery.py:445:21:</a> SIM108 Use ternary operator `second_job_row: str | None = None if not second_records else second_records.pop(0)` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/command/subcommands/file_output.py#L148'>src/bokeh/command/subcommands/file_output.py:148:9:</a> SIM108 Use ternary operator `outputs: list[str] = [] if args.output is None else list(args.output)` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pypa/cibuildwheel">pypa/cibuildwheel</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pypa/cibuildwheel/blob/3f123af8a1547a540f07255a4982ca48dc56379d/cibuildwheel/oci_container.py#L443'>cibuildwheel/oci_container.py:443:9:</a> SIM108 Use ternary operator `output_io: IO[bytes] = io.BytesIO() if capture_output else sys.stdout.buffer` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/75b6b1d54fed9196e01dc558d2fce91a19825067/zerver/actions/bots.py#L145'>zerver/actions/bots.py:145:9:</a> SIM108 Use ternary operator `stream_name: str | None = stream.name if stream else None` instead of `if`-`else`-block
+ <a href='https://github.com/zulip/zulip/blob/75b6b1d54fed9196e01dc558d2fce91a19825067/zerver/actions/bots.py#L186'>zerver/actions/bots.py:186:9:</a> SIM108 Use ternary operator `stream_name: str | None = stream.name if stream else None` instead of `if`-`else`-block
+ <a href='https://github.com/zulip/zulip/blob/75b6b1d54fed9196e01dc558d2fce91a19825067/zerver/lib/narrow.py#L339'>zerver/lib/narrow.py:339:9:</a> SIM108 Use ternary operator `maybe_negate: ConditionTransform = not_ if negated else lambda cond: cond` instead of `if`-`else`-block
+ <a href='https://github.com/zulip/zulip/blob/75b6b1d54fed9196e01dc558d2fce91a19825067/zilencer/management/commands/calculate_first_visible_message_id.py#L31'>zilencer/management/commands/calculate_first_visible_message_id.py:31:9:</a> SIM108 Use ternary operator `realms: Iterable[Realm] = Realm.objects.all() if target_realm is None else [target_realm]` instead of `if`-`else`-block
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| SIM108 | 12 | 12 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2025-01-22 07:55_

Thanks for working on this. As mentioned on the issue, I'm not sure if we should do this (as discussed in the issue) because it can lead to new or different typing errors. 

---

_Comment by @InSyncWithFoo on 2025-01-22 12:41_

I made extra care only to rewrite when the `body` branch has an annotation but the `else` branch doesn't. This is perhaps safe enough, as the pattern is well-recognized by various type checkers ([Mypy](https://mypy-play.net/?mypy=1.14.1&python=3.13&flags=strict&gist=20d84e284f7cf846212d31e11d758014), [Pyright](https://pyright-play.net/?pyrightVersion=1.1.392&pythonVersion=3.14&strict=true&enableExperimentalFeatures=true&code=JYMwBARg9lA2AUBKAXAWAFBi2AHsswAdgC5gC8YALBgKawDONam2O5YAzFgMRghE0ANGEJRSAJxoATGv0LBiwKIQxA), [Pyre](https://pyre-check.org/play?input=if%20bool()%3A%0A%20%20%20%20x%3A%20int%20%3D%204%0Aelse%3A%0A%20%20%20%20x%20%3D%203%20%20%23%20fine%2C%20not%20redefinition%0A), Pytype, perhaps even PyCharm; Red Knot too will need to support it).

---

_Comment by @Daverball on 2025-01-22 13:13_

In fact annotating both branches is considered a redefinition error (even if both annotations are the same), so this should be fine.

What I do think is worth looking into however is if we can handle type declarations that have been moved outside the two branches. I find myself doing that quite often, since it makes it more clear that the variable is shared between the two branches and that the value will be used afterwards.

So concretely the following:
```python
x: int
if foo:
    x = 5
else:
    x = 0
```

becomes:

```python
x: int = 5 if foo else 0
```

rather than

```python
x: int
x = 5 if foo else 0
```

---

_Comment by @InSyncWithFoo on 2025-01-22 13:23_

> ```python
> x: int
> x = 5 if foo else 0
> ```

This should probably be handled by another rule, similar to the [`UP031`](https://docs.astral.sh/ruff/rules/printf-string-formatting/)/[`UP032`](https://docs.astral.sh/ruff/rules/f-string/) and [`FURB140`](https://docs.astral.sh/ruff/rules/reimplemented-starmap/)/[`RUF058`](https://github.com/astral-sh/ruff/pull/15483) pairs. Such a rule doesn't exist yet, however.

---

_Comment by @MichaReiser on 2025-01-22 13:42_

> In fact annotating both branches is considered a redefinition error (even if both annotations are the same), so this should be fine.

I'd have to test if that's true for all existing type checkers or is this defined anywhere in the typing spec? 

I'm still leaning towards leaving it as is because I currently can't prioritize doing the necessary research to see if this is safe for all type checker and while nice, it doesn't seem urgent. I can try to find time to review this change if someone invests the necessary time and documents the PR accordingly (where does it provide fixes, why, and why is it safe in those circumstnaces)

---

_Comment by @InSyncWithFoo on 2025-01-22 14:15_

It is rather hard to explain why the fix is safe (it is actually always marked as unsafe). Here's how I would explain it for someone who has basic knowledge of static typing in Python:

> This pattern is recognized by major type checkers, which won't consider the second assignment as a redeclaration. Simplifying the `if` block to a single assignment thus won't cause a type checking error.
> 
> On the other hand, if both assignments have annotations or only the second have, some type checkers might report an error:
> 
> (playgrounds: [Mypy](https://mypy-play.net/?mypy=1.14.1&python=3.13&flags=strict&gist=86cca6bd50ca7f800334d786609ddc60), [Pyright](https://pyright-play.net/?pyrightVersion=1.1.392&pythonVersion=3.14&strict=true&enableExperimentalFeatures=true&code=CYUwZgBA%2BgFGA2BDA5gLggIwPZfgSggFoA%2BCAOSwDsRUBYAKAiYgEtIEU7HmfF0XKAFwgBeCAEYGPEPADONKT2aJREAExKmAYggBbAJ4AHfQBoIxgE4tkAC0FnLIB-sFGQo0mAFOIlLMIsQUABjJAtEQRYqBhjuJjYIDjRFTQx%2BIVVJOIgZeS5NJjTWDLENCB1LaztnQOdXQ3dNEU9vMz8AoJBQxHDI6OyCwfK9I30h8aVmnIsLLAszQJCwiKjKWJTWdiRkgeZgzI3chV2mYPThUu1zfStbe2va6-rGyZbqNv8IJZ6V-omhnQGYz-f5TEAzOZmb69VYMIA), [Pyre](https://pyre-check.org/play?input=%23%20pyre-strict%0A%0Adef%20_(flag%3A%20bool)%20-%3E%20None%3A%0A%20%20%20%20if%20flag%3A%0A%20%20%20%20%20%20%20%20a%3A%20int%20%3D%201%0A%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20a%20%3D%202%20%20%20%20%20%20%20%23%20mypy%2C%20pyright%2C%20pyre%2C%20pytype%20%3D%3E%20fine%2C%20not%20redeclaration%0A%0A%0A%20%20%20%20if%20flag%3A%0A%20%20%20%20%20%20%20%20b%3A%20int%20%3D%201%0A%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20b%3A%20int%20%3D%202%20%20%23%20pyright%2C%20pyre%2C%20pytype%20%20%20%20%20%20%20%3D%3E%20fine%2C%20not%20redeclaration%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%23%20mypy%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3D%3E%20error%2C%20redeclaration%0A%0A%0A%20%20%20%20if%20flag%3A%0A%20%20%20%20%20%20%20%20c%20%3D%201%0A%20%20%20%20else%3A%0A%20%20%20%20%20%20%20%20c%3A%20int%20%3D%202%20%20%23%20pyright%2C%20pyre%2C%20pytype%20%20%20%20%20%20%20%3D%3E%20fine%2C%20not%20declaration%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%23%20mypy%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%3D%3E%20error%2C%20declaration%0A))
> 
> ```python
> if flag:
>     a: int = 1
> else:
>     a = 2       # mypy, pyright, pyre, pytype => fine, not redeclaration
> 
> if flag:
>     b: int = 1
> else:
>     b: int = 2  # pyright, pyre, pytype       => fine, not redeclaration
>                 # mypy                        => error, redeclaration
> 
> if flag:
>     c = 1
> else:
>     c: int = 2  # pyright, pyre, pytype       => fine, not declaration
>                 # mypy                        => error, declaration
> ```

Is that easy to understand?

---

_Comment by @Daverball on 2025-01-22 14:33_

I think the other thing @MichaReiser is worried about is inference behavior if one of the branches doesn't match the annotated type of the other branch.

Unfortunately it seems like pytype is actually extremely forgiving here, so the following would have a different output for pytype:

```python
if flag:
    a: int = 1
else:
    a = '2'
reveal_type(a)  # Union[int, str]
```

than the reformatted version:

```python
a: int = 1 if flag else '2'  # error: annotation-type-mismatch
reveal_type(a)  # int
```

But other than that I don't think there's any type checker that would pass the above example. Even pyre disallows it, but it allows you to have different annotations for each branch and then infers the union outside the branch, just like pytype.

---

_Comment by @InSyncWithFoo on 2025-01-22 14:49_

> Unfortunately it seems like pytype is actually extremely forgiving here [...]

I think this is not too big a problem, considering that the fix is marked as unsafe, but, yes, it is nevertheless a major drawback of the PR.

---

_Label `rule` added by @MichaReiser on 2025-01-22 14:50_

---

_Label `needs-decision` added by @MichaReiser on 2025-01-22 14:50_

---

_Comment by @MichaReiser on 2025-01-22 14:59_

I think this airflow example is interesting because it changes the declared types:

https://github.com/apache/airflow/blob/66b1712acc548a582267b7fc5e87149ca9cbb59e/airflow/utils/timeout.py#L85-L88



---

_Comment by @InSyncWithFoo on 2025-01-22 15:13_

@MichaReiser I'm not sure if I follow. That block would be simplified to:

```python
timeout: type[TimeoutWindows | TimeoutPosix] = TimeoutWindows if IS_WINDOWS else TimeoutPosix
```

...which is perfectly fine type-checking-wise as far as I can tell.

---

_Comment by @MichaReiser on 2025-01-22 16:04_

It's fine for `Windows` but it results in a wider type for unix platforms where the type was simply:

```py
timeout: type[TimeoutPosix] = TimeoutPosix
```

at least if the type checker understands the platform-specific branches. 

---

_Comment by @InSyncWithFoo on 2025-01-22 18:59_

@MichaReiser Normally [`sys.platform`](https://docs.python.org/3/library/sys.html#sys.platform) is used for that purpose:

```python
import sys

if sys.platform == 'win32':
	timeout = TimeoutWindows
else:
	timeout = TimeoutPosix
```

`SIM108` does indeed [recognize this as well as `TYPE_CHECKING`](https://github.com/astral-sh/ruff/pull/15665/files#diff-1a05984f59acaff0d9be64de27dbab3ab0bbd83a491063ddb3dee222d4058c23R125-R133). As for custom variables, that should be the topic of another issue/PR.

---
