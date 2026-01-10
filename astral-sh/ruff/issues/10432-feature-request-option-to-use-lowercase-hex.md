```yaml
number: 10432
title: "Feature request: Option to use lowercase hex literals"
type: issue
state: open
author: DavidBuchanan314
labels:
  - formatter
  - style
assignees: []
created_at: 2024-03-16T22:59:14Z
updated_at: 2025-12-24T02:53:19Z
url: https://github.com/astral-sh/ruff/issues/10432
synced_at: 2026-01-10T11:09:52Z
```

# Feature request: Option to use lowercase hex literals

---

_Issue opened by @DavidBuchanan314 on 2024-03-16 22:59_

For a brief time window, `black` had this feature, before it was reverted https://github.com/psf/black/issues/1692

I'm a big fan of lowercase hex literals, for most of the practical reasons outlined in the above issue, and also for purely aesthetic reasons - uppercase feels shouty.

If I had my way I'd make lowercase the default, but failing that it'd be great to have a configuration option.

---

_Label `formatter` added by @zanieb on 2024-03-17 17:36_

---

_Label `style` added by @zanieb on 2024-03-17 17:36_

---

_Comment by @MichaReiser on 2024-03-18 08:41_

Hi @DavidBuchanan314 

If I understand correctly, this is about formatting hex number literals like `0xEF`? I'm asking because Ruff normalizes hex numbers in string literals to lowercase [Playground](https://play.ruff.rs/57e30c6a-4d5e-4227-818a-d0c9dbe25d87)

My preference would be to change the default to lowercase to make it consistent with hex formatting in strings. However, from reading through Black's issues and discussions on the string hex formatting PRs, I understand that Black changed the preferred casing for hex number literals multiple times and that it's unlikely that they'll change it again in the future. I don't know if I want to deviate from Black's default, considering that the benefits are marginal. 

That means that supporting lowercase hex formatting most likely requires adding a new formatter option. I'm not opposed to this, but we need to make a holistic decision if we want to support formatting-related options or keep Black's opinionated stance to have no-formatting-related settings (or very limited). 




---

_Comment by @DavidBuchanan314 on 2024-03-18 10:16_

Yeah, it's the number literals I'm interested in. As for whether it's actually worth it, I guess I'll leave that to everyone else to decide :)

---

_Comment by @kaddkaka on 2024-03-18 18:22_

To me this kind of configuration is great. It shouldn't affect other formatting and it allows to adopt ruff formatting on an existing codebase that uses either style.

I would also like to propose three values for this kind of config:

- uppercase
- lowercase
- dont_touch (any_case) 

The last option let's the programmer choose case and don't modify it with the formatter - this should make the formatter faster 🐎

---

_Comment by @Avasam on 2024-04-03 16:00_

Piggybacking here a bit, but I was quite surprised when "Ruff format" changed the content of a hex string [^1], and perplexed that the preference for string and number literal is different.

"don't touch" or "off" option is also great to introduce to existing code bases.

[^1]: That was on a project already currently checked with black. Maybe black just didn't detect that case. I know it also doesn't change the value, still surprising nonetheless.

---

_Comment by @DavidBuchanan314 on 2024-08-23 13:02_

I was just thinking about this again, and another argument for lower-case is that the `hex()` built-in produces lower-case hex.

```py
>>> hex(123)
'0x7b'
```

---

_Comment by @PhilipYip1988 on 2024-09-12 01:08_

I was looking for this option too. The formal representation for hexadecimal values in the ipython console is in lower case:

```python
>>> 0xABB4AB8A
2880744330

>>> hex(2880744330)
'0xabb4ab8a'

>>> bin(200)
'0b11001000'
```

At a quick glance, the B and 8 can be mistaken and the 4 and A can also be mistaken. The characters are much more distinct in lower case...

For the exponent lower case e is preferred but black is consistent here:

```python
>>>1E100
1e+100
```
The formal representation of strings also prefers single-quotations, unless a string literal is included:

```python
>>> "hello world!"
'hello world!'

>>>'text = \'hello world!\''
"text = 'hello world!'"
```

The Ruff option quote-style fixes this:

```python
[format]
# Prefer single quotes over double quotes.
quote-style = "single"
```

Lower case f is preferred for a formatted string and black is consistent here:

```python
var = 'world'
f'hello {var}!'
```

The prefix for raw strings are not changed in black because code syntax highlighters differentiate R and r for file paths and regular expressions: 

```python
file_path = R'C:\Windows\System32'
email = r'[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}'
```

Having something similar to the following would be useful:

```python
[format]
# Prefer single quotes over double quotes.
quote-style = 'single'
# Prefer lowercase in hexadecimal and binary.
hex-style = 'lower'
```

If there are other changes that black makes, that differ to the formal representation and the style seen in the official Python Documentation itself, then it might also be convenient for end users if these changes are grouped. For example:

```python
# Match Python's official documentation code style
style='python'
```

```python
# Match black code style
style='black'
```


---

_Comment by @hodunov on 2025-01-28 09:45_

Any updates on this? It really makes sense.

---

_Comment by @avabarnard on 2025-06-17 21:57_

Commenting because @MichaReiser said there needs to be more demand to add this so hopefully you reconsider at some point soon. Came here looking for an option to do exactly what kaddkaka suggested. I'm trying to introduce ruff to an existing codebase and its reformatting the hex in an ugly way. Also, it seems like a strange choice to have the inconsistency in formatting hex numbers inside and outside of string literals but I'm not here to argue with tradition, just want the option.

---

_Comment by @laura240406 on 2025-10-12 01:29_

I'm also trying to switch to ruff but the uppercase hex is a huge issue for me.

---

_Comment by @inno on 2025-11-16 22:33_

@MichaReiser What would constitute "enough demand" in order to get https://github.com/astral-sh/ruff/issues/10432 reopened and merged?

---

_Comment by @MichaReiser on 2025-11-17 07:22_

We don't have any absolute number, but it has to be something with high demand in the community: a feature that is requested frequently by many users. 

---

_Comment by @laura240406 on 2025-12-24 02:53_

What are the downsides of including this? There are multiple people who would benefit from it while I don't see any reason why this would harm the user experience of people who don't need it.

---
