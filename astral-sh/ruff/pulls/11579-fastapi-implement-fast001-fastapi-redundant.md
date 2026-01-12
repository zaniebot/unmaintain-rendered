```yaml
number: 11579
title: "[`fastapi`] Implement `FAST001` (`fastapi-redundant-response-model`) and `FAST002` (`fastapi-non-annotated-dependency`)"
type: pull_request
state: merged
author: TomerBin
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fastapi-redundant-response-model
created_at: 2024-05-28T07:20:50Z
updated_at: 2024-07-30T04:35:48Z
url: https://github.com/astral-sh/ruff/pull/11579
synced_at: 2026-01-12T15:55:38Z
```

# [`fastapi`] Implement `FAST001` (`fastapi-redundant-response-model`) and `FAST002` (`fastapi-non-annotated-dependency`)

---

_@TomerBin_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Implements ruff specific role for fastapi routes, and its autofix.



## Test Plan

`cargo test` / `cargo insta review`


---

_Comment by @github-actions[bot] on 2024-05-28 07:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+3 -0 violations, +0 -0 fixes in 1 projects; 49 projects unchanged)

<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+3 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/2db5a83f4ed5dd21d99123a0947238f0674270c0/lnbits/core/views/api.py#L66'>lnbits/core/views/api.py:66:37:</a> RUF102 FastAPI route with redundant response_model argument
+ <a href='https://github.com/lnbits/lnbits/blob/2db5a83f4ed5dd21d99123a0947238f0674270c0/lnbits/core/views/node_api.py#L79'>lnbits/core/views/node_api.py:79:34:</a> RUF102 FastAPI route with redundant response_model argument
+ <a href='https://github.com/lnbits/lnbits/blob/2db5a83f4ed5dd21d99123a0947238f0674270c0/lnbits/core/views/wallet_api.py#L72'>lnbits/core/views/wallet_api.py:72:25:</a> RUF102 FastAPI route with redundant response_model argument
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF102 | 3 | 3 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @codspeed-hq[bot] on 2024-05-28 07:40_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/TomerBin:fastapi-redundant-response-model)

### Merging #11579 will **not alter performance**

<sub>Comparing <code>TomerBin:fastapi-redundant-response-model</code> (dd50d3b) with <code>TomerBin:fastapi-redundant-response-model</code> (13df570)</sub>



### Summary

`✅ 33` untouched benchmarks






---

_Comment by @charliermarsh on 2024-06-04 04:00_

Thank you for this! Two reactions, which I'd in-turn like your reaction on:

1. I think we'll need to find someone who feels comfortable reviewing these rules for correctness and quality (e.g., is this a recommended pattern?). I'm not familiar enough with FastAPI to be a good judge here. Perhaps I can ask Sebastian what he thinks of it.
2. Ideally we'd implement multiple FastAPI rules before committing to supporting a one-off rule. This helps prevent Ruff from feeling like an unwieldy collection of one-off rules.


---

_Comment by @TomerBin on 2024-06-15 11:33_

@charliermarsh Thanks for your comments.

1. It would be great if Sebastian could confirm that rule. Could you please ask for his opinion?
2. Another rule I'm considering is to use the [new Annotated style](https://fastapi.tiangolo.com/tutorial/dependencies/#__tabbed_3_4) for dependencies declaration. Other rules I have in mind require complicated type inference and cross-file analysis, so they are probably out of scope for this PR. Do you think multiple rules are mandatory for merging this PR?

---

_Comment by @TomerBin on 2024-07-06 16:48_

@charliermarsh Any chance you could provide feedback on my previous message when you have a moment? 

---

_Assigned to @charliermarsh by @MichaReiser on 2024-07-10 07:53_

---

_Comment by @tiangolo on 2024-07-15 23:56_

Thanks @TomerBin for emailing me! This looks great.

And recommending `Annotated` would be awesome as well.

Additionally, if there was a way to auto-fix code that uses the old style to now use `Annotated` in Ruff, that would be a dream come true. It applies to dependencies or to `Security` as well (another type of dependency.

An old dependency and `Security` param would look like:

```Python
@app.get("/items/")
def get_items(
    current_user: User = Depends(get_current_user),
    some_security_param: str = Security(get_oauth2_user),
):
    # do stuff
    pass
```

That would be ideally transformed into:

```Python
@app.get("/items/")
def get_items(
    current_user: Annotated[User, Depends(get_current_user)],
    some_security_param: Annotated[str, Security(get_oauth2_user)],
):
    # do stuff
    pass
```

Additionally, the same transformation would apply to other types of parameters, like `Query`, `Path`, `Body`, `Cookie`.

The only caveat is that if one of those has a `default` argument (or the first argument), as in `Query(default="foo")` then that would become the new default value. For example:

```Python
@app.post("/stuff/")
def do_stuff(
    some_query_param: str | None = Query(default=None),
    some_path_param: str = Path(),
    some_body_param: str = Body("foo"),
    some_cookie_param: str = Cookie(),
    some_header_param: int = Header(default=5),
    some_file_param: UploadFile = File(),
    some_form_param: str = Form(),
):
    # do stuff
    pass
```

would ideally be transformed into:

```Python
@app.post("/stuff/")
def do_stuff(
    some_query_param: Annotated[str | None, Query()] = None,
    some_path_param: Annotated[str, Path()],
    some_body_param: Annotated[str, Body()] = "foo",
    some_cookie_param: Annotated[str, Cookie()],
    some_header_param: Annotated[int, Header()] = 5,
    some_file_param: Annotated[UploadFile, File()],
    some_form_param: Annotated[str, Form()],
):
    # do stuff
    pass
```

---

**Note**: please keep pinging me on email if I miss a GitHub notification around this, I miss most GitHub notifications as I still have around 10k unread. :sweat_smile: 

---

_Comment by @charliermarsh on 2024-07-16 00:00_

Thank you @TomerBin and @tiangolo 🙏 

---

_Renamed from "[`ruff`] Implement `RUF102` (`fastapi-redundant-response-model`)" to "[`fastapi`] Implement `FAST001` (`fastapi-redundant-response-model`) and `FAST002` (`fastapi-non-annotated-dependency`)" by @charliermarsh on 2024-07-21 18:17_

---

_@charliermarsh approved on 2024-07-21 18:19_

Thanks. I decided to move these to a dedicated FastAPI category. There's precedence for it with NumPy (`NPY`), and I think I'd prefer to keep the Ruff rules framework-agnostic.

---

_Label `rule` added by @charliermarsh on 2024-07-21 18:19_

---

_Label `preview` added by @charliermarsh on 2024-07-21 18:19_

---

_Merged by @charliermarsh on 2024-07-21 18:28_

---

_Closed by @charliermarsh on 2024-07-21 18:28_

---

_Comment by @marcuslimdw on 2024-07-30 04:35_

@TomerBin, thanks for your work on this!

Is this rule intended to apply only to route functions? I ask because at my company, we have a pretty comprehensive set of free functions that are used as dependency providers, for example:

`controller.py`:
```
@app.post("/")
def route(dependency: Annotated[str, Depends(providers.base_provider)]) -> None:
    ...
```

`providers.py`:
```
def base_provider(sub_provider: Annotated[str, Depends(sub_provder)]) -> str:
    ...

def sub_provider() -> str:
    ...
```

Currently, this rule only detects and fixes the route function, and not the provider function in `providers.py` (which would be pretty nice). As far as I can tell, this wouldn't be susceptible to false positives, since FastAPI dependencies are marked with a FastAPI-specific class.

---
