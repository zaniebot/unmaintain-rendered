---
number: 21380
title: Allow one empty newline after class header
type: issue
state: open
author: laymonage
labels:
  - formatter
  - style
assignees: []
created_at: 2025-11-11T14:44:33Z
updated_at: 2025-12-18T20:51:38Z
url: https://github.com/astral-sh/ruff/issues/21380
synced_at: 2026-01-07T13:12:16-06:00
---

# Allow one empty newline after class header

---

_Issue opened by @laymonage on 2025-11-11 14:44_

### Summary

Ref: #8566, https://github.com/astral-sh/ruff/issues/9745#issuecomment-2023056948, https://github.com/astral-sh/ruff/issues/9745#issuecomment-2662810468, https://github.com/astral-sh/ruff/issues/9745#issuecomment-2695173158, https://github.com/astral-sh/ruff/issues/9745#issuecomment-2876498730.

## Description

Black's 2022 style removes empty newlines after code block open. However, as of the [2024 style](https://black.readthedocs.io/en/stable/change_log.html#id52), it now allows one empty first line at the beginning of most blocks (https://github.com/psf/black/pull/3967).

For class headers, Ruff still follows the 2022 style, producing the following result:

### Input file

```py
class Migration(migrations.Migration):

    dependencies = []
```

### black

```py
class Migration(migrations.Migration):

    dependencies = []
```

### ruff

```py
class Migration(migrations.Migration):
    dependencies = []
```

## Use case

For us, this is a blocker when migrating from black to ruff for formatting Django migrations, as it would produce noisy diffs that are otherwise avoidable.

As of #21110 (v0.14.4), one empty newline is now allowed after function headers. I'd like to make a case for class headers as well.

Thanks for all the great work!

### Version

0.14.4

---

_Renamed from "Allow newline after class header" to "Allow one empty newline after class header" by @laymonage on 2025-11-11 14:44_

---

_Referenced in [astral-sh/ruff#20482](../../astral-sh/ruff/issues/20482.md) on 2025-11-11 14:47_

---

_Label `formatter` added by @MichaReiser on 2025-11-11 15:30_

---

_Label `style` added by @MichaReiser on 2025-11-11 15:30_

---

_Comment by @MichaReiser on 2025-11-11 15:33_

It seems the main issue is that Django migration insists on the empty line. Do you know why Django migration adds the empty line? Is this something inherent to the Django code style? Has it previously been discussed to remove the empty line?

---

_Comment by @laymonage on 2025-11-11 15:55_

I'm guessing it's due to [the way the file template is written](https://github.com/django/django/blob/2b0f24e6223bf7e294fba63741f58eb7b0bf49ff/django/db/migrations/writer.py#L315). There are 0-4 attributes added to the class depending on the migration detector, and the template is written in a way such that there's always an empty line in between the attributes (and the class header).

Even if that were to change, that would only affect new migrations. My main concern is when switching from black to ruff, we'll end up producing noisy diffs for the existing migrations. For new migrations, if we already adopted ruff, we could just reformat them before committing the code.

I think what I'd really appreciate is a clear signal from the ruff team whether this is something that will be supported or not. If it is, we'll gladly wait until a new version is released before enabling ruff formatting for the migration files. If it isn't, that's fine, we just have to bite the bullet – reformat the existing migrations and move on.

---

_Comment by @MichaReiser on 2025-11-11 16:00_

> I think what I'd really appreciate is a clear signal from the ruff team whether this is something that will be supported or not. If it is, we'll gladly wait until a new version is released before enabling ruff formatting for the migration files. If it isn't, that's fine, we just have to bite the bullet – reformat the existing migrations and move on.

For me, special casing the formatter because of a single tool and only to reduce the files that were introduced by the formatter seems not a strong enough motivation, especially as it leads to less consistent formatting overall. It's also very likely that we'll run into a similar issue with any future style guide change.

However, I'm open to allowing the empty line if motivated, e.g. by improved readability.

---

_Comment by @laymonage on 2025-11-11 16:38_

That's fair, thanks! I was under the impression that ruff aims to remove [the entirety of this particular deviation from Black](https://docs.astral.sh/ruff/formatter/black/#blank-lines-at-the-start-of-a-block), but if that's not the case, I can see how you'd like to see readability improvements made possible by the change.

Other than the black -> ruff migration pain point for Django migrations (pun intended), I don't have any strong feelings for this, so I can't give an example.

I guess there was a strong case to be made for functions and methods, since not having the empty line can hurt readability when there are type annotations. There isn't any for classes, and newline is already preserved if there is a docstring or comment.

I'll leave this open in case other people would chime in since the previous issue was closed, but feel free to close it if there's no activity or if you'd rather have it closed.

---

_Comment by @pekkaklarck on 2025-12-17 10:30_

For our project the removal of the empty line between the class definition and the first method is the only reason to keep using Black. There are various reasons why we prefer
```python
class Example:

    def method(self);
        ...

    def m2(self):
        ...
```
over
```python
class Example:
    def method(self);
        ...

    def m2(self):
        ...
```
and I list the most important ones here:

1. Such change creates unnecessary churn that is annoying especially in bigger projects.
2. It's inconsistent not to have an empty line before the first method in the above case, but to have one if the class has a docstring or attributes.
3. PEP 8 [explicitly states](https://peps.python.org/pep-0008/#blank-lines) this:
    > Method definitions inside a class are *surrounded* by a single blank line.

    Removing the empty line is a clear violation of this rule.

In my opinion auto-formatters should actually *add* an empty line before all method definitions if there isn't one. I understand such a change would cause code churn, though, and preserving empty lines, but not adding them, like Black does is probably a better idea. Ultimately I'd prefer things like this to be configurable, but that's a totally different discussion.

---

_Comment by @amyreese on 2025-12-17 18:43_

FWIW Ruff will preserve an empty line if the class definition contains a docstring like this:

```py
class Foo:
	"""something"""

	def foo(self):
		pass
```

---

_Referenced in [astral-sh/ruff#22237](../../astral-sh/ruff/issues/22237.md) on 2025-12-29 02:08_

---
