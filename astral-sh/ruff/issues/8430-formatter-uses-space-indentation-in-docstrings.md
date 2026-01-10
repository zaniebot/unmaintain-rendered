```yaml
number: 8430
title: "Formatter: Uses space indentation in docstrings when using `indent-style = \"tab\"`"
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-11-02T01:50:42Z
updated_at: 2024-02-12T15:09:15Z
url: https://github.com/astral-sh/ruff/issues/8430
synced_at: 2026-01-10T11:09:50Z
```

# Formatter: Uses space indentation in docstrings when using `indent-style = "tab"`

---

_Issue opened by @MichaReiser on 2023-11-02 01:50_

If I have indent-style set to "tab", when I run ruff format it is still converting indentation within docstring comments to be spaces, which then causes ruff check to fail on rule E101: Indentation contains mixed spaces and tabs

Before: 

![image](https://github.com/astral-sh/ruff/assets/10078284/bbe6eda5-ac45-4b0a-b04a-a66169adea1b)

After:

![image](https://github.com/astral-sh/ruff/assets/10078284/fe3e91b2-15ee-499a-a306-69988ff85513)

Is there a format setting I can add to fix this? or is this a bug/intentional?

_Originally posted by @1024Adam in https://github.com/astral-sh/ruff/discussions/7310#discussioncomment-7450056_

We can reproduce this behavior in our own tests:

https://github.com/astral-sh/ruff/blob/3076d76b0ab7a4b221c3ec1f34a8b68ed07059b1/crates/ruff_python_formatter/tests/snapshots/format@docstring.py.snap#L538-L544

https://github.com/astral-sh/ruff/blob/3076d76b0ab7a4b221c3ec1f34a8b68ed07059b1/crates/ruff_python_formatter/tests/snapshots/format@docstring.py.snap#L569-L577

E101 has no special handling for docstring. It simply looks at every line and tests if the leading whitespace contains mixed spaces and tabs. Meaning, there can also be false positives if a multiline string contains leading whitespace that is a mix of spaces and tabs.

---

_Label `formatter` added by @MichaReiser on 2023-11-02 01:51_

---

_Comment by @MichaReiser on 2023-11-02 02:00_

@konstin you implemented the docstring formatting and have a lot of context on the Python interop. Could you take this on as a formatter goal sometime this month? The solution doesn't have to be to fix the docstring formatting, we could also warn about using `E101` with the formatter. 

I think what the formatter should support is that it preserves tabs when using `indent-style=tab` by using `indent`. It means it wouldn't fix mixed indentation in docstrings for you, but it, at least, doesn't result in mixed indentation.

The alternative is that we always use `indent` (and `align` if it isn't a multiple of tab-width). But I think there we have the issue that Python always assumes a tab-width of 8

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-11-02 02:00_

---

_Assigned to @konstin by @konstin on 2023-11-02 09:28_

---

_Comment by @MichaReiser on 2023-11-03 01:24_

Depends on https://github.com/astral-sh/ruff/issues/8445

---

_Comment by @Mutantpenguin on 2023-11-09 12:54_

Is there any way to circumvent this behaviour right now? Like telling the formatter to not format docstrings at all?

---

_Comment by @konstin on 2023-11-10 11:14_

@Mutantpenguin What is the formatting that you want for docstring when using tab indentation? And if you use some documentation generator (like sphinx), do you know how they react to tab indent vs. space indent, and if indent size matters?

---

_Comment by @Mutantpenguin on 2023-11-13 11:34_

We use handwritten docstrings so I can't say anything about sphinx and so on.

I want the same as MichaReiser:
* the formatter shouldn't use spaces when I specified tabs
* the comment shouldn't be indented more than before (as you can see in his screenshots)


E114 (which is in preview) should be taken into account too https://docs.astral.sh/ruff/rules/indentation-with-invalid-multiple-comment/

---

_Comment by @Mutantpenguin on 2023-11-22 10:46_

I now have a nice testcase.

ruff.toml:
```toml
[format]
indent-style="tab"
```

This stays the same after formatting and doesn't get changed at all:
```python
def test(arg1: str) -> None:
	"""
	Arguments:
	arg1: super duper arg with a tab in front
	"""
```

This stays the same after formatting and doesn't get changed at all too:
```python
def test(arg1: str) -> None:
	"""
	Arguments:
	    arg1: super duper arg with a tab and 4 spaces in front
	"""
```

This
```python
def test(arg1: str) -> None:
	"""
	Arguments:
		arg1: super duper arg with 2 tabs in front
	"""
```

gets butchered and formatted to this:
```python
def test(arg1: str) -> None:
	"""
	Arguments:
	        arg1: super duper arg with 1 tab and 8 spaces in front
	"""
```

What the GitHub code formatting hides: we use a tab size of 4 in our editor so the last case looks expecially weird since the 8 spaces look like 2 indentation levels with tabs.

---

_Comment by @MichaReiser on 2023-11-29 03:50_

@konstin are you still working on this? If not, feel free to un-assign.

---

_Unassigned @konstin by @konstin on 2023-11-29 16:07_

---

_Comment by @konstin on 2023-11-29 16:09_

No, i don't have a good solution for this. I'd probably go with replacing every 8 spaces in the beginning of a docstring with tabs, implementation-wise, even if whatever we do is bad because it we either mismatch with cpython (always unconditionally assumes tab width 8 and always converts to spaces) or we mismatch our configuration. 

---

_Comment by @BurntSushi on 2023-12-05 14:12_

Would it be possible to add a new config knob to the formatter that lets an end user specify their tabwidth? Then it seems like we might be able to get things right here.

---

_Comment by @MichaReiser on 2023-12-05 23:58_

> Would it be possible to add a new config knob to the formatter that lets an end user specify their tabwidth? Then it seems like we might be able to get things right here.

We do have `indent_width` which specifies the width of a tab in spaces AND the number of spaces to use when using `indent_style = space`. The reason for only having one option is that it seemed sufficient for the majority of use cases. Do you propose to have a dedicated `tab_size` setting similar to [editorconfig](https://editorconfig.org/#file-format-details)? How would you use this new setting in combination with Python assuming a tab size of 8 in docstrings?

---

_Comment by @BurntSushi on 2023-12-06 13:01_

I think I probably don't have the full problem paged into context here. From above:

> I think what the formatter should support is that it preserves tabs when using `indent-style=tab` by using `indent`. It means it wouldn't fix mixed indentation in docstrings for you, but it, at least, doesn't result in mixed indentation.
> 
> The alternative is that we always use `indent` (and `align` if it isn't a multiple of tab-width). But I think there we have the issue that Python always assumes a tab-width of 8

I think this is the part I was reacting to. So maybe it would be better to ask what it means when Python assumes a tabwidth of 8 inside of a docstring. Do we _also_ have to follow that assumption? That's the part where I have gaps in my understanding I think.

---

_Comment by @MichaReiser on 2023-12-07 05:38_

That's fair. For reference, there's some more explanation in https://github.com/astral-sh/ruff/issues/8445 about where Python assumes a tab width of 8 (used when inspecting a docstring `.__doc__` at runtime.

---

_Comment by @sciyoshi on 2023-12-11 19:44_

I think it would be safe to change any leading groups of `<indent-width>` consecutive spaces in docstrings to tabs when using `indent-style = "tab"`. This would mean that any indent that is not a multiple of `<indent-width>` may end up with mixed indentation, but this isn't something I believe we need to worry about because Black doesn't support tabs to begin with so there's no compatibility concerns. Any codebases that are using autoformatters with tabs are likely either using Black + `expand`/`unexpand` (which would match my proposed behavior) or other solutions which avoid touching the docstring indentation altogether except for any common indentation. This would be a valid alternative that probably changes less code "in the wild", but I think given that [Ruff formats code inside docstrings](https://github.com/astral-sh/ruff/pull/9003) the former behavior would be simpler and more intuitive.

---

_Comment by @MichaReiser on 2024-02-08 14:14_

# TLDR

IMO, the current behavior that doesn't convert alignment spaces to tabs is correct because it ensures that docstrings are displayed correctly regardless of the used tab width. I agree that the behavior is unexpected when using a docstring-convention like

```python
def test(arg1: str) -> None:
	"""
	Arguments:
		arg1: super duper arg with 2 tabs in front
	"""
```

But that's something the formatter doesn't understand understand today. The solution is to support docstring formatting (with the common convention) so that the formatter can convert spaces in front of `arg1` to tabs. 

That said, I agree that the formatter should not convert tabs to 8 spaces. It should either convert the tab to `indent-width` spaces OR leave the tab unchanged.

I keep the issue open to track this work but remove it from the stable formatter milestone because we want to keep the existing behavior.

# Reasoning

The only a difference between Python <= 2.12 and 3.13 is when inspecting `__doc__` directly which is something that tests might do. `inspect.cleandoc` remains unchanged. 

`inspect.cleandoc` replaces `\t` with 8 spaces. This does not change the indentation-levels but it can change how code is displayed when tabs are used beyond the initial indentation.

While not explicitly documented, the formatter’s `indent-style` configuration only configures whether to use tabs for indentation. Alignment continues to use spaces. This is known as [smart tabs](https://www.emacswiki.org/emacs/SmartTabs). Using smart tabs ensures that code is displayed correctly everywhere, regardless of the configured indent-width and ensures that the formatter doesn’t break any ASCII art.

I believe the expectation mismatch comes from that the whitespace before `arg1` is indentation and not alignment:

```python
def test(arg1: str) -> None:
	"""
	Arguments:
		arg1: super duper arg with 2 tabs in front
	"""
```

Distinguishing the whitespace before `arg1` from any other alignment whitespace requires docstring parsing and formatting which is something we want to support in the future but do not today. 

That’s why I recommend keeping the behaviour as is and the proper fix for it is to support docstring formatting (darglint). 

A possible alternative is to add a configuration option that allows enforcing hard-tabs over soft tabs. I’m reluctant of doing this because it may limit our formatting options in the future, e.g. it would no longer be possible to align the closing `])` with the binary expressions operands:

```python
a 
	+ b
	+ len([
			1, 
			2, 
		])
```

Instead of 

```python
a 
	+ b
	+ len([
			1, 
			2, 
	])
```

## Research

The following program uses tabs for indenting the doctoring as well as alignment inside of the doctoring:

```python
def f():
	"""Computes the value.
	x = 1
		y = x + 1
	z = 2
	
	
	"""
```

A more realistic example:

```python
def test(arg1: str) -> None:
	"""
	Arguments:
		arg1: super duper arg with 2 tabs in front
	"""
```

Where `arg1` is indented by a tab.

### <= 3.12
#### `f.__doc__`
Python preserves the indentation and alignment tabs, as well as the trailing new lines. 

```python
Computes the value.
	x = 1
		y = x + 1
	z = 2
	
	
	

```

```python

	Arguments:
		arg1: super duper arg with 2 tabs in front
	
```

#### `inspect.cleandoc(f.__doc__)`
Removes the indentation whitespace, converts alignment tabs to 8 spaces. 

```
Computes the value.
x = 1
        y = x + 1
z = 2
```

```python
Arguments:
        arg1: super duper arg with 2 tabs in front
```

### >= 3.13
#### `f.__doc__`
Removes indentation tabs, converts alignment tabs to 8 spaces. Preserves trailing whitespace. 
```python
Computes the value.
x = 1
        y = x + 1
z = 2




```

```python

Arguments:
        arg1: super duper arg with 2 tabs in front

```

#### `inspect.cleandoc(f.__doc__)`
The same as for earlier Python versions. It’s similar to `f.__doc__`, but removes trailing newlines. 

```python
Computes the value.
x = 1
        y = x + 1
z = 2
```

```python
Arguments:
        arg1: super duper arg with 2 tabs in front
```

### ASCII Art
Automatically converting multiples of `indent-width` spaces to `tabs` can break the doctoring formatting when the spaces were used for alignment, e.g. in ASCII art:

```python
def test():
	r"""
	Look at this beautiful tree. 
	
	    a
	   / \
	  b   c
	 / \  
	d   e
	"""
```

The example uses spaces for alignment but Ruff would convert the spaces to tabs:

```python
def test():
	r"""
	Look at this beautiful tree. 
	
			a
		 / \
		b   c
	 / \  
	d   e
	"""
```

Printing `test.__doc__` for the version using tabs results in:

```

Look at this beautiful tree. 

                a
         / \
        b   c
 / \  
d   e
      
```

Which is all broken. X


### Editors
* PyCharm uses tabs by default when hitting the `tab` key and setting the indent style to “tabs”. It converts space indents to tabs. 
* PyCharm seems to have some heuristics around when not to convert spaces to tabs. It converts all whitespace to tabs on the initial formatting. It does not convert whitespace to tabs when adding one new argument where the last indent uses spaces only.
* Rustfmt does not normalise whitespace in doc comments to tabs.


### Usages
I find 52 usages of `indent-style` tab on GitHub ([source](https://github.com/search?type=code&auto_enroll=true&q=%22indent-style+%3D+%5C%22tab%5C%22%22+path%3Apyproject.toml+language%3Atoml))


## Resources
* https://github.com/python/cpython/pull/106411
* https://github.com/python/cpython/issues/81283
* https://github.com/astral-sh/ruff/issues/8445
* https://github.com/astral-sh/ruff/issues/8430
* https://www.emacswiki.org/emacs/SmartTabs

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2024-02-08 14:16_

---

_Assigned to @MichaReiser by @MichaReiser on 2024-02-09 16:39_

---

_Closed by @MichaReiser on 2024-02-12 15:09_

---
