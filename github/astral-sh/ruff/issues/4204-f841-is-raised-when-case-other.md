---
number: 4204
title: "F841 is raised when `case other:`"
type: issue
state: closed
author: mmahmoudian
labels: []
assignees: []
created_at: 2023-05-03T13:23:21Z
updated_at: 2023-05-04T02:44:29Z
url: https://github.com/astral-sh/ruff/issues/4204
synced_at: 2026-01-07T13:12:14-06:00
---

# F841 is raised when `case other:`

---

_Issue opened by @mmahmoudian on 2023-05-03 13:23_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Ruff seems to be sensitive to `other` in match-case situation and throws `F841 [*] Local variable `other` is assigned to but never used`. For example consider this example

```py
def my_func(var):
    match var:
        case 'foo':
            print('blah1')
        case 'bar':
            print('blah2')
        case other:
            print('invalid blah')


my_func('boo')
```

The `case other:` is equivalent of `case _:` but more readable, and should be treated in the same way imho.

```console
❯  ruff test.py
test.py:8:14: F841 [*] Local variable `other` is assigned to but never used
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

I just installed Ruff version 0.0.261 and I have not done any Ruff specific settings 

-------
P.s: I searched smong open and closed issues, but considering that the `match`, `case`, and `other` are very common words and Github returns several pages of "matched issues", I might have missed seeing the correct issue and created a duplicate. I apologize if that is the case.

---

_Referenced in [astral-sh/ruff#4212](../../astral-sh/ruff/pulls/4212.md) on 2023-05-03 17:32_

---

_Comment by @JonathanPlasse on 2023-05-03 17:35_

Here are [codes](https://github.com/search?q=%22case+other%3A%22+language%3APython&type=code) that use the pattern `case other:`.

---

_Comment by @mmahmoudian on 2023-05-03 17:56_

Github search engine is nauseatingly bad, so even with your well-crafted query it still bring lots of false-positive. Nonetheless, in the first page of the same search there are python codes that have indeed used `case other:`.

Thanks for the PR. I'm n00b in Rust and also not familiar with Ruff codebase, but to me it seems in your PR, only the change in `crates/ruff/src/rules/pyflakes/rules/unused_variable.rs` is related to this issue (along with the test stuff. For instance I don't quite understand how [this](https://github.com/charliermarsh/ruff/pull/4212/files#diff-c2c8422959405016e1fb54ca23edee9716bd06ad64e610ceea9612a8291122fcL4137-R4137) or [this](https://github.com/charliermarsh/ruff/pull/4212/files#diff-7c1ff8e582049ed4a051f3895b903c6032838c2e427b7044296cc53c83db10f4R257) are related. 😅 

Anyways, I'm glad that it was a quick fix, and thanks again for working on this  👍🏼 

---

_Renamed from "F841 is raise when `case other:`" to "F841 is raised when `case other:`" by @mmahmoudian on 2023-05-03 17:57_

---

_Comment by @JonathanPlasse on 2023-05-03 18:19_

This is needed otherwise an normal assignment to a variable named `other` would be a false negative.
```python
other = 1
```
The code above would not raise an error.
Only pattern assignments should ignore `other` not other assignment kinds.

---

_Comment by @CobaltCause on 2023-05-03 18:40_

I'm not sure this is a good idea because other tools do not support silencing unused variable hints for `other`. Take `pyright` for example:

```python
import random


def main() -> None:
    match random.choice(["foo", "bar"]):
        case "foo":
            print("happy path :)")
        case other:
            print("sad path :(")

if __name__ == "__main__":
    main()

```
```
.../playground/src/playground/__main__.py
  .../playground/src/playground/__main__.py:8:14 - error: Variable "other" is not accessed (reportUnusedVariable)
1 error, 0 warnings, 0 informations
```

But if you change `other` to `_`, `pyright` understands that the binding is not intended to be used and will not produce an error.

Edit: Also, while the `case other:` search finds 346 files, a similar search for `case _:` finds ~11,600 files.

---

_Comment by @mmahmoudian on 2023-05-03 18:47_

The `other` is not a variable, it is a key word. I think you are looking at it the wrong way. Instead of checking what other tools do, you should see if python can correctly parse it.

About the popularity, I'm not surprised, people are so lazy that the first thing they do it to rename numpy to np ! As if tab completion does not exist.

I think the reference should be the python parser rather. If it works and python does not complaint, it is correct and Ruff or other tools should not complain.

---

_Comment by @CobaltCause on 2023-05-03 18:50_

> The `other` is not a variable, it is a key word.

I'm not sure what you mean by this. See below.

```
$ python -c 'import keyword; print(keyword.iskeyword("other"))'
False
```

> If it works and python does not complaint, it is correct

Why are you using a linter at all then?

---

_Comment by @mmahmoudian on 2023-05-03 18:53_

> Why are you using a linter at all then?

Simple:
1. To help me avoid mistakes before running the code
2. To help me structure my code better

And I don't expect to get false alarm about a non-existing mistake 😊

How about you?

---

_Comment by @Xiretza on 2023-05-03 18:56_

> The other is not a variable, it is a key word.

Is it? I'm pretty sure it's just an arbitrary name, there's nothing special about `other` compared to `foo`. Why should the linter not warn when the name happens to be `other`, but should warn when it's `foo`?

Meanwhile `_` *is* special, it matches anything and does not bind any variables (not even `_`, which would otherwise be a valid variable name): https://peps.python.org/pep-0636/#adding-a-wildcard

---

_Comment by @mmahmoudian on 2023-05-03 19:09_

> it's just an arbitrary name, there's nothing special about `other` compared to `foo`

You are right. Thanks for pointing it out. I always thought it is a keyword, but literally anything there would do!!

I still think the linter should not complain, or at least it should complaint with a special dedicated code that can be suppressed by the user. That said, I think this feature request is mostly pointless and should be closed.

@Xiretza Thanks again for correcting my mistake. 

---

_Comment by @Xiretza on 2023-05-03 19:12_

> it should complaint with a special dedicated code that can be suppressed by the user.

Not sure why that would be necessary, but a good addition might be a note that `_` should be used if the result of the match is not intended to be used.

---

_Comment by @mmahmoudian on 2023-05-03 19:15_

> Not sure why that would be necessary

Simply because the [F841](https://www.flake8rules.com/rules/F841.html) is "Local variable name is assigned to but never used" which is literally not the case here. Actually here (assuming that `other` or `foo` are variables) a variable is **not** defined but **is** used. 

> a good addition might be a note that _ should be used if the result of the match is not intended to be used.

This is a very good suggestion. But imho should not be printed along with F841 as F841 can happen else where, but `_` can not be replaced in every condition (e.g the example in the flake8rules)

---

_Comment by @Xiretza on 2023-05-03 19:16_

> Actually here (assuming that `other` or `foo` are variables) a variable is **not** defined but **is** used.

That's not how `match` works. In this case, it assigns the content of `var` to `other` unless it's `"foo"` of `"bar"`. You then never use that variable.

---

_Comment by @charliermarsh on 2023-05-04 02:41_

My feeling is that the rule is correct as-implemented. The `match` statement is defining the `other` variable, and it takes on whatever value is being matched against (it's a "wildcard" match, IIRC). In this case, the `other` variable is being defined, but not used. If you _do_ have a fall-through case like that, I think `_` better matches the intent and Python idioms of an "intentionally unused value".

Note that, if you feel strongly about allowing `other` specifically, you can change the [`dummy-variable-rgx`](https://beta.ruff.rs/docs/settings/#dummy-variable-rgx) in your configuration file to treat `other` as a permitted-unused value.


---

_Closed by @mmahmoudian on 2023-05-04 02:44_

---
