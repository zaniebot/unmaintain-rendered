```yaml
number: 3206
title: "`isort` need its own line-length configuration"
type: issue
state: open
author: KerberosMorphy
labels:
  - isort
assignees: []
created_at: 2023-02-24T15:04:52Z
updated_at: 2024-05-05T14:00:43Z
url: https://github.com/astral-sh/ruff/issues/3206
synced_at: 2026-01-12T15:54:43Z
```

# `isort` need its own line-length configuration

---

_@KerberosMorphy_

I remove `isort` rules in my ruff configuration to use `isort` directly since it broke my config.

I use 2 line-length, `110` for `black` and `79` for `isort`. I found it easier to read my import like that.

```toml
[tool.black]
line-length = 110

[tool.isort]
line_length = 79
```

Based on the documentation, ruff use only a main `line-length` configuration to govern them all. I would really appreciate the ability to configure `line-length` per linter as:

### `isort`

`line-length`

The line length to use when enforcing `I001` violations.

**Default value**: Will use the `tool.ruff.line-length` configuration if not specified

**Type**: `int`

**Example usage**:


```toml
[tool.ruff.isort]
# Allow lines to be as long as 79 characters.
line-length = 79
```

---

_Comment by @charliermarsh on 2023-02-25 16:53_

This isn't too hard to support, but if others are looking for this behavior, please chime in on the issue or react so we can gauge interest :)

---

_Label `isort` added by @charliermarsh on 2023-02-25 16:53_

---

_Label `good first issue` added by @charliermarsh on 2023-03-01 00:46_

---

_Comment by @evanrittenhouse on 2023-03-04 04:48_

I can try to take this!

---

_Comment by @charliermarsh on 2023-03-04 15:00_

I’m still a little torn on whether to include it. It will e.g. add complexity that we’d have to respect in the autoformatter in the future.

---

_Comment by @evanrittenhouse on 2023-03-04 21:31_

Up to you! I haven't looked at the implementation too seriously yet. Where does the complexity lie?

---

_Comment by @KerberosMorphy on 2023-03-06 14:09_

> I’m still a little torn on whether to include it. It will e.g. add complexity that we’d have to respect in the autoformatter in the future.

I understand your concern. 

@evanrittenhouse, example of added complexity to a future autoformater would be:

- If import line-length > code line-length:
  - Do we raise a configuration error while parsing user ruff settings?
  - Do we apply the shortest one regardless the user settings?
- While autoformating:
  - Do we exclude import (including import inside a function) while applying code formatter?
  - Do we apply code formatting and do a second pass to adjust import formatting?

I know W505 have it's own configuration for docstring `max-doc-length` but it don't have autofix.

---

_Comment by @lachtanek on 2023-05-02 06:41_

I can provide context where this would help: we have a legacy codebase where we've used Flake8 + Isort + Black for a long time now. Due to legacy reasons, we've configured Flake8 to use 120 line length instead of 80 or 88, so over the years the codebase sometimes has lines that long. Now we'd like to replace Flake8 + Isort with Ruff, but in that case we either have to set the line length to 88 (meaning we now have a lot of code to fix or add noqa to all of them - not really great), or set it to 120 (but in that case import autofix will reformat some imports to a single line and Black will reformat them back to multiple).
We don't really want to increase the Black's line-length (since 120 is pretty long), but sometimes having the option to have longer lines is also useful (this is where B950 would come into play, if it were in Ruff).
I hope this is useful.

---

_Comment by @allisonkarlitskaya on 2023-05-25 10:41_

We also have a similar case: a lot of old code with long lines, but we want new code written to the new standard, and certainly `--fix` should never exceed the new standard.

I was going to suggest another approach to this idea which I think would tackle the same use case: have a new toplevel setting called `max-line-length`, which is more or less specific to `E501`.  Then you could do:

```toml
[tool.ruff]
line-length = 88
max-line-length = 200
```

which would cause things like isort's autoformatting to respect the (ideal) chosen length of 88 characters when wrapping, but will prevent E501 from complaining until things get over 200.

By default, `max-line-length` would have the same value as `line-length` (ie: setting `line-length` would set both) but setting `max-line-length` on its own would leave `line-length` with its default value (88).

*edit: for now, in our project, we're just going to ignore `E501` out of hope that the problem doesn't get worse over time, and intent to fix it "some day".  That's better than having excessively-wide import lines.*

---

_Comment by @charliermarsh on 2023-06-12 16:18_

Is it always the case that users are looking for an `isort` length that's _lower_ than the standard line-length? Is it ever the case that folks want to allow import lines to be _longer_?

---

_Comment by @sector119 on 2023-06-13 17:41_

> Is it always the case that users are looking for an `isort` length that's _lower_ than the standard line-length? Is it ever the case that folks want to allow import lines to be _longer_?

we want to allow import lines to be longer than code ones

because with settings like this

[tool.ruff.isort]
force-single-line = true

we got import lines like those below :(

from epsilon_xmlrpc.app.services.account import (
    AccountServiceMultipleResultsFound,
)
from epsilon_xmlrpc.app.services.account import AccountServiceNoResultFound

not like
from epsilon_xmlrpc.app.services.account import AccountServiceMultipleResultsFound
from epsilon_xmlrpc.app.services.account import AccountServiceNoResultFound


How to keep those lines like that with black + ruff and line-length = 79 ?

---

_Comment by @Avasam on 2023-06-20 23:17_

I would use this feature to ignore line length on imports by setting the value to 999.

**Why do I want this?**

https://github.com/astral-sh/ruff/issues/5196#issue-1764346812
> 1. It takes a lot more vertical space than it needs to. The least the better.
>    Imports are something we almost never have to even look at. They can be auto imported by the IDE, and unused imports can automatically be removed. Even when I have to manually add an import (almost always `from collections.abc` because of https://github.com/microsoft/pylance-release/issues/3318, I just type it anywhere and it automatically goes to the top and the file and sorted.
> 2. The "it reduces git conflicts" argument is moot because:
>    2.1. When there's a conflict, imports are easily autofixable by just "accepting both", then running autofixes. Duplicates are automatically merged and unused are removed.
>    2.2. Randomly wrapping or unwrapping imports because it just crossed the threshold does not help reduce git conflicts, which anyway are easily autofixable as mentioned previously.
> 3. This just looks silly
>    ![image](https://user-images.githubusercontent.com/1350584/246950730-64d04aec-1dea-494d-9d9c-552ced2a2f1d.png)
>    ![image](https://user-images.githubusercontent.com/1350584/246950750-224582c0-0b8a-42f3-96c1-2e7b7522a9f0.png)



---

_Comment by @rafaelclp on 2023-08-15 00:18_

I found https://github.com/astral-sh/ruff/issues/5196 while trying to migrate from pylint to ruff, but since that was closed as a duplicate to this one, I'm commenting here instead.

My case is similar to the ones reported above, but I think it's worth adding because my import line literally cannot be broken into multiple lines - it's not just a matter of formatting preference.

We never really looked at directory/file names, given auto-import solves imports for us. So, it's no surprise we ended up with some really long module paths. I have an import line that is too long and cannot be shortened, like this:
```python
from src.some_package.another_even_deeper_package.a_really_long_module_name_for_whatever_reason import (
    TheThingWeWantToImport,
)
```

Of course, we could add `# noqa: E501`, but I consider this a regression in terms of productivity. While before we never had to care about `import` lines (due to auto-import creating them, and black+isort formatting and sorting them for us), with ruff we'd hit linter errors and would have to manually add `# noqa` to some of those lines.

Edit: to clarify, I'd like to keep code line-length at 88. Black will use that 88 to break import lines (with some lines ending up longer when it's not possible to break them further). I'd like ruff to then ignore the line-length check for the import lines. Having a `ignore-line-length-for-imports` config would be enough for my case.

---

_Label `good first issue` removed by @charliermarsh on 2023-09-14 19:48_

---

_Comment by @Kache on 2023-11-10 22:14_

> I would use this feature to ignore line length on imports by setting the value to 999 -- @Avasam

> Having a ignore-line-length-for-imports config would be enough for my case. -- @rafaelclp

The related `isort` feature for that is `multi_line_output = 7`:
https://pycqa.github.io/isort/docs/configuration/multi_line_output_modes.html#7-noqa

Which I use for the same reasons and has a corresponding issue: #2600

---

_Comment by @charliermarsh on 2024-05-05 13:39_

To recap, it's easy to implement this, but we can't really support the following use-case:

isort line length is greater than global line length, and `E501` is enabled (we'd still raise "line too long" errors for imports that exceed the global line length). For example, if you want imports to wrap at 120 characters wide, but `E501` to be raised at 100 characters wide, we would still end up raising `E501` errors on imports that exceed 100 characters wide.


---

_Comment by @rwarren on 2024-05-05 13:58_

More context on why it would be nice to decouple the linter "line too long" `E501` threshold from the isort import wrapping setting (`line_length`)...

This is for the specific case where `E501` linter warning length is greater than the isort `line_length` setting.  The opposite would definitely be a problem.... unless the isort wrap width just went to `min(isort.line_length, ruff.line_length)` to avoid such problems?

Some notes;

* `E501` should be there to catch lines deemed too long according to linter warning settings 
* auto-formatting is different from linting, and rigidly coupling their settings (through the single global `line-length` setting) can cause problems
* I wrote up additional use-case rationale in #11290 (which is a duplicate of this one... oops) that I won't repeat here


---
