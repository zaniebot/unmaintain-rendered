```yaml
number: 7684
title: "Formatter: Keep right-hanging comments aligned"
type: issue
state: open
author: warsaw
labels:
  - formatter
  - style
assignees: []
created_at: 2023-09-27T23:50:36Z
updated_at: 2025-06-28T01:17:46Z
url: https://github.com/astral-sh/ruff/issues/7684
synced_at: 2026-01-12T15:54:47Z
```

# Formatter: Keep right-hanging comments aligned

---

_@warsaw_

`ruff format` follows `black`'s style for right-hanging comments (i.e. inline comments), but that leads to poorly readable results, especially when a series of consecutive lines uses comment alignment by column.  Blue fixes this readability problem.

Let's start with this file:

```python
def foo():
    x = 1                                         # this comment
    why = 2                                       # aligns with this comment
    zebra = 3                                     # and this one
```

As you can see, every `#` on each of these lines appears at the same column.  How does ruff reformat the line?

```python
def foo():
    x = 1  # this comment
    why = 2  # aligns with this comment
    zebra = 3  # and this one
```

This is the way black does it, i.e. by jamming all comment starts to two space characters between the last code character and the `#`.  This destroys the alignment.

How does blue format the file?  Nothing changes!  Blue preserves the number of spaces between the last code character and the `#` with the assumption that the spacing is deliberate and done for readability.

`ruff format` should follow `blue`'s rule here.

---

_Label `formatter` added by @MichaReiser on 2023-09-28 06:58_

---

_Label `wish` added by @MichaReiser on 2023-09-28 06:58_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-09-28 06:58_

---

_Removed from milestone `Formatter: Stable` by @MichaReiser on 2023-09-28 06:58_

---

_Comment by @warsaw on 2023-10-03 02:08_

Now that the quote handling (#7310) has been fixed in 0.0.292, this is the last formatting incompatibility that I've been able to find between `ruff` and `blue`.  Once this ticket is resolved I can officially start promoting `ruff` as a replacement for `blue`.  Great work y'all and thank you!

---

_Comment by @zanieb on 2023-10-03 02:09_

I have also been annoyed with this when using Black. I'm not sure if there's a downside to supporting this. Ideally it would only apply if they start out aligned. Additionally, it'd be cool if it shrunk to the minimum compatible distance for alignment — but that's a little magical.

---

_Comment by @charliermarsh on 2023-10-03 02:26_

Enforcing this conditionally seems really hard... and enforcing this globally by default would be a big deviation from Black. I suspect it would need to be a global setting, if we choose to support it.


---

_Comment by @JelleZijlstra on 2023-10-03 04:16_

For what it's worth, I have consistently rejected this sort of feature in Black, as I don't think it leads to a consistent, stable formatting style. Obviously, what style Ruff chooses is up to you.

Barry's proposed rule is that the number of spaces before the hash gets preserved. But what if someone comes along and decides that zebras are worth more:

```python
def foo():
    x = 1                                         # this comment
    why = 2                                       # aligns with this comment
    zebra = 31                                     # and this one
```

Now either the code gets misaligned (ugly), or they have to manually fiddle with the spaces, which defeats the point of an autoformatter.

Now, possibly the formatter could be made smart enough to detect that the comments are misaligned, and fix the spacing on the zebra line to get them aligned again.

But now what if we decide that zebras are worth a lot more:

```python
def foo():
    x = 1                                         # this comment
    why = 2                                       # aligns with this comment
    zebra = 300000000000000000000000000000000000000  # and this one
```
Then, to get the lines aligned again, the formatter would have to change the `x` and `why` line, creating noise in the diff.

---

_Comment by @warsaw on 2023-10-03 20:27_

There's a little bit of missing context that might be helpful.  In [python-mode](https://gitlab.com/python-mode-devs/python-mode/-/tree/master) you can set a comment column, and then hit `M-;` (I'm not sure if that's a default keybinding, but it's set to `comment-dwim` for me) and Emacs will do it's best to ensure that hanging comment starts at that column.  Of course in @JelleZijlstra 's second example, it will put a minimum number of spaces there but they won't align.

So @JelleZijlstra 's first example, it's relatively easy, albeit manual, to re-indent everything to an aligning column.  Emacs only looks at the current, line so it doesn't actively try to keep things lined up, but the effect is usually that they are.  The preservation of whitespace rule that `blue` implements works well with this approach.

It's never going to be perfect, but I'd argue that `blue`'s rule is better than the step-wise always-2-spaces rule that `black` (and currently `ruff`) implements because that leads to difficult to read lines.  It's worse with `black` though because you don't even have the option of manually aligning those hanging comments -- black will just crush them.

---

_Comment by @zanieb on 2023-10-03 20:32_

Hm... a `comment-column` setting is an interesting approach. Overall this sounds pretty difficult to implement and I agree that manually fiddling with the alignment would be a disappointing outcome. I appreciate all of the extra context!

---

_Comment by @konstin on 2023-10-04 12:01_

I'd prefer not to support this, i think comments directly after the statements or alternatively leading own line comments (where they are column aligned) are fine enough to read. For the the case of not modifying whitespace i agree with @JelleZijlstra that would defeat the point of an autoformatter while for the case of column aligning i don't think the possible improvement warrants introducing a column layout when we don't use one elsewhere¹.

This is also bad from a point of locality, currently the formatting of one statement does not influence the formatting of other statements (we see those comments as trailing on the statement).

---
¹With column layout, i mean e.g.
```python
class Foo:
    x     = 1                  # this comment
    why   = 22                 # aligns with this comment
    zebra = 333                # and this one
```
    

---

_Comment by @warsaw on 2023-10-04 15:29_

> Hm... a comment-column setting is an interesting approach

Thinking about this overnight, this could be the best way forward.  It would be a local-to-the-line only setting -- no need for global parsing or alignment.  It also, um, aligns more with the Emacs behavior.  `ruff` would do its best to preserve `comment-column` but in the "big zebra" case above, it would just push the hanging comment out to the right.  That's fine because the user would still have a lot of flexibility in getting the format they want.

Straw man proposal:

* `comment-column = 60` - best effort to align the hanging comment `#` on column 60, the Emacs behavior
* `comment-column = +2` - a relative setting, i.e. the current black/ruff default
* `comment-column = false` - preserve existing whitespace, i.e. the `blue` behavior


---

_Comment by @effigies on 2023-10-08 01:20_

The top two reasons I use blue and not black are single quotes and not messing with comment spacing. I don't mind bumping the spaces to 2, but removing additional spaces is a real pain. If I'm using more, there's pretty much always a reason.

Here's an example that even blue currently damages (blue fixed the single-line expression case). These are pairs of field names and numpy dtype codes, followed by comments:

```diff
 header_dtd = [
-    ('sizeof_hdr', 'i4'),      # 0; must be 348
-    ('data_type', 'S10'),      # 4; unused
-    ('db_name', 'S18'),        # 14; unused
-    ('extents', 'i4'),         # 32; unused
-    ('session_error', 'i2'),   # 36; unused
-    ('regular', 'S1'),         # 38; unused
-    ('dim_info', 'u1'),        # 39; MRI slice ordering code
-    ('dim', 'i2', (8,)),       # 40; data array dimensions
+    ('sizeof_hdr', 'i4'),  # 0; must be 348
+    ('data_type', 'S10'),  # 4; unused
+    ('db_name', 'S18'),  # 14; unused
+    ('extents', 'i4'),  # 32; unused
+    ('session_error', 'i2'),  # 36; unused
+    ('regular', 'S1'),  # 38; unused
+    ('dim_info', 'u1'),  # 39; MRI slice ordering code
+    ('dim', 'i2', (8,)),  # 40; data array dimensions
 ]
```

I think it's hard to argue that this change makes things more readable. I understand that preserving an aligned column is difficult, and I really don't care if an autoformatter chooses not to try. But if I go through the trouble of making an aligned column of comments, it's frustrating for an autoformatter to destroy it and give no option to just leave it alone. The PR I opened for blue about this (https://github.com/grantjenks/blue/pull/83) still enforces at least two spaces, but otherwise leaves inline comment spacing alone.

---

_Comment by @dhirschfeld on 2023-10-15 23:33_

Destroying carefully crafted comment spacing can make code harder to parse/understand which in turn can make it easier for bugs to creep in.

Some people don't use comment spacing to make their code easier to visually parse and for them keeping the option of enforcing 2-spaces is fine. For those who do use spacing it would be nice to have the option to at least turn off any messing with comment spacing at all. I'd much rather manually align my comments (or not) than have an auto-formatter make my code harder to understand and maintain.

The ability to easily parse the code and therefore more easily spot bugs is also at the core of [Jarrod's request](https://github.com/astral-sh/ruff/discussions/7310#discussioncomment-7266904) to not mess with `numpy` spacing (which I'm also interested in, for the same reasons).

I'd be fine with switches such as `--no-align-comments`, `--no-align-numpy` to globally turn off autoformatting for those use cases.

---

_Comment by @MichaReiser on 2023-10-16 01:27_

How frequent do you use these carefully crafted comments? IMO, disabling formatting might be the best option if they are rare. 

Considering:

```python
header_dtd = [
    ('sizeof_hdr', 'i4'),      # 0; must be 348
    ('data_type', 'S10'),      # 4; unused
    ('db_name', 'S18'),        # 14; unused
    ('extents', 'i4'),         # 32; unused
    ('session_error', 'i2'),   # 36; unused
    ('regular', 'S1'),         # 38; unused
    ('dim_info', 'u1'),        # 39; MRI slice ordering code
    ('dim', 'i2', (8,)),       # 40; data array dimensions
 ]
```

It's unclear how the formatter should format the comments once a line exceeds the configured line width

```python
header_dtd = [
    ('sizeof_hdr', 'i4'),      # 0; must be 348
    ('data_type', 'S10'),      # 4; unused
    ('db_name', 'S18'),        # 14; unused
    ('extents', 'i4'),         # 32; unused
    ('session_error', 'i2'),   # 36; unused
    ('regular', 'S1'),         # 38; unused
    ('dim_info', 'u1'),        # 39; MRI slice ordering code
    (
		'a very long header that exceeds the line width so that it breaks', 'i2', (8,)
	),       # 40; data array dimensions
 ]
```



---

_Comment by @dhirschfeld on 2023-10-16 01:50_

> How frequent do you use these carefully crafted comments? IMO, disabling formatting might be the best option if they are rare.

For me, they're rare enough that disabling formatting on a case-by-case basis is ok. I guess there's a threshold where too many `# nofmt` comments gets distracting and a global config would be preferred.

---

_Comment by @effigies on 2023-10-16 02:15_

Yes, I can disable formatting for these structures, but there are nice features of formatting, such as uniformizing indentations and quote style, and it would be nice not to have to throw out the baby with the bathwater.

To be clear, I do not want the formatter to calculate the correct columns. Bumping 0/1 spaces to 2 is a completely fine rule to apply, but otherwise I just want them left alone. If I had indicated to the formatter to leave comments alone, I would expect the tuple to get reformatted and the comment spacing kept the same. If I then decided to adjust the spacing before the comment, I would expect the formatter to leave the new spacing alone.

FWIW, if your situation occurred, I would probably reformat it to:

```python
header_dtd = [
    ('sizeof_hdr', 'i4'),      # 0; must be 348
    ...,
    (
        'a very long header that exceeds the line width so that it breaks',
        'i2',
        (8,),
    ),                         # 40; data array dimensions
 ]
```

Just as a meta-comment, it really is odd to me that black et al have decided that using spacing in comments to improve readability needs to be done away with, like arguments over tabs vs spaces or where to put newlines in parameter lists. Comments are the one bit of code whose only purpose is communication with other humans (okay, modulo `pragma:` and `fmt:` and so on), so it strikes me as particularly funny that everybody seems to be saying "We can't come up with an algorithm to make these look good all the time, so let's prevent humans from doing it themselves."

---

_Comment by @warsaw on 2023-10-16 18:58_

>  it strikes me as particularly funny that everybody seems to be saying "We can't come up with an algorithm to make these look good all the time, so let's prevent humans from doing it themselves."

💯 

What do you think about my [previous suggestion](https://github.com/astral-sh/ruff/issues/7684#issuecomment-1747129718)?  I think it covers all the use cases.

---

_Comment by @effigies on 2023-10-16 19:05_

> What do you think about my [previous suggestion](https://github.com/astral-sh/ruff/issues/7684#issuecomment-1747129718)? I think it covers all the use cases.

Yes, that would work fine for me.

---

_Comment by @MichaReiser on 2023-10-17 00:54_

> it strikes me as particularly funny that everybody seems to be saying "We can't come up with an algorithm to make these look good all the time, so let's prevent humans from doing it themselves."

We want to support the best possible comment formatting that matches users' intuitions. It's just that comment formatting is surprisingly hard.I would estimate that about 30% of the development effort of our formatter is related to handling comments as best as we can. Comments are hard because they can appear in any position, there's no formal definition of what a comment comments, the output must be stable (on the first try), the formatter must support all possible comment placements, and misplacing comments can easily lead to syntax errors. 

> What do you think about my https://github.com/astral-sh/ruff/issues/7684#issuecomment-1747129718? I think it covers all the use cases.

Assuming we align all comments at, e.g. column 60. What happens if the comment has a width of 29, exceeding the line width by 1? For example, let's pretend the comment below is aligned at column 60 and does exceed the line width.

```python
a = [1, 2]           # comment that exceeds the line width when aligned to column 60
```

The comment would fit fine if it isn't aligned at column 60. What's your expected behavior? 

---

_Comment by @warsaw on 2023-10-17 02:09_

> The comment would fit fine if it isn't aligned at column 60. What's your expected behavior?

Speaking just for myself, I would expect that `ruff format` would leave the comment alone, but `ruff check` would complain.  I actually think it's an anti-pattern to have long hanging comments, and would personally rewrite such comments as a separate block preceding the code.  My personal preference is that hanging comments usually end up being pragmas or type checker hints, although I do occasionally have short, lined-up "blocks" like the examples above.

Aside: I wish there was a better way to signal intent to static checkers, rather than pragma/type comments.

---

_Comment by @effigies on 2023-10-17 02:40_

> I would expect that ruff format would leave the comment alone, but ruff check would complain.

Yes. There are plenty of ruff checks that are not auto-fixable, and that hasn't been an existential problem. If the problem is this difficult, the obvious solution is to work on something that is tractable.

Clearly some people like black's behavior, so I am not saying do not include it. Just let it be optional, please. Or you can go further and allow some other specific behaviors, as Barry suggests.

Anyway, I feel like I've taken up enough space in this thread. I like ruff, I like autoformatting. Thanks for all the effort, whatever the end result of this conversation is.

---

_Label `wish` removed by @MichaReiser on 2023-10-27 02:08_

---

_Label `style` added by @MichaReiser on 2023-10-27 02:08_

---

_Comment by @toppk on 2023-11-08 05:54_

if we're still in the spit-balling ideas phase, I'd like to use a double hash `##` for @warsaw `comment-column = false` behavior (in addition to, not as a replacement) .   For me it's only a few spots where I use comment columns, and I'd prefer having something easy enough to remember, not a prama or conf setting.  And I'm happy that ruff format is fixing the whitespace in 99% of cases.

fwiw, I came up with this as the most pleasant to my eyes workaround

```python
class Foo(Enum):
    AAA = -2  #      > for the a's
    BBBBBBBB = 0  #  > NOT USED
    CCCC = 1  #      > only when you need sea
```

---

_Comment by @warsaw on 2023-11-08 19:37_

Not sure I would use "most pleasant" over "least horrible" but I get your point!  😆 

I suppose you could do this too:

```python
class Foo(Enum):
    AAA = -2  #      # for the a's
    BBBBBBBB = 0  #  # NOT USED
    CCCC = 1  #      # only when you need sea
```

which *almost* looks right!

---

_Comment by @toppk on 2023-11-12 02:36_

> Not sure I would use "most pleasant" over "least horrible" but I get your point! 😆
> 
> which _almost_ looks right!

that was too uncanny valley for me 🤪

---

_Comment by @nuno-andre on 2023-11-28 08:33_

I would generalize this issue as vertical alignment (colons, assignments, and comments), including cases such as:

```python3
@dataclass
class Example:
    an_attr:      int
    another_attr: str | None
    whatever:     dict[str, str]


class Colors(IntEnum):
    BLACK  = 0  # a comment
    RED    = 1  # another comment
    YELLOW = 2
```

I use a VSCode extension for vertical alignment, [Better Align](https://github.com/chouzz/vscode-better-align), which has a very convenient configuration method.

So, the above formatting would be: 

```toml
colon = [ -1, 1 ]
assignment = [ 1, 1 ]
comment = 2
```



---

_Comment by @randolf-scholz on 2024-01-11 13:59_

The real solution to this problem are [elastic tabstops](https://nickgravgaard.com/elastic-tabstops/), which would make this problem disappear overnight by using a single variable length tab for separation. But as long as editors are stuck in the 60s by interpreting tabs always as `1 + col % tab_length` spaces, there is no good solution.

---

_Comment by @warsaw on 2024-01-11 17:21_

Pretty interesting read, but I'm not sure how this helps this particular issue in practice, given there are probably more editors in use out there than lawyers (or more relevant perhaps - coders?) with opinions 🤣 .

---

_Comment by @mcrumiller on 2024-03-20 15:00_

New to the party this one really bugs me. I have so many cases where I have hanging aligned comments:

```python
coefficients = [
   a + b + c,     # c0 - used for X
   a * c,         # c1 - used for Y
   1 /2 + d,      # c2 - used for Z
]
```
Am I correct reading through the issue that there is no way to add some type of exclusion to prevent inline comments from being excluded from formatting?

---

_Comment by @rotten on 2024-08-15 12:55_

I'm curious if there are any new thoughts or progress on this?   Is it likely to have a solution in the near future or far future?  Debating ruff for a new project instead of blue.  I'm undecided on whether to make the jump.

---

_Comment by @mcrumiller on 2024-08-15 13:00_

I'm of the opinion that comments, including the whitespace leading up to them, should not be touched by the formatter. All of the workarounds supplied in this thread lead to really ugly comment code. This one issue is my current biggest gripe with auto formatters and I find myself smattering `# fmt: skip` everywhere to deal with this issue.

---

_Comment by @warsaw on 2024-08-15 15:07_

> Debating ruff for a new project instead of blue. I'm undecided on whether to make the jump.

As one of the original maintainers of blue, I'll say that despite this issue still being unresolved, I've already made the jump despite the occasional `# fmt` toggle.   Still, I'd love to get an ETA on this ticket.

---

_Comment by @ncoghlan on 2024-10-18 18:05_

> I find myself smattering # fmt: skip everywhere to deal with this issue.

Is there a place to put the `# fmt: skip` to deal with this in a `dataclass` or `TypedDict` body?

I'm having to resort to a `# fmt: off`/`# fmt: on` pair for those, and it doesn't look nice (especially for small classes with only a few fields)

(It's also somewhat annoying that  `# fmt: off (preserve the trailing comment alignment)` doesn't work, since the additional trailing text prevents `ruff` from recognising the directive)


---

_Comment by @MichaReiser on 2024-10-18 18:49_

Would you mind sharing a concrete example. It would help me understand what you're running into

---

_Comment by @b-destoop on 2024-11-13 12:43_

I actually do this as shown below. It is far from perfect, and if the length of any of the characters before the # changes, the comment has to be updated manually, but as long as I am working somewhere else in the file (and auto-formatting along the way), nothing is changed.

def foo():
    x = 1  #               this comment
    why = 2  #          aligns with this comment
    zebra = 3  #        and this one

---

_Comment by @ncoghlan on 2024-11-14 05:56_

@MichaReiser If you were asking about my dataclass/TypeDict comment, https://github.com/lmstudio-ai/venvstacks/blob/7877f743fd3df4d0d97c81d2f637f64ab73f1c34/src/venvstacks/stacks.py#L177 is one of the simpler cases:

```python
class EnvironmentLockMetadata(TypedDict):
    """Details of the last time this environment was locked."""

    # fmt: off
    locked_at: str          # ISO formatted date/time value
    requirements_hash: str  # Uses "algorithm:hexdigest" format
    lock_version: int       # Auto-incremented from previous lock metadata
    # fmt: on
```

Putting `# fmt: skip` in the header line after the trailing `:` didn't turn off the automatic formatting in the class body - an `off`/`on` pair (as shown) seems to be the only way to do that.

For the original feature request here, I wonder if a special trailing comment alignment directive could solve the ambiguity problem:

```python
class EnvironmentLockMetadata(TypedDict):
    """Details of the last time this environment was locked."""

    # fmt:                  # align comments
    locked_at: str          # ISO formatted date/time value
    requirements_hash: str  # Uses "algorithm:hexdigest" format
    lock_version: int       # Auto-incremented from previous lock metadata
```

Until the next blank line, trailing comments would be aligned with the trailing comment on the `# fmt: ...` line.

The trailing comment alignment behaviour could then be specified as:

* `# align comments`: comments are pushed right if necessary to get a two space gap after the longest line, but never pulled left  (keeps diffs as small as possible without losing trailing comment alignment)
* `# align +2`: comments are always aligned with a two space gap between the end of the longest line and its trailing comment (moves trailing comments as far left as possible without losing alignment, at the expense of sometimes getting noisier diffs)


---

_Comment by @ncoghlan on 2025-01-06 01:29_

The topic of [aligned assignments](https://discuss.python.org/t/pep-8-more-nuanced-alignment-guidance/76078/) came up again on discuss.python.org.

Considering the "alignment header" idea in that light, I would revise my suggestion to be:

```python
# fmt: [= align][# align [+N]]
```

* If `= align` is present, assignments have their `=` aligned with the one in the formatting header (and the formatter complains if the assignment targets don't all fit)
* If `# align` is present, trailing comments have their leading `#` aligned with the one in the formatting header (and the formatter complains if the non-comment parts of the lines don't all fit)
* If `# align +N` is present, trailing comments, including the one in the formatting header, have their leading `#` aligned to `N` spaces after the end of the longest line

The column formatting would apply until the next blank line.

Edit: Alternatively, if there needs to be a directive name up front for the formatter to make sense of the header comment:

```python
# fmt: align [= (...|+N)][# (...|+N)]
```

* omitting both alignment fields is equivalent to specifying `# fmt: align  # +2`
* If `= ...` is present, assignments have their `=` aligned with the one in the formatting header (and the formatter complains if the assignment targets don't all fit). For multiple assignment targets in a line, the last one before the assigned value is the one that gets aligned.
* If `= +N` is present, assignments, including the one in the formatting header, have their last `=` aligned to `N` spaces after the end of the longest assignment target. `# fmt: align` is NOT considered an assignment target for purposes of this calculation, so the formatting header may not fully align if all the assignment targets are shorter than 12 characters.
* If `# ...` is present, trailing comments have their leading `#` aligned with the one in the formatting header (and the formatter complains if the non-comment parts of the lines don't all fit)
* If `# +N` is present, trailing comments, including the one in the formatting header, have their leading `#` aligned to `N` spaces after the end of the longest line. `# fmt: align`, `# fmt: align = ...` and `# fmt: align = +N ` are NOT considered lines for purposes of this calculation, so the formatting header may not fully align if all the other lines in the block are shorter than that.


---

_Comment by @warsaw on 2025-05-10 02:15_

Friendly ping on this issue to see if you've thought about it any more.  Having just gone through a ruff (pun intended) pass on a bunch of code, this really is the last thing that bugs me.

> > Hm... a comment-column setting is an interesting approach
> 
> Thinking about this overnight, this could be the best way forward. It would be a local-to-the-line only setting -- no need for global parsing or alignment. It also, um, aligns more with the Emacs behavior. `ruff` would do its best to preserve `comment-column` but in the "big zebra" case above, it would just push the hanging comment out to the right. That's fine because the user would still have a lot of flexibility in getting the format they want.
> 
> Straw man proposal:
> 
> * `comment-column = 60` - best effort to align the hanging comment `#` on column 60, the Emacs behavior
> * `comment-column = +2` - a relative setting, i.e. the current black/ruff default
> * `comment-column = false` - preserve existing whitespace, i.e. the `blue` behavior

I still think something like this is the best compromise.  It won't ever be perfect, but it doesn't need to be.  Would it be difficult to implement this and put it behind a `preview` or `experimental` flag?

---

_Comment by @MichaReiser on 2025-05-10 10:16_

No, we've been busy with ty

---

_Comment by @pirate on 2025-05-15 22:40_

I think implementing these changes gradually is the best approach, just start with a simple global flag like `"ignore-excess-whitespace-before-trailing-comments"` that leaves any comments with >2 spaces in front alone. In this mode it's on the developer to manually re-align comments when new long lines are added. This will satisfy 90% of people landing on this issue from google, looking for a way to stop `ruff format` from messing up their hand-aligned comments. Devs can use a multitude of editor plugins that are available to help re-align when new long lines are added.

Then add separate file-level/block-level auto-aligning `# fmt: align ...` flags later after more design discussion. I'd even consider splitting this issue into two separate issues because these really are separate features.

---

_Comment by @pirate on 2025-06-19 01:27_

> start with a simple global flag like "ignore-excess-whitespace-before-trailing-comments" that leaves any comments with >2 spaces in front alone

would you be open to me contributing a PR for this simple initial approach @MichaReiser?

do you have a preference on what to name the config option?

---

_Comment by @MichaReiser on 2025-06-19 09:15_

> start with a simple global flag like "ignore-excess-whitespace-before-trailing-comments" that leaves any comments with >2 spaces in front alone

I'm not convinced that this results in a great experience if it applies to all comments, because it will always mess up the comment alignment if the formatter reflows your code. But I do like the simplicity

---

_Comment by @warsaw on 2025-06-19 16:27_

> I'm not convinced that this results in a great experience if it applies to all comments, because it will always mess up the comment alignment if the formatter reflows your code. But I do like the simplicity

I'm not sure the simplicity is worth it, if it doesn't provide what users want.  I still think the best proposal is basically what @ncoghlan has suggested, but to be explicit:

* A configuration variable `comment-align` (name to be bikeshed), with values:
* `+2` - current/default behavior; two spaces between the last non-whitespace character and the `#`
* `+N` - same as above, but with `N` spaces
* `preserve` - preserve the same number of whitespace characters between the last non-whitespace character and the `#`, even if it causes misaligned `#`; the user can always manually fix it.
* `N` - put the `#` in column `N`.  If the last non-whitespace character is past column `N`, fall back to `+2`.   This acts like a tab stop.  Negative numbers are not allowed.
* For bonus, a `fmt: comment-align <setting>` before a commented block of code would temporarily adjust the choice for that block.  `fmt: off` restores the configured settings.

I think this will cover all the bases, and the alignment algorithm probably isn't that hard to implement.  I'm not so sure about the bonus.

---

_Comment by @MichaReiser on 2025-06-19 16:37_

I think what's needed here before any implementation is that someone drives a design proposal that has a rough consensus. 

---

_Comment by @pirate on 2025-06-19 20:18_

Sure but that all sounds much more complicated and likely to delay this feature by quite a bit.

For me personally having no option to disable it is a daily source of pain, I almost want to disable ruff entirely because it's making nearly every inline comment harder to read by mixing it in with all the code on the left.

Seems simpler to start with a flag to disable it and then add all those other options gradually? I think users in my situation who opt to *disable* leading whitespace removal are perfectly happy reformatting our own comments if the formatter breaks alignment. The people who really want it to handle auto-re-aligning are a separate ven diagram circle, which is why I was encouraging splitting this feature up.

---

_Comment by @mcrumiller on 2025-06-19 20:36_

> I very much do not want the formatter trying to auto-align my comments, I just want it to leave them alone I will handle my own formatting of comments.

I agree. I do not want my comments to be forced to be two spaces after the content, I want my comments to be where I put them.

---

_Comment by @warsaw on 2025-06-20 17:12_

For me, [`fmt: off`](https://docs.astral.sh/ruff/formatter/#format-suppression) blocks are sufficient to disable right hanging comment reformatting in the places where it's the most egregious, blocks of lined up comments.   I generally avoid (and [discourage](https://barry.warsaw.us/software/STYLEGUIDE.txt)) most uses of right hanging comments.  With `ruff` and `black` when I *do* have to use them, I just accept my fate.

OTOH, I implemented [blue's default behavior](https://blue.readthedocs.io/en/latest/) of just preserving the whitespace preceding the `#` for right hanging comments, and it can be Good Enough.  Maybe this is a case where [perfect is the enemy of the good](https://en.wikipedia.org/wiki/Perfect_is_the_enemy_of_good).

---

_Comment by @warsaw on 2025-06-20 20:47_

> OTOH, I implemented [blue's default behavior](https://blue.readthedocs.io/en/latest/) of just preserving the whitespace preceding the `#` for right hanging comments, and it can be Good Enough. Maybe this is a case where [perfect is the enemy of the good](https://en.wikipedia.org/wiki/Perfect_is_the_enemy_of_good).

BTW, if we agree that this is good enough, then we can still implement [my suggestion here](https://github.com/astral-sh/ruff/issues/7684#issuecomment-2988640933), although I would recommend only supporting the `+2` and `preserve` options for now.  `+2` of course being the current `ruff` behavior (and would be the default for backward compatibility), while `preserve` would be the `blue` behavior, i.e. just keep the same number of whitespace before the `#`.



---

_Comment by @mcrumiller on 2025-06-27 12:59_

> I generally avoid (and [discourage](https://barry.warsaw.us/software/STYLEGUIDE.txt)) most uses of right hanging comments

There really is no better way to comment e.g. large dictionaries or lists though.

```python
patient_codes = [
   "ABC",    # patient is healthy
   "CD",     # patient has terminal diagnosis
   "X",      # patient is deceased
   "AAA01",  # patient is pending
   "AAA02",  # patient is pre-processing
]
```
...etc. Try reading that versus what ruff does:

```python
patient_codes = [
   "ABC",  # patient is healthy
   "CD",  # patient has terminal diagnosis
   "X",  # patient is deceased
   "AAA01",  # patient is pending
   "AAA02",  # patient is pre-processing
]
```


---

_Comment by @MichaReiser on 2025-06-27 13:48_

Can we focus the discussion on a design proposal for how to solve this rather than keep arguing which comment style is the right one because it seems unlikely that everyone will ever agree on this ;)

---

_Comment by @warsaw on 2025-06-27 18:48_

> Can we focus the discussion on a design proposal for how to solve this rather than keep arguing which comment style is the right one because it seems unlikely that everyone will ever agree on this ;)

Hi @MichaReiser - I'm curious about the level of detail you're looking for and the "forum" for making such a design proposal.  Let me summarize my proposal distilled from [this post](https://github.com/astral-sh/ruff/issues/7684#issuecomment-2988640933) and [this one](https://github.com/astral-sh/ruff/issues/7684#issuecomment-2992814992).

* Add a new configuration setting called `comment-alignment`
* `comment-alignment` accepts two values currently (with room for expansion in the future): `+2` and `preserve`
* `tool.ruff.format.comment-alignment = "+2"` is exactly the current behavior and is the default
* `tool.ruff.format.comment-alignment = "preserve"` keeps exactly the current whitespace preceding the comment character.  This solves the @mcrumiller example without "step stairing" table-ish alignment you've already meticulously written.  If the author changes, say a key value to be longer, then it's up to them to adjust the `#` alignments manually.  At least this way, `ruff` doesn't make anything worse.

I think this is a simple approach that's easily explained and probably easy to implement.  `+2` is what `black` does, `preserve` is what `blue` does.  Those two simple cases should probably cover 95% of what people want.

---

_Comment by @mcrumiller on 2025-06-27 18:51_

@warsaw that sounds like a very reasonable approach and I fully support it.

---

_Comment by @MichaReiser on 2025-06-27 21:10_

> Hi @MichaReiser - I'm curious about the level of detail you're looking for and the "forum" for making such a design proposal. Let me summarize my proposal distilled from https://github.com/astral-sh/ruff/issues/7684#issuecomment-2988640933 and https://github.com/astral-sh/ruff/issues/7684#issuecomment-2992814992.

I'd expect some research on how other formatters (including those from other ecosystem) handle this. 

I did see your previous comment and I'm still not convinced that a preserve option gives a great experience and I'd consider this a last resort option (see https://github.com/astral-sh/ruff/issues/7684#issuecomment-2987354988)

I don't see any value in posting the same proposal multiple times.

---

_Comment by @warsaw on 2025-06-27 21:37_

> I'd expect some research on how other formatters (including those from other ecosystem) handle this.

For Python, we have at least `black` and `blue` as prior art.  I know for a fact (albeit anecdotally) that people switched to `blue` because of this one difference.

I don't have enough experience with other language ecosystem formatters to add anything relevant there.

---

_Comment by @pirate on 2025-06-27 23:33_

It's important to acknowledge "what users what" is not a monolithic thing, there are two separate groups, the people that want formatters to leave their comments alone, and the people that want fomratters to handle auto-formatting comments. Both groups are large enough that it justifies adding flags to support either option, but I'd wager at least $50 the first group is larger than the second one. The first group is also easier to satisfy with a simple "dont remove leading whitespace before comments" flag, satisfying the second group requires a deeper design discusssion.

---

_Comment by @zanieb on 2025-06-27 23:49_

On black vs blue, it is worth noting that there are ~70m downloads of black per month and ~86k downloads of blue per month — a 1000x difference. I don't mean to disparage blue, but I don't think it's a stellar example of the community at large demonstrating that preserving comment positioning is essential. yapf is actually much higher at 14m downloads per month, and  has a [spaces before comment](https://github.com/google/yapf?tab=readme-ov-file#spaces_before_comment) option, which may make it more relevant for understanding other designs.

As Micha noted, it does seem reasonable to do some research on how other ecosystems handle this to inform a design since Python is mostly dominated by black.


---

_Comment by @warsaw on 2025-06-28 00:50_

Downloads alone aren't the whole story, mostly because blue is (was) a fork of black and never advertised nearly as widely.  It was also difficult to keep it updated as black evolved, due to the monkey patching nature of blue.  Also, well, ruff was good enough in every other respect (and solved the other [biggest complaint about black](https://docs.astral.sh/ruff/settings/#format_quote-style)) that we lost all motivation to continue to support blue.  🥳

> yapf is actually much higher at 14m downloads per month, and has a [spaces before comment](https://github.com/google/yapf?tab=readme-ov-file#spaces_before_comment) option, which may make it more relevant for understanding other designs.

The single value setting looks more like my `+N` suggestion.  The multi-value setting is pretty interesting in that it essentially represents tab stops and isn't directly part of my earlier, more complex suggestion.  The setting syntax I propose could support this in the future.

Neither seems to be "I know what I'm doing, please leave my spaces alone", which is what I think we're really requesting.

Personal note: We could never get yapf to "work right" on our code bases.  Perhaps if we had, we would have never created blue in the first place.

---

_Comment by @MeGaGiGaGon on 2025-06-28 01:17_

My two cents is that trying to guess how important alignment of specific features is should not land on the formatter, or should be build into a formatter more universally.

Code alignment for me shows up most often in highly simple and repetitive code, like constant definitions and type declarations. By marking those entire sections as ignored for the formatter, it allows for more better looking options, like aligning the equal signs as well as the comments after the numbers. It also would not be good for a formatter to have to decide to favor alignment or line length, which is why I think these sorts of sections should always have formatting explicitly disabled.

(small plug, that's why I made the very production not ready [`cargo-align`](https://crates.io/crates/cargo-align) that doesn't actually rely on cargo/is not rust specific, so I could have a tool to consistently make nice looking formatting like aligning both the equal signs and comments after.)

---
