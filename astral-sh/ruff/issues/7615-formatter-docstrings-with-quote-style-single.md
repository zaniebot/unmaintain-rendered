```yaml
number: 7615
title: "Formatter: Docstrings with `quote-style = \"single\"` configuration"
type: issue
state: closed
author: alkatar21
labels:
  - formatter
  - accepted
assignees: []
created_at: 2023-09-23T12:42:07Z
updated_at: 2025-08-31T05:28:21Z
url: https://github.com/astral-sh/ruff/issues/7615
synced_at: 2026-01-10T11:09:49Z
```

# Formatter: Docstrings with `quote-style = "single"` configuration

---

_Issue opened by @alkatar21 on 2023-09-23 12:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Following https://github.com/astral-sh/ruff/issues/1904#issuecomment-1731862483 I tried the new configuration
```toml
[tool.ruff.format]
# Use single quotes rather than double quotes.
quote-style = "single"
```

But now docstrings are also formatted with single quotes:
```py
def func():
  '''Docstring.'''
  string_taking_function('A string')
```
I thought with docstrings there is no real discussion that they always use `"""` and it only refers to normal strings and [PEP 8](https://peps.python.org/pep-0008/#string-quotes) also says so. Like the following example:
```py
def func():
  """Docstring."""
  string_taking_function('A string')
```

There was a short discussion on [Discord](https://discord.com/channels/1039017663004942429/1073384953741574217/1155091710813159484) already where @charliermarsh mentioned the following:
> Yeah need to look into this — we’ve discussed it before but started off by using the configured quotes universally.
> Worth checking what the single-quote Black forks do here, and if users of our flake8-quotes plug-in tend to use double quotes for docstrings even when enforcing single quotes 

For me it would be important that docstrings always use `"""` regardless of `quote-style`. I am not quite sure if this is to be understood as a feature request or rather as a bug. [D300](https://docs.astral.sh/ruff/rules/triple-single-quotes) considers this as a bug as well.


---

_Label `formatter` added by @konstin on 2023-09-23 12:43_

---

_Label `needs-decision` added by @konstin on 2023-09-23 12:43_

---

_Comment by @vanchaxy on 2023-09-23 13:47_

We also use "single everywhere except docstring". +1 to always use double for docstring (preferably) or make it configurable. 

---

_Comment by @charliermarsh on 2023-09-23 15:53_

It looks like [Blue](https://pypi.org/project/blue/) also uses double quotes for triple-quoted strings and docstrings, and single quotes everywhere else.


---

_Comment by @charliermarsh on 2023-09-23 15:59_

Some data from Code Search:

- 22 files with `docstring-quotes = "single"` ([source](https://github.com/search?type=code&q=%22docstring-quotes+%3D+%5C%22single%5C%22%22+path%3Apyproject.toml))
- 57 files with `multiline-quotes = "single"` ([source](https://github.com/search?type=code&q=%22multiline-quotes+%3D+%5C%22single%5C%22%22+path%3Apyproject.toml))
- 372 files with `inline-quotes = "single"` ([source](https://github.com/search?type=code&q=%22inline-quotes+%3D+%5C%22single%5C%22%22+path%3Apyproject.toml))

So the majority of projects that are using single quotes only apply it to non-docstring, non-multiline strings, but there are _some_ projects that deviate from that pattern.


---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-24 15:22_

---

_Comment by @MichaReiser on 2023-09-25 11:55_

@alkatar21 would you expect that `"` are also used for non-docstring triple quoted strings? 

What about a single-quoted docstring? Should it use `"` or `'`': 

```python
def test():
    'Single quoted docstring'
   ...
```

Using `"` for all triple quoted strings would be in line with [PEP8](https://peps.python.org/pep-0008/#string-quotes)

We can
* Change `quote-style` to only apply to not-triple quoted strings
* and/or add a new `quote-style-triple-quoted` (or similar) 

---

_Comment by @alkatar21 on 2023-09-25 13:05_

> @alkatar21 would you expect that `"` are also used for non-docstring triple quoted strings?

I believe the following are all docstrings (used by editors and sphinx for example) and should therefore use `"`.
```py
class ExampleClass:
    """Class docstring."""
    variable1: int
    """Docstring for variable1."""
    variable2: int
    """Docstring for variable2."""

    def __init__(self, parameter: str) -> None:
        self.parameter: str = parameter
        """Docstring for parameter."""
```
I don't remember ever using non-docstring triple quoted strings and accordingly have no personal opinion about it, but I would suggest to follow PEP8.

> What about a single-quoted docstring? Should it use `"` or `'`':
> 
> ```python
> def test():
>     'Single quoted docstring'
>    ...
> ```

I didn't even know that is possible, but off the top of my head I'd say `'` since it's a simple string?

> Using `"` for all triple quoted strings would be in line with [PEP8](https://peps.python.org/pep-0008/#string-quotes)

Then I would suggest to follow PEP8.

> We can
> 
>     * Change `quote-style` to only apply to not-triple quoted strings
> 
>     * and/or add a new `quote-style-triple-quoted` (or similar)
I would suggest to follow PEP8 and implement ` Change 'quote-style' to only apply to not-triple quoted strings`. It is probably easier and well justified with PEP8. If there is a big demand, you can always consider adding the option later. I think it is better to possibly add the option at some point in the future than to regret about the option later on.


---

_Comment by @charliermarsh on 2023-09-25 13:11_

@MichaReiser - the other option is to mirror the configuration options that we use for flake8-quotes: a setting for inline quotes, a setting for multiline quotes, and a setting for docstrings (which applies to single- and triple-quoted docstrings).

---

_Comment by @konstin on 2023-09-25 13:17_

I'd prefer only having at most a single quotes setting

---

_Comment by @charliermarsh on 2023-09-25 13:24_

I agree that it would be nice to have a single setting. The data I scraped above demonstrates that we can’t use a single setting and still support all users, but we could make that choice.

If we want a single setting, my suggestion would be to enforce double quotes for docstrings and triple-quoted strings, and use the string setting for all others. This would be consistent with Blue and Pyink which always use double quotes for such strings.

---

_Comment by @MichaReiser on 2023-09-25 13:42_

> @MichaReiser - the other option is to mirror the configuration options that we use for flake8-quotes: a setting for inline quotes, a setting for multiline quotes, and a setting for docstrings (which applies to single- and triple-quoted docstrings).

Yeah, I prefer having as few options as possible. Ideally, just one, two if we must. I would try to avoid having all three. 

The other part that's interesting about this is... Should these options now become global? Or are we fine repeating the configuration twice (I would argue it's fine, because the formatter makes the lint rule obsolete)



---

_Comment by @tanoc on 2023-09-27 14:30_

I think having the same settings as in flake8-quotes is not a bad idea. I'm fine with defaults values for multiline and docstrings checking what is most commonly used, but excluding customization on these settings could create the same problem that fueled the need for Black forks (Blue, Pyink, ...)

---

_Label `needs-decision` removed by @MichaReiser on 2023-09-27 16:22_

---

_Comment by @charliermarsh on 2023-09-27 16:23_

Our current plan is to always use double quotes for docstrings and triple-quoted strings (as per PEP 8 and PEP 257). We'll change this an upcoming release, prior to the Beta.

If there's still strong demand, we'll then consider adding an additional dedicated setting to use single quotes everywhere (i.e., for docstrings and triple-quoted strings in addition).


---

_Comment by @tanoc on 2023-09-27 17:11_

Seems reasonable. Just to add to your data, please keep in mind that there [a lot of projects](https://github.com/search?type=code&q=%22id%3A+double-quote-string-fixer%22+path%3A.pre-commit-config.yaml) using pre-commit with `double-quote-string-fixer` (that just fixes normal strings and doesn't change docstrings or triple-quoted strings, so maybe is not so black an white as it seems).

---

_Label `accepted` added by @charliermarsh on 2023-09-27 18:45_

---

_Comment by @warsaw on 2023-09-27 19:28_

I just tried out `quote-style = 'single'` on one of my repos and hit exactly this problem.

> It looks like [Blue](https://pypi.org/project/blue/) also uses double quotes for triple-quoted strings and docstrings, and single quotes everywhere else.

Yes, `blue` explicitly keeps triple-quoted-double-quotes for docstrings.

---

_Comment by @charliermarsh on 2023-09-27 19:39_

👍 We're going to change this, probably in the upcoming release (or the next one, if it doesn't make it).

---

_Comment by @warsaw on 2023-09-27 19:49_

Great to hear @charliermarsh.  I'm super excited about being able to switch to `ruff format`.

I noticed one other formatting deviation from blue related to right-hanging comments, but I'll open a new ticket on that when I get a chance, if it hasn't already been reported.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 20:23_

---

_Closed by @charliermarsh on 2023-09-28 19:11_

---

_Comment by @Destroy666x on 2023-10-13 04:13_

Please reconsider this as it's a bit nonsensical as of now since you can do:
```toml
[tool.ruff.flake8-quotes]
docstring-quotes = 'single'
inline-quotes = 'single'
multiline-quotes = 'single'
```

and then the formatter is just straight up incompatible with the other part of the software. Software definitely shouldn't conflict with itself.

You could just split it into 3 variables like above for the most compatible approach.

If you need any arguments why use single quotes - they're simply easier to press and that's the advantage. Disadvantages? None for me, I don't mind that let's say 90% of projects go for doubles, no other tool that I'm using has problems with that either.

---

_Comment by @Destroy666x on 2023-10-20 14:47_

Formatter is unrelated to rules, as can be also seen from my comment above. Doing that changed nothing indeed. There's currently no way to disable the quote formatting, pretty sure.

EDIT: comment got deleted in case someone is confused.

---

_Comment by @henryiii on 2023-10-26 14:04_

FYI, the disadvantage is that you're much more likely to have single quotes inside a string of text than a double quote. Your comment above has one single quote, and mine has seven. Neither of us used a double quote.

And easier to press doesn't matter if your formatter converts them to double quotes for you anyway. ;)

(That's the reasoning behind Black's choice, by the way)

Though I'd also add that single quotes are used by Python's built in repr's.

---

_Comment by @henryiii on 2023-10-26 14:10_

I was hoping to covert projects like pypa/black and pyproject-metadata to the Ruff formatter, and wasn't able to due to this - previously we used `double-quote-string-fixer` to convert strings to single quotes, so all our triple quoted strings are single quoted already. I don't really see why someone would want double quoted triple strings if they wanted single quoted strings - but it's a bit hard for me to be sure though, since I'm personally on the double quote "go with black" side. Triple quoted strings actually do not have the one key differentiator for double/single quotes: you can put single or double quotes inside them!

But anyway, I'd like to adopt ruff-format in these projects without having to ask all our developers about changing styles. Though I could ask, given what PEP says.

---

_Comment by @henryiii on 2023-10-26 14:21_

Ahh! `double-quote-string-fixer` leaves triple quoted strings alone! So we have both types of strings in the codebase, I think that's totally fine to standardize on something, and moving toward more double quotes is good! Not an issue for me anymore. I'll let someone who actually cares about single quotes argue about triple quoted strings if they care. :)

Though I notice all the triple quoted strings that were changed were not docstrings. :(

---

_Comment by @Destroy666x on 2023-10-26 14:33_

> FYI, the disadvantage is that you're much more likely to have single quotes inside a string of text than a double quote. Your comment above has one single quote, and mine has seven. Neither of us used a double quote.

That's... quite unreasonable I'd say, as it assumes, I don't know why:
- usage of shortened forms
- language - English, which uses `'` quite often indeed. Most of other languages don't.
- language usage in the 1st place, my software could be just a bot with nearly 0 user interaction. Or I could have software with good practice of keeping translations in some config/data-like files, e.g. YAML or JSON. This would mean most of strings are programming-related and unlikely to have any of `'` or `"`, unless parsing of specific formats occurs, but then e.g. dealing with JSON would mean mostly dealing with `"`.

You just can't generalize like that, so still no idea why software vendors/authors try to enforce such things TBH, especially when introducing inconsistencies like I mentioned. And the formatters utilize double minimal escaping quoting method anyways, so wrapping with the other when necessary occurs. It really shouldn't matter what's used and what's the user's preference. 

> And easier to press doesn't matter if your formatter converts them to double quotes for you anyway. ;)

True. I could just prefer the more minimalistic look of `'` though, including while reading the code in repository, why not. I'm not that pedantic but some people might be.

---

_Comment by @henryiii on 2023-10-26 14:46_

I'm not sure I'd say "most other" - At least Spanish and French both use single quotes, and pretty much don't use double quotes at all! (`«»` used instead). Languages that don't use either don't matter, since either is fine. Also, Python is written in English. I think it's enough to break the symmetry; it doesn't need to be a huge reason since otherwise it's a toss-up, it's just a character choice. And if you are dealing with formatted intput/output like json you should use a library (factor out the quoting) and not try to manually handle quoting. :)

Also, many other (computer) languages don't give you the option, and those mostly force double quotes (C, C++, Java, Rust, Matlab, Go, Swift) for strings. Using double quotes in Python is also consistent with those languages, and makes it easier to go back and forth. (note: JavaScript and Fortran are like Python. Ruby and Bash allow both, but double quotes interpolate and single do not. I don't know of a language that forces single quotes.)

Anyway, doesn't really matter here, I think here the thing to argue is if you like single quotes, do you like triple quoted single quotes? Due to PEP 257, enforcing double quoted docstrings seems fine, but what about free-standing single triple quotes? That's what we have (hopefully had) in pypa/build. Docstrings used double, everything else (even if triple quoted) was single. I don't like single quotes anyway, so I'm a bad judge. :)

---

_Comment by @warsaw on 2023-10-26 17:19_

>  including while reading the code in repository

Yes, exactly.  It's not just about the ease of *writing* single quotes over double quotes (at least for US qwerty keyboards, don't know about others), but about *reading* code.  The extra screen grit of double quotes can reduce the readability of code too.

My understanding was that black adopted this style because it was more compatible with JavaScript or JSON or some such, with little ergonomic consideration.

---

_Comment by @Destroy666x on 2023-10-26 19:22_

> I'm not sure I'd say "most other" - At least Spanish and French both use single quotes, and pretty much don't use double quotes at all

I don't think any of Asian languges does and that's already the most as there are loads of variations. While French uses them as frequently as English, I don't know where you see them in Spanish - I personally almost never do. They're mostly popular in Germanic languages I'd say. 

> Also, Python is written in English. I think it's enough to break the symmetry

Why does that matter at all...? Not only does Python allow both, but also the language used by authors is quite irrelevant IMO, while we're talking about a specific tool especially.

> And if you are dealing with formatted intput/output like json you should use a library (factor out the quoting) and not try to manually handle quoting

This point makes no sense, as I was obviously talking from perspective of someone e.g. writing the librar to parse the JSON.

> Using double quotes in Python is also consistent with those languages

Why would I want to be consistent with entirely different languages though? Python is normally way way more loose than C, C++ or Java and rather uncomparable. The fact that C doesn't have lambdas doesn't mean I should avoid lambda usage, for instance. The fact that languages differ is an advantage of variety of them, not a disadvantage. You can code a quick bot without type hints and without caring much about formatting with Python and then a business application with 1000 classes with Java that follows very specific standards.

> do you like triple quoted single quotes?

I don't mind them at all ¯\_(ツ)_/¯ E.g. for the reason specified by the person above.


I think you're way too focused on defending a specific style while not considering the freedom aspect at all.


---

_Comment by @henryiii on 2023-10-27 01:47_

I think this discussion shows why Black didn't make it configurable, and just picked a "correct" one. :) Happy to argue about why double quotes are better somewhere else, but it doesn't have anything to do with this issue - Ruff-format already lets you pick single quotes!

To summarize: if you set `quote-style = "single"`, you get this:

```python
def f():
    """
    Docstring.
    """

    single = 'hello'
    triple = """Hi there"""
```

The docstring is correct, PEP 257 states docstrings should use double quotes. The triple quote string is correct according to PEP 8, which states all triple quoted strings should be made with double quotes. I'm okay with Ruff not providing configurability to contradict a PEP - I'll try to get it updated in pypa/build and pyproject-metadata. The one place where it's left open by formatting PEPs is for simple strings, and Ruff has an option for that, which is nice. Though IMO, even just seeing them above, I'd much rather just make the simple string double quoted too. ;)

Having a '"preserve"` option would be fantastic (didn't notice it was missing!), and would help people do whatever they are doing now with Black. Being at least as configurable as Black is a low bar. ;)

---

_Comment by @MichaReiser on 2023-10-27 02:15_

> Having a '"preserve"` option would be fantastic (didn't notice it was missing!), and would help people do whatever they are doing now with Black. Being at least as configurable as Black is a low bar. ;)

`preserve` would be slightly different to Black's `--skip-string-normalization` in the sense that it still removes unnecessary escapes and normalizes string prefixes. It's the same in that it keeps your quotes the same. However, I want to wait with adding it until we figure out how to handle the new string preview feature that automatically joins and splits strings. The formatter could get the quotes wrong when splitting or joining strings depending on how obscure the quote  rules are in a project.

---

_Comment by @ThiefMaster on 2023-11-09 18:33_

I'd also like to be able to configure quote styles separately. flake8-quotes does it very nicely, so why not allow autoformatting to be as flexible:

```
inline-quotes = single
multiline-quotes = single
docstring-quotes = double
avoid-escape = true
```

---

_Comment by @ThiefMaster on 2023-11-09 18:37_

Actually, if you want a single setting: How about a `quotes = match-linter` option? That way you have onle ONE setting and it uses the config of the flake8-quotes rules...

---

_Comment by @MichaReiser on 2023-11-29 03:59_

We're considering adding the `preserve` option in https://github.com/astral-sh/ruff/pull/8822. Please comment on the PR if you use `--skip-string-normalization` and your use case isn't covered by Ruff's settings today nor after adding the `preserve` option. Thanks

---

_Comment by @sknaumov on 2025-08-31 05:28_

Have you considered having `quote-style: single-pep8` option to support formatting with single quotes for non-triplequote cases?

I think that it solves most concerns:
1. Support people who are using `blue` formatter but want to use more modern one, without reformatting their codebase.
2. Provide single quotes option that conforms to PEP8.
3. Do not introduce too many config options, unlike some proposals in this ticket => no exponential number of configurations to test.

---
