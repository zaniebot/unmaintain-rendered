```yaml
number: 1198
title: "Option to sort re-exports in `__all__` (`sort_exports` from isort)"
type: issue
state: closed
author: Jackenmen
labels:
  - rule
assignees: []
created_at: 2022-12-11T18:35:16Z
updated_at: 2025-01-30T03:56:34Z
url: https://github.com/astral-sh/ruff/issues/1198
synced_at: 2026-01-10T11:09:43Z
```

# Option to sort re-exports in `__all__` (`sort_exports` from isort)

---

_Issue opened by @Jackenmen on 2022-12-11 18:35_

Isort allows sorting `__all__` when `sort_reexports` is used:
https://github.com/PyCQA/isort/blob/f547290178f86b6e5f0007cf1cb31d4736dcdaee/isort/core.py#L235-L241

---

_Label `enhancement` added by @charliermarsh on 2022-12-11 18:36_

---

_Label `isort` added by @charliermarsh on 2022-12-11 18:36_

---

_Comment by @charliermarsh on 2022-12-11 20:34_

Is this undocumented behavior?

---

_Comment by @Jackenmen on 2022-12-11 22:01_

isort has not had a release for over a year so it's one of the features that never ended up getting released. Actually, that reminds me of another (more important) feature that *also* has not been released yet, and seems like ruff isn't handling it properly either, will make an issue in a moment.

---

_Comment by @Jackenmen on 2022-12-11 22:18_

Note that there's another functionality that is available in a released version of isort - sorting literals that are prepended with `# isort: list` or `# isort: tuple`:
https://github.com/PyCQA/isort/issues/1358
`sort_reexports` option is actually using the same code for sorting.

One thing that both of these don't support is `__all__` with multiple sections in it that should be sorted separately, e.g. something like what `typing`'s code does (well, sort of, it doesn't seem like they're that good at keeping it alphabetical manually :smile:):
https://github.com/python/cpython/blob/606adb4b891b52c8b9a53d29d594e996f117c0b3/Lib/typing.py#L42-L152
I'm actually not sure if isort's literal sorting even supports other profiles such as Black's, it seems to just sort it all in a pre-defined way.

---

_Comment by @charliermarsh on 2022-12-11 22:25_

Oh yeah, this was brought to my attention on [Twitter](https://twitter.com/timothycrosley/status/1601856205672308736). I think we could support this! There are some other things I need to get done before I spend time on it though.

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:15_

---

_Comment by @Jackenmen on 2023-04-15 11:18_

I have a suggestion to extend this feature request a bit and allow sorting by the definition order of names (functions/classes/variables) that are put in `__all__`.

So for example:
```py
from .submodule import reexported_name

__all__ = (
    "reexported_name",
    "function_b",
    "X",
    "function_a",
    "function_c",
    "Y",
)


def function_b():
    ...


class X:
   ...


def function_a():
    ...


def function_c():
    ...


class Y:
   ...
```

The reason for such a suggestion is that I think it makes it easier to find out what names of public APIs are missing in `__all__` and what names shouldn't be in it (especially the latter one) since it allows one to just go through the file and look through the names in `__all__` in the same order.

---

_Comment by @kdeldycke on 2023-04-15 11:26_

For reference, there's another tool available to sort `__all__`: https://github.com/cpendery/asort

I guess it could be used to get rid of `isort` while we wait to have this feature implemented in `ruff`.

---

_Comment by @kdeldycke on 2023-04-16 05:33_

> For reference, there's another tool available to sort `__all__`: https://github.com/cpendery/asort

Hmm, forget about `asort`, I just stumble upon an issue: [`asort` breaks docstring indention (asort#8)](https://github.com/cpendery/asort/issues/8)

That being said, regarding the sorting order, I also proposed to add support for another one that would be [a case-insensitive lexicographical sorting](https://github.com/cpendery/asort/issues/9). Not sure how popular or common this is.

---

_Comment by @charliermarsh on 2023-07-24 23:42_

@cpendery - Yeah I'm open to including this. We'll need to decide how it should be categorized (i.e., does it go in the `isort` rule set? Or the Ruff-specific rules?) but it seems like a reasonable thing to include.

---

_Comment by @cpendery on 2023-07-24 23:47_

> @cpendery - Yeah I'm open to including this. We'll need to decide how it should be categorized (i.e., does it go in the `isort` rule set? Or the Ruff-specific rules?) but it seems like a reasonable thing to include.

@charliermarsh I think it probably should be categorized as Ruff specific rule since it was never an officially released isort feature, especially if we extend it with the additional functionality of multiple sorting options

---

_Comment by @kkirsche on 2023-10-12 17:48_

> I have a suggestion to extend this feature request a bit and allow sorting by the definition order of names (functions/classes/variables) that are put in `__all__`.


This existed with isort's `# isort: unique-list` code styling comment as described in #7929. 



---

_Comment by @harshil21 on 2024-01-07 17:07_

We've tried using isort's `sort_exports` in our project and it has several problems (from https://github.com/python-telegram-bot/python-telegram-bot/pull/4052#issuecomment-1875942280):

- Doesn't honor black settings, i.e. uses single quotes (`'`) instead of double (`"`).
- collapses multiline definitions (https://github.com/PyCQA/isort/issues/2193)

We found a new package https://pypi.org/project/sort-all/ which fixes these issues, however that package has one drawback of removing the comments in the `__all__`. 


---

_Comment by @Bibo-Joshi on 2024-01-08 16:29_

Adding to @harshil21 s comment, I'd like to mention that we're also interested in sorting `__slots__` in addition to `__all__`.

---

_Comment by @AlexWaygood on 2024-01-08 16:30_

I'm working on a PR to implement this feature.

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-01-09 08:03_

---

_Comment by @AlexWaygood on 2024-01-10 08:54_

> One thing that both of these don't support is `__all__` with multiple sections in it that should be sorted separately, e.g. something like what `typing`'s code does (well, sort of, it doesn't seem like they're that good at keeping it alphabetical manually 😄):
> https://github.com/python/cpython/blob/606adb4b891b52c8b9a53d29d594e996f117c0b3/Lib/typing.py#L42-L152

So, a question on this: what should ruff do if it encounters something like this?

```py
__all__ = [
    "d",
    "c",
    # a comment introducing a new section for 'b' and 'a':
    "b",
    "a",
]
```

Should it:

1. Emit the violation on this snippet (since `__all__` is not sorted), but refuse to autofix it, since the comment on its own line introduces an ambiguity regarding the user's intent
2. Sort `__all__` like this, assuming that the user wanted to use the comment to create a new section:

   ```py
   __all__ = [
       "c",
       "d",
       # a comment introducing a new section for 'b' and 'a':
       "a",
       "b",
   ]
   ```
3. Same as (2), but only if the user enabled a specific configuration flag on the command line or in their config file to enable "sorting per-section"?

I think the thing we probably _shouldn't_ do in this kind of situation is this:

```py
__all__ = [
    "a",
    "b",
    # a comment introducing a new section for 'b' and 'a':
    "c",
    "d",
]
```

---

_Comment by @Jackenmen on 2024-01-10 09:28_

For alphabetical definitely option 2. For sort by definition order I'm not really sure what would work best, I guess anything is fine though I would definitely prefer if whatever it does didn't require a manual fix.

---

_Comment by @MichaReiser on 2024-01-10 09:45_

This is an interesting question. I would expand the example by one that uses a non-section defining comment:

```python
__all__ = [
	"c",
	# TODO: Remove once #0000 is complete
	"a"
]
```

In this case, I would expect the comment to move with `a`. 

From what I understand is that isort requires the use of isort suppression comments to suppress import sorting. What if we would do the same for `__all__`?

```python
__all__ = [
    "c",
    "d",
    # a comment introducing a new section for 'b' and 'a':
    "a",  #  noqa: RUFXXX Don't move 
    "b",
]
```

or 

```python
__all__ = [
    "c",
    "d",
    # a comment introducing a new section for 'b' and 'a':  #  noqa: RUFXXX Don't move
    "a",  
    "b",
]
```


---

_Comment by @AlexWaygood on 2024-01-10 10:04_

> This is an interesting question. I would expand the example by one that uses a non-section defining comment [...] In this case, I would expect the comment to move with `a`.

Yes, that's exactly the kind of thing I was worrying about w.r.t. Option (2) -- thanks!

> From what I understand is that isort requires the use of isort suppression comments to suppress import sorting. What if we would do the same for `__all__`?

That's an interesting idea! Let's call that Option (4). I worry with Option (4) that the precise semantics of the `noqa` directive might be hard for users to get their head around, though. You'd be applying a `noqa` directive to a specific line in `__all__`, but the addition of the `noqa` directive could have an impact on ruff's treatment of all items in `__all__`.

Here's three possible things the `noqa` directive could mean if it were applied to a comment at "index 3" in a multiline `__all__` (I don't find (2) very plausible, but it would at least be simpler to implement than (3) 😅)

1. The comment should remain at "index 3", but all items in `__all__` are free to "move around" it.
2. The comment should remain at "index 3". All items above it should be sorted, but all items below it should be untouched.
3. The comment should remain at "index 3", and should be treated as a section delimiter. All items in the range of `(previous_delimiting_comment_index + 1)..3` should be sorted within their section, and all items in the range of `4..index_of_previous_delimiting_comment` should be sorted within _their_ section.

I guess I'm leaning towards Option (3) in my [original selection of options](https://github.com/astral-sh/ruff/issues/1198#issuecomment-1884429395):

- By default, comments move with the items below them with multiline `__all__` definitions.
- _But_, if a specific configuration flag is enabled, the behaviour changes so that ruff treats all comments on their own line as being "section delimiters" -- it will sort imports according to each section it spots, but won't disturb the sections.

If neither of the above is what the user wants, they can just `# noqa` the whole definition of `__all__` (or just not opt into the rule at all!), so Option (1) in my original selection isn't very appealing here.

---

_Comment by @AlexWaygood on 2024-01-10 10:06_

> For sort by definition order I'm not really sure what would work best

FWIW, my initial implementation of this rule is just going to do "sort by alphabetical order", so that I can get a MVP out there :)

We can possibly implement "sort by definition order" as a followup.

---

_Comment by @MichaReiser on 2024-01-10 11:42_

> That's an interesting idea! Let's call that Option (4). I worry with Option (4) that the precise semantics of the noqa directive might be hard for users to get their head around, though. You'd be applying a noqa directive to a specific line in __all__, but the addition of the noqa directive could have an impact on ruff's treatment of all items in __all__.

That's fair. Do you know how common the use of sections is? If they're uncommon, then disabling sorting for the entire `__all__` might be a reasonable option.

Is there some sort of a community standard when it comes to formatting section comments, that we could use to identify them? If not, could we allow users to configure a regex to identify the section comments?

> I guess I'm leaning towards Option (3) in my https://github.com/astral-sh/ruff/issues/1198#issuecomment-1884429395:

I fear that a project-level configuration might not be granular enough, but maybe it is. I really don't know.

> FWIW, my initial implementation of this rule is just going to do "sort by alphabetical order", so that I can get a MVP out there :)

That's an excellent plan. Let's get sorting working and figure out how to disable sorting at a later stage :)

---

_Comment by @AlexWaygood on 2024-01-10 11:56_

> Do you know how common the use of sections is? If they're uncommon, then disabling sorting for the entire `__all__` might be a reasonable option.

I _think_ they're reasonably common, but I don't really know -- I might just think this because I spend a fair amount of time staring at `typing.py` over at CPython, and, [as pointed out earlier](https://github.com/astral-sh/ruff/issues/1198#issuecomment-1345672433), we use them there :)

> Is there some sort of a community standard when it comes to formatting section comments, that we could use to identify them?

I don't think so, not really :( Historically, there hasn't been a well-established tool for people to check the ordering of their `__all__` definitions with, really, so I don't think it's really mattered in the past.

> I fear that a project-level configuration might not be granular enough, but maybe it is. I really don't know.

Here's an idea: as well as simple `noqa` comments to switch `isort` on and off, `isort` also supports slightly more sophisticated "action comments": https://docs.astral.sh/ruff/linter/#action-comments. What if we had a version of that for this rule, as well as a global config setting? So, if ruff saw this:

```py
__all__ = [  # ruff: sort-by-section
    ...
]
```

...then that would override the global config setting, and ruff would treat all comments taking up their own line in that `__all__` definition as being section delimiters.

---

_Comment by @MichaReiser on 2024-01-10 12:34_

> Here's an idea: as well as simple noqa comments to switch isort on and off, isort also supports slightly more sophisticated "action comments": [docs.astral.sh/ruff/linter/#action-comments](https://docs.astral.sh/ruff/linter/#action-comments). What if we had a version of that for this rule, as well as a global config setting? So, if ruff saw this

I'm a bit hesitant of adding more "magic comments" because they're hard to remember and not easily understood. You can't click on them and get an explanation of what they do; you need to know in which places they're allowed, and they don't work well with auto formatters (formatter needs to understand them as well).  That's why I would favor a solution that doesn't require a new pragma comment.

---

_Label `isort` removed by @AlexWaygood on 2024-01-16 14:54_

---

_Label `rule` added by @AlexWaygood on 2024-01-16 14:54_

---

_Comment by @AlexWaygood on 2024-01-16 15:35_

Okay, https://github.com/astral-sh/ruff/pull/9474 has now been merged! The next release of Ruff should include a rule and autofix to keep your `__all__` definitions sorted. The sort order you get is an "isort-style" sort:
- SCREAMING_SNAKE_CASE members come first
- After that come CamelCase members
- Anything else comes last
- Within each casing category, members are sorted according to a [natural sort](https://en.wikipedia.org/wiki/Natural_sort_order).

Currently the rule is not configurable. We _might_ consider adding some configuration options in the future, but for now we've decided to wait to see how much demand there is for that in the community.

The rule has been added as RUF022 rather than as an isort rule, since it seems like it doesn't really have much to do with imports. While isort does have this functionality, it's never been documented.

Enjoy tidying up your `__all__` definitions! 😄

---

_Closed by @AlexWaygood on 2024-01-16 15:35_

---

_Comment by @flisboac on 2025-01-29 16:46_

Is this rule auto-fixable? If not, do we need to open a new issue for it?

I would love to have the option to leave Ruff to sort the `__all__` list by itself. The ordering scheme is very specific to Ruff (and I like how it was defined), but this causes problems when using any other sorting mechanism (e.g. VSCode's default sort lines, which is what I've been using until I discovered that Ruff has this check).

Just for context, this how I configured my project's `pyproject.toml`:

```toml
[tool.ruff.lint]
select = ["RUF022"]
fixable = ["ALL"]
```

With the VSCode extension, ruff correctly identifies badly sorted imports, but does not fix it by itself when issuing a "Format code" command.

---

_Comment by @AlexWaygood on 2025-01-29 18:21_

> Is this rule auto-fixable?

Yup! Each rule documents whether it's fixable or not. The docs for this rule are here: https://docs.astral.sh/ruff/rules/unsorted-dunder-all/.

> With the VSCode extension, ruff correctly identifies badly sorted imports, but does not fix it by itself when issuing a "Format code" command.

Hmm, I'm not an expert on our VSCode integration, so I don't know how to debug this. @dhruvmanila might be able to help here.

Do other Ruff lint rule autofixes run when you issue a "Format code" command? The autofix is offered as part of Ruff's linter rather than our formatter -- that might be part of the issue here.

---

_Comment by @flisboac on 2025-01-29 18:29_

@AlexWaygood Thank you for the fast response!

AFAICT, the file is properly re-formatted, for all the enabled rules (which is basically only the default ones). RUF022 is the only that so far seems to not be taken into account. But RUF022's validation itself works, I'm able to get an alert in VSCode if `__all__` is not properly sorted.

---

_Comment by @flisboac on 2025-01-29 20:37_

I just noticed something: only commands do not work. Neither "Organize Imports" nor "Format Document" reorganize `__all__`.

But I can sort `__all__` with the overlay menu ("quick fix"):

![Image](https://github.com/user-attachments/assets/16d9ab27-f23b-4383-ac1e-dc0d0f36f9f5)

Now that I found it out, this is not an issue anymore, just a mild nuisance at best. :)

But it would be nice to be able to just organize `__all__` together with either action (e.g. with the normal shortcuts for format code, reorganize imports, etc.).

---

_Comment by @dhruvmanila on 2025-01-30 03:56_

So, there are 3 commands that are available in an editor context:
* Format document - this runs the formatter (`ruff format`)
* Organize imports - this runs the linter with `I` enabled (isort rules) and applies the autofix
* Fix all - this runs the linter as per the configuration and applies the autofix

`RUF022` is a linter rule that provides an autofix which means that the "Fix all" commands should be used to apply the fix. Does that make sense?

---
