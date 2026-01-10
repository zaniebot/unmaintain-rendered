```yaml
number: 14118
title: "Feature request: Allow Configurable Quote Styling for f-Strings in Python 3.12+"
type: issue
state: open
author: matthewlloyd
labels:
  - formatter
  - style
assignees: []
created_at: 2024-11-05T22:46:30Z
updated_at: 2025-12-31T16:32:55Z
url: https://github.com/astral-sh/ruff/issues/14118
synced_at: 2026-01-10T11:09:55Z
```

# Feature request: Allow Configurable Quote Styling for f-Strings in Python 3.12+

---

_Issue opened by @matthewlloyd on 2024-11-05 22:46_

I would like to request an option in Ruff formatter to configure quote styling for f-strings in Python 3.12 and later - specifically, the ability to maintain consistent use of double quotes throughout f-strings, even when nesting quotes.

## Background

With the introduction of Python 3.12, it's now possible to nest double quotes within double-quoted f-strings without needing to alternate between single and double quotes. This enhancement allows for more consistent and uniform code styling.

Currently, Ruff formatter's preview mode reverts nested double quotes to single quotes inside double-quoted f-strings, following the style used in earlier Python versions. While this approach ensures backward compatibility, it may not be ideal for codebases that have migrated to Python 3.12 and prefer consistent quote usage.

## Benefits

* Reduced cognitive load: Consistent quotes within f-strings simplify reading and writing code, reducing the mental overhead associated with switching between quote types.
* Improved readability: Uniform quote usage enhances the clarity of f-strings, especially in longer or more intricate strings with nested quotes.
* Seamless copying and pasting: Expressions within f-strings can be copied and pasted without needing to adjust quote styles, streamlining code modifications.

## Proposal

Introduce a configuration option in Ruff formatter that allows developers to:

* Opt-in to consistent quote styling for f-strings in Python 3.12 and later.
* Choose their preferred quote style (e.g., always use double quotes).

This flexibility would enable developers to take full advantage of the new string handling capabilities in Python 3.12 while adhering to their codebase's stylistic preferences.

This configuration option could be a sibling of the existing option, `quote-style`, which allows developers to choose between double or single quote characters: https://docs.astral.sh/ruff/settings/#format_quote-style

## Alternative Solution

As an alternative to adding a new configuration option, Ruff formatter could automatically switch to the new f-string quote formatting when the target Python version is set to 3.12 or higher. This would involve:

* Default Behavior Change for Python 3.12+: When the target Python version is 3.12 or above, Ruff formatter would default to using consistent quotes throughout f-strings, leveraging the enhanced syntax support.
* Backward Compatibility: For projects targeting earlier Python versions, the formatter would maintain the existing behavior of alternating quotes to ensure compatibility.
* Simplified Configuration: This approach eliminates the need for additional configuration options, making it easier for developers to adopt the new formatting style without extra setup.

## Rationale

* Alignment with Python Evolution: Adopting the new formatting by default for Python 3.12+ aligns Ruff formatter with the latest Python language features and future direction.
* Temporary Compatibility Concerns: While changing quotes might affect syntax highlighting in some tools, major IDEs like VS Code and PyCharm already support the new syntax. Any remaining tooling issues are likely to be resolved over time.
* Future-Proofing: Considering that pre-3.12 versions of Python will reach end-of-life after October 2027, updating the default behavior now helps future-proof codebases and reduces the need for later adjustments.

## Additional context

I understand that changing the default behavior may have implications for existing workflows. However, making this adjustment based on the target Python version provides a balanced approach that benefits developers who are ready to adopt Python 3.12 features while preserving the experience for those who are not.

Thank you for considering this feature request.

## References

For further discussion, see the comments here: https://github.com/astral-sh/ruff/issues/11056#issuecomment-2458269566

The pull request which changed the f-string quote style from consistent to alternating in the formatter's preview mode is here: https://github.com/astral-sh/ruff/pull/13860

---

_Label `formatter` added by @AlexWaygood on 2024-11-05 23:00_

---

_Label `style` added by @MichaReiser on 2024-11-06 08:13_

---

_Comment by @dhruvmanila on 2025-01-31 05:45_

(Cross linking for visibility: https://github.com/astral-sh/ruff/discussions/15836)

---

_Comment by @anatoli26 on 2025-02-25 03:08_

I second this request, a setting to use the same quotes everywhere, including inside f-strings, is needed. pylint is warning about inconsistent quotes, I have to run it with --disable=W1405.

---

_Comment by @dhruvmanila on 2025-02-25 10:18_

> pylint is warning about inconsistent quotes, I have to run it with --disable=W1405.

Can you provide an example of where it's triggering this rule? From what I understand reading the documentation of [W1405](https://pylint.pycqa.org/en/latest/user_guide/messages/warning/inconsistent-quotes.html) is that it's more about maintaining the consistency at a global level in the file and not specifically related to f-string but I might be mistaken.

---

_Comment by @matthewlloyd on 2025-02-26 16:59_

For those who need this feature, while waiting for my PR to be merged, you can use my `ruff` and `ruff-pre-commit` forks which add a new config flag to control the quotes in nested f-string expressions.

In your `pyproject.toml`:

```
dependencies = [
    "ruff @ git+https://github.com/matthewlloyd/ruff@0.9.7-f-string-consistent-quotes",
]
```

In your `.pre-commit.yaml`:

```
  - repo: https://github.com/matthewlloyd/ruff-pre-commit
    rev: "v0.9.7-f-string-consistent-quotes"
```

Then, to enable the config flag in your `pyproject.toml`:

```
[tool.ruff.format]
f-string-consistent-quotes = true
```

Or, in your `ruff.toml`:

```
[format]
f-string-consistent-quotes = true
```

Or, on the command line:

```
ruff format path/to/file --config "f-string-consistent-quotes=true"
```

---

_Comment by @anatoli26 on 2025-02-27 02:33_

> Can you provide an example of where it's triggering this rule?

Hi, sure, any f-string with a dict access like this:

```
f"Some text {some_dict['something']}"
```

I place double quotes around `something`, but ruff changes them to single quotes. Then pylint complains about inconsistent use of quotes.

---

_Comment by @anatoli26 on 2025-02-27 02:35_

BTW, I use

```
[tool.ruff]
target-version = "py312"
```

in my `pyproject.toml`

---

_Comment by @matthewlloyd on 2025-02-27 03:37_

Pylint's W1405 does check quotes are consistent inside nested f-string expressions when targeting Python 3.12+, see e.g. https://github.com/pylint-dev/pylint/issues/9113.

Example input (copied from that issue):

```
dictionary = {'0': 0}
f_string = f'{dictionary["0"]}'
```

Pylint output:

```
************* Module a
a.py:2:0: W1405: Quote delimiter " is inconsistent with the rest of the file (inconsistent-quotes)
```

---

_Comment by @vivodi on 2025-02-28 16:12_

> Pylint's W1405 does check quotes are consistent inside nested f-string expressions when targeting Python 3.12+, see e.g. [pylint-dev/pylint#9113](https://github.com/pylint-dev/pylint/issues/9113).
> 
> Example input (copied from that issue):
> 
> ```
> dictionary = {'0': 0}
> f_string = f'{dictionary["0"]}'
> ```
> 
> Pylint output:
> 
> ```
> ************* Module a
> a.py:2:0: W1405: Quote delimiter " is inconsistent with the rest of the file (inconsistent-quotes)
> ```

I think this justify the PR #16385 @MichaReiser 

---

_Comment by @MichaReiser on 2025-03-03 12:44_

I reply here to some of the comments made in https://github.com/astral-sh/ruff/pull/16385

As stated before, I do think that this option could be useful, but I first want to see more demand because each option has a cost: It requires that every Ruff user makes a decision on what value to pick for this new option. There are options (like quote style) where I think this is justified and others that are useful for many, but still don't meet the bar. That's why I want to hear from more users (by upvoting or explaining their reason for using same quotes if it wasn't discussed before) about why this option meets our high bar for options. 

> I think the Python official decision to introduce 'Quote reuse in f-strings' in [Python 3.12](https://docs.python.org/3/whatsnew/3.12.html#pep-701-syntactic-formalization-of-f-strings) justifies this PR.

I haven't followed the f-string PEP, so I could be wrong here but I don't think that allowing the same quotes suggests in any way that this should now be considered the idiomatic way of quoting f-strings. I see the main motivation as a) allow arbitrary nesting of f-strings and b) be less surprising as the main motivation to allow either quotes in nested f-strings. 

It's also in the very nature of an opinionated formatter to have opinions on what syntax to restrict. For example, Python supports line continuations (lines ending with `\`) but Ruff intentionally removes them in favor of normal line breaks and parentheses. There's no configuration option that allows changing this behavior. This isn't a reason not to add this option, but the opposite isn't true either: Just because Python supports a specific syntax doesn't mean Ruff should have an option for it. 

> If you're worried about providing too many options, why not make this PR (f-string consistent quotes) the only choice for users of Python 3.12 and above?

IMO, It has a poor user experience. Users upgrading their target-version from 3.11 to 3.12 would see a lot of formatting churn that's simply unnecessary. 

> Conclusion: Change the existing behavior to f-string consistent quotes instead of adding a new option. I think this is a reasonable solution, avoiding the issue raised by @MichaReiser about providing too many options.

I don't consider changing the default an option. It creates a lot of churn for users, has poor backward compatibility, and, IMO, is less readable.

I don't mean to start over the discussion on the closed PR. I've heard your concerns, and I understand your need. What I'm interested in is to hear from other Ruff users that need this feature (e.g. upvoting) and what their reasons are. 

The main reason I hear so far of why such an option might be needed other than just "I prefer it the other way" (which is a fine enough reason) is for compatibility with other tools.

---

_Comment by @vivodi on 2025-03-03 13:41_

I'll argue for the necessity of *quote reuse in f-strings* from another perspective.  

If a user sets `quote-style = "single"`, it indicates a clear preference for single quotes. In that case, Ruff should strive to use single quotes wherever possible, rather than unnecessarily introducing double quotes in f-strings against the user's preference.  

For example, I find single quotes visually comfortable, but I don't like the look of double quotes.

---

_Comment by @vivodi on 2025-03-03 13:56_

> It requires that every Ruff user make a decision on what value to pick for this new option.  

Ruff being an opinionated formatter does not prevent it from offering options to accommodate some users' preferences. The "opinionated" aspect of Ruff refers to its default configuration, rather than being a dictatorship based on the personal preferences of Ruff developers. Users who do not care about these formatting details can simply use Ruff’s officially recommended defaults. Meanwhile, for those who dislike the default settings provided by Ruff's developers, Ruff should also cater to their needs.  

Ruff can certainly have opinionated default configurations to ensure a smooth out-of-the-box experience. **However, being opinionated should also come with inclusivity and diversity—it should not turn formatting preferences into a dictatorship of the developers’ own tastes.**

I believe adding an f-string consistent quotes formatting option would not increase maintenance overhead. Users who are not concerned with minor formatting details can stick with the default configuration, while those who dislike inconsistent quote styles should have an option available in Ruff.

---

_Comment by @vivodi on 2025-03-03 14:14_

> The main reason I hear so far of why such an option might be needed other than just "I prefer it the other way" (which is a fine enough reason) is for compatibility with other tools.

Inconsistent quotes within f-strings violate [Pylint's **W1405**](https://github.com/pylint-dev/pylint/issues/9113) rule. If a user relies on both Pylint and Ruff's formatter, they will encounter Pylint errors unless they explicitly disable W1405.  

Given that Pylint is a **prominent** Python linter with a large user base—and that Ruff's linter has already adopted many of its rules—I believe Ruff's formatter should offer an option that respects Pylint's guidelines.

Moreover, the existence of W1405 itself reflects **Pylint's stance** on this issue: f-strings should maintain consistent quotes. This further justifies the necessity of a corresponding option in Ruff.

---

_Comment by @MichaReiser on 2025-03-03 14:16_

Thanks @vivodi for all your input. I understand your standpoint and I can see that this is very important to you. Let's leave room for other users to express their opinions and avoid repeating arguments that have been made before. 

---

_Comment by @anatoli26 on 2025-03-03 14:47_

> Inconsistent quotes within f-strings violate [Pylint's **W1405**](https://github.com/pylint-dev/pylint/issues/9113) rule. If a user relies on both Pylint and Ruff's formatter, they will encounter Pylint errors unless they explicitly disable W1405

Exactly my case! I use Ruff for formatting and additionally validate the code with pylint using [pylintrc](https://google.github.io/styleguide/pylintrc) from [Google Style Guide](https://google.github.io/styleguide/pyguide.html).

So I absolutely support the request for consistent quotes in f-strings. The default for a new option could be the current behavior, but Ruff users need an option to turn the consistent quotes on, otherwise the code is broken from the pylint perspective.

I have CI/CD pipelines at some clients that validate the code with pylint, so the code formatted with Ruff doesn't pass the checks, and I can't convince every organization to introduce changes to their CI/CD pipelines to suppress the W1405 warning just because my code doesn't conform to the standard.

---

_Comment by @trooz on 2025-03-15 17:44_

I need an option to enforce consistent f-string quotes. This would make batch find-and-replace operations for expressions like `entry['filepath']` simpler and less error-prone.  

Recently, while refactoring my project, I needed to change all `entry['filepath']` values from `str` to `pathlib.Path`. I updated all occurrences of `entry['filepath']` in the project, but later encountered a bug because instances like `f'{entry["filepath"]}'` were not modified. If Ruff enforced consistent f-string quotes, this bug could have been avoided.  

I have already set `quote-style = 'single'`, but `entry["filepath"]` still appears, which eventually caused the bug. I believe this is unreasonable.

---

_Comment by @vasily-v-ryabov on 2025-04-03 10:48_

Let me add my five cents. We do use `quote-style = "single"` and it works pretty well. But all multi-line strings (which are not a docstrings) are converted to triple double quotes `"""` instead of expected triple single quotes `'''`.

 1. AFAIK a docstring can be easily distinguished from other multi-line string in AST. If one more config option is unwanted, at least non-doc multi-line strings could included into action for existing `quote-style` option. It seems pretty critical to us. We have 1k+ such issues.

 2. Ideal solution would be an option `triple-quote-style`. If it was implemented, the previous case would be much less critical or even unnecessary. So I would support this option for sure. Because the first point is a kind of compromise.

 3. Regarding the way how single quoted strings are re-formatted... Replacing `'\'key\' = \'value\''` with `"'key' = 'value'"` looks obvious, but there are some cases that break quotes consistency like this:
    ```python
                '[ComponentName]: Unknown errors were found for keys:\n'
                "'key': {value!r}\n"
                '{key2!r}: {value2!r}'
    ```
    or for some collections. But such cases are pretty rare (less than 20 in our codebase), they can be handled by `# fmt: off`. It seems no action needed from Ruff team for point 3.

I think the number of necessary `# fmt: off` statements may help everybody to decide how critical their cases or not.

---

_Comment by @jbdyn on 2025-05-13 13:39_

I am also upvoting for consistent quotation marks in f-strings for the same case @anatoli26 already [mentioned above](https://github.com/astral-sh/ruff/issues/14118#issuecomment-2686702456):

```python
f"Some text {some_dict["something"]}"
```

As @vivodi [wrote before](https://github.com/astral-sh/ruff/issues/14118#issuecomment-2694434187), I like the idea to have this behavior reflected by the `quote-style` setting:

> If a user sets `quote-style = "single"`, it indicates a clear preference for single quotes.

IMO, there is conceptually no difference between inserts in a `.format`, `%` or f-string statement and [PEP 701](https://peps.python.org/pep-0701/) enables one to express this in code:

```python
d = dict(key="foo")

# either
s = "%s or bar" % d["key"]

# or
s = "{} or bar".format(d["key"])

# or
s = f"{d["key"]} or bar"
```

Thanky you all and @matthewlloyd and @MichaReiser for your commitment. Looking forward to see this implemented.

---

_Comment by @lukas-lang on 2025-10-19 14:10_

Another argument in favor of preserving quotes: Ruff is marketed as a drop-in replacement for Black. However, Black leaves the quotes alone, so switching over to Ruff leads no unexpected formatting changes, forcing you to re-format everything and having a unnecessary "formatted code" commit

---

_Comment by @GideonBear on 2025-12-31 15:16_

(I initially thought this could sidestep the formatter and be a purely-linter (Q000) issue, but it cannot, because that would cause conflicts with the formatter.)

I would like to propose an alternative solution (that, AFAICT, hasn't been suggested yet):
- Change the formatter to **ignore this altogether**, meaning, leave quotes inside f-string expressions alone completely.
	- This is technically backwards compatible, although I would understand if this is not desired; an alternative would be to make this configurable instead, like `format.keep-quotes-inside-fstrings`.
- Add a new lint rule (Q100 or something) for inconsistent quote usage:
```py
foo = f"{"bar"}"
foo = f"{'bar'}"  # Q100
foo = f'{'bar'}'
foo = f'{"bar"}'  # Q000
```

Then maybe the formatter option could be automatically enabled when that lint rule (Q100) is selected? As the formatter and linter are designed to be used together, I think making them explicitly work together like that is sensible.

Some more thoughts:
- Given the high bar for formatter settings, does it at all help to make a setting disable an opinionated formatting choice, instead of making the setting choose between two options?
- The only reason I haven't made a seperate issue for the (lint) rule suggestion, is because it would conflict with the formatter, and thus be either unusable with the formatter, or require a formatter change anyway. The strict and unconfigurable formatter is holding me back from solving this in my own way (e.g. with pylint, or manually).

I am also +1 for just making this a formatter option, but I wanted to do something with my thought of this being a natural extension of Q000. Maybe this is useful.

P.S., if there is existing discussion or documentation about formatter-linter interaction, I would be interested in reading that

---

_Comment by @MichaReiser on 2025-12-31 16:32_

I'd prefer this to be a formatter option over disabling formatting entirely. We're also reaching the number of upvotes that I think justify adding the option

---
