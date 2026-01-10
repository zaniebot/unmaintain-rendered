```yaml
number: 3676
title: "[Autofix Error] noqa Removed"
type: issue
state: closed
author: JacobCoffee
labels:
  - question
assignees: []
created_at: 2023-03-23T05:19:06Z
updated_at: 2023-03-26T14:51:13Z
url: https://github.com/astral-sh/ruff/issues/3676
synced_at: 2026-01-10T11:09:46Z
```

# [Autofix Error] noqa Removed

---

_Issue opened by @JacobCoffee on 2023-03-23 05:19_

              When adding TRY rules in ruff configuration for napari project, then I add # noqa TRY301

				        try:
				            raise ValueError("a")  # noqa TRY301
				        except ValueError:
				            threading.excepthook(sys.exc_info())
				
				Then in review there was a decision to skip TRY301 For all test files.
				
				Then running ruff ends with:
				
				error: Autofix introduced a syntax error. Reverting all changes.
				
				This indicates a bug in `ruff`. If you could open an issue at:
				
				    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D
				
				...quoting the contents of `napari/_qt/_tests/test_qt_notifications.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
				
				Updating code to
				
				        try:
				            raise ValueError("a")  # noqa: TRY301
				        except ValueError:
				            threading.excepthook(sys.exc_info())
				
				Makes auto-fix work.
				
				It comes from the incorrect noqa # noqa TRY301 instead of # noqa: TRY301. But it looks like auto-fix should comment out the rest of the text if something is left after removing noqa
				
				(in napari I introduce ruff in parts to make the review possible)

              I believe this was fixed in https://github.com/charliermarsh/ruff/pull/3589 (not yet released).

_Originally posted by @charliermarsh in https://github.com/charliermarsh/ruff/issues/3666#issuecomment-1479728963_

I believe this introduces a bug whereby the noqa is completely blasted away.

![image](https://user-images.githubusercontent.com/45884264/227110657-10a95107-260b-4bba-abc1-500ce6fe2fda.png)

```shell
│polyfactory on  ruff-integration פֿ 6  2 🤷 2 ⇣ 1 is  v1.17.2 via  v3.8.16 🐠 took 38s                                                                                                                                                                                                                                                                     │
│✗ pip show ruff                                                                                                                                                                                                                                                                                                                                               │
│Name: ruff                                                                                                                                                                                                                                                                                                                                                    │
│Version: 0.0.258                                                                                                                                                                                                                                                                                                                                              │
│Summary: An extremely fast Python linter, written in Rust.                                                                                                                                                                                                                                                                                                    │
│Home-page: https://github.com/charliermarsh/ruff                                                                                                                                                                                                                                                                                                              │
│Author: Charlie Marsh <charlie.r.marsh@gmail.com>                                                                                                                                                                                                                                                                                                             │
│Author-email: Charlie Marsh <charlie.r.marsh@gmail.com>                                                                                                                                                                                                                                                                                                       │
│License: MIT
```
```yaml
  - repo: https://github.com/charliermarsh/ruff-pre-commit
    rev: "v0.0.258"
```

![image](https://user-images.githubusercontent.com/45884264/227110985-23fedb54-695e-4416-a231-e5e3ac8d63dc.png)


---

_Comment by @JacobCoffee on 2023-03-23 05:24_

I also have this issue where this comment is deleted for some reason, if it is related or another issue or my own doing

https://github.com/JacobCoffee/polyfactory/blob/c95b3e9ed71eb15afc65def2aa3eb5700654f5ae/polyfactory/value_generators/constrained_numbers.py#L92

![image](https://user-images.githubusercontent.com/45884264/227111241-2080c445-9df4-4567-820c-1e9be1bc202b.png)


---

_Comment by @charliermarsh on 2023-03-23 17:05_

I think the second issue is probably an `eradicate` bug -- it assumes that's commented-out code, and tries to remove it.

For the first issue, do you have a link to the full file? Or can you include the contents and pyproject.toml here?

---

_Label `bug` added by @charliermarsh on 2023-03-23 17:05_

---

_Label `question` added by @charliermarsh on 2023-03-23 17:07_

---

_Comment by @JacobCoffee on 2023-03-24 05:24_

Some examples
https://github.com/JacobCoffee/polyfactory/blob/ruff-integration/pyproject.toml

test_allows_user_to_define_faker_instance 
![image](https://user-images.githubusercontent.com/45884264/227431704-ae2a0126-0246-4d51-84d8-442fa945b01e.png)
https://github.com/JacobCoffee/polyfactory/blob/c95b3e9ed71eb15afc65def2aa3eb5700654f5ae/tests/test_faker_customization.py#L9

https://github.com/JacobCoffee/polyfactory/blob/c95b3e9ed71eb15afc65def2aa3eb5700654f5ae/polyfactory/value_generators/regex.py#L120


2nd issue eradicate potential bug
https://github.com/JacobCoffee/polyfactory/blob/ruff-integration/polyfactory/value_generators/constrained_numbers.py#L91

---

_Comment by @charliermarsh on 2023-03-24 12:18_

Thanks, will revisit this in the afternoon!

---

_Comment by @charliermarsh on 2023-03-24 23:48_

I think these may both be correct -- that is, the `noqa` comments _do_ seem to be unused, and so removing them seems _correct_.

In the first case, you have `B010` in the [`ignore`](https://github.com/JacobCoffee/polyfactory/blob/ruff-integration/pyproject.toml#L152), so that `noqa` is technically unused. 

In the second case, we don't flag `R504` if you have a function call between the assignment and the `return` (since that function call could have a significant effect, as in your case):

```py
def __call__(self, string_or_regex: Union[str, Pattern]) -> str:
    """Generate a string matching a regex.

    :param string_or_regex: A string or pattern.

    :return: The generated string.
    """
    pattern = string_or_regex.pattern if isinstance(string_or_regex, Pattern) else string_or_regex
    parsed = parse(pattern)
    result = self._build_string(parsed)
    self._cache.clear()
    return result
```

So that `noqa` isn't needed either.



---

_Comment by @charliermarsh on 2023-03-24 23:48_

Are you seeing that the `noqa` comments are ignored, and now the errors appear when you run Ruff? Maybe there's a version mismatch somewhere.

---

_Label `bug` removed by @charliermarsh on 2023-03-24 23:48_

---

_Comment by @JacobCoffee on 2023-03-25 00:57_

When running ruff on .256 through .258 I see the `# noqa: XYZ` completely removed

---

_Comment by @charliermarsh on 2023-03-25 03:10_

But, I think that might be the correct and intended behavior, since you have `RUF100` enabled, which detects and removes unnecessary `# noqa` comments, and those comments seem to be unnecessary. So the removal is the autofix behavior for `RUF100`.

---

_Comment by @charliermarsh on 2023-03-25 16:14_

You can remove `RUF100` from the rule set to turn this off? Or make it `unfixable`? But I think we might be talking past each other. What's the problem with removing the `# noqa: XYZ` comments?

---

_Comment by @JacobCoffee on 2023-03-25 16:53_

Im not sure that the others would have an issue with removal, but for example this: https://github.com/JacobCoffee/polyfactory/blob/c95b3e9ed71eb15afc65def2aa3eb5700654f5ae/polyfactory/value_generators/constrainted_strings.py#LL16 fails on not being correct casing, so adding the noqa was what I did during migration to ruff for this repo. Maybe I am just going about it wrong, though?

---

_Comment by @charliermarsh on 2023-03-26 14:51_

Hmm, I think everything here is working as intended, but maybe I can clarify that _intent_.

To start from first principles: `# noqa` comments exist to suppress errors. So if you have an unused import, like `import os`, and you want to tell Ruff to ignore that error (and, thus, _not_ to remove it), you'd add `import os  # noqa: F401`.

If instead you changed the code to reference `os` somewhere, the import would no longer be unused, so you could remove the `# noqa: F401` comment -- since Ruff wouldn't be raising the error anymore, there'd be nothing to suppress.

At that point, the `# noqa: F401` comment would be unnecessary. Ruff has a rule (`RUF100`) to detect those _unnecessary_ `noqa` comments, flag them, and remove them. So if you ran Ruff over this:

```py
import os  # noqa: F401

x = os.path.join("foo", "bar")
```

Ruff would raise a `RUF100` on the first line, given the unnecessary `noqa` comment (the import _is_ used), and Ruff would remove it if run with `--fix`.


---

_Closed by @charliermarsh on 2023-03-26 14:51_

---
