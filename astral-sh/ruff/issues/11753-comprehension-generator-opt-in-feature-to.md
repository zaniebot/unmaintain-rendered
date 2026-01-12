```yaml
number: 11753
title: Comprehension / generator, opt-in feature to preserve line breaks
type: issue
state: open
author: Martin19037
labels:
  - formatter
  - style
assignees: []
created_at: 2024-06-05T12:57:23Z
updated_at: 2026-01-05T20:58:37Z
url: https://github.com/astral-sh/ruff/issues/11753
synced_at: 2026-01-12T15:54:51Z
```

# Comprehension / generator, opt-in feature to preserve line breaks

---

_@Martin19037_

Before I begin, here is some context:

I would love to incorporate ruff as a pre-commit formatter for one of our projects. We developed the habit of almost always separating comprehension target-expression, for-in statement(s), and optional if-expression(s) into separate lines, except for very short and trivial comprehensions. Naturally, this makes identifying these separate comprehension parts very easy at a glance, at the cost of a few more lines. Also, we have increased our line length somewhat.

In its current form, ruff will always collapse comprehensions to a single line if:
- it is short enough to fit within the line length
- there are no "magic trailing commas" in any of the comprehension parts

We do use trailing commas ourselves, so many of our hand-formatted comprehensions that contain trailing commas in any form remain more or less unchanged after running ruff format. The ones that do not, usually get collapsed to a single line mess (also due to our increased line length).

One toy-example for this would be:
```python
[
    i + n
    for i in range(5)
    for n in range(5)
    if i < 3
]
```

which would be collapsed to a single line:
```python
[i + n for i in range(5) for n in range(5) if i < 3]
```

which is not as easy to recognize at a glance as the original example.

Would it be possible to have an opt-in feature to always preserve line breaks in comprehensions and generators? The behavior would be the same as if there were a trailing comma anywhere in the comprehension, and each comprehension-part is given a separate line, maybe even expand the whole comprehension into separate lines if there is at least one line break in it.



---

_Label `formatter` added by @AlexWaygood on 2024-06-05 13:18_

---

_Label `style` added by @MichaReiser on 2024-06-05 13:52_

---

_Comment by @MichaReiser on 2024-06-05 13:59_

Thanks for the detailed write up. I can understand that nested comprehensions can get hard to read.

> Would it be possible to have an opt-in feature to always preserve line breaks in comprehensions and generators? 

Respecting line breaks would set a new precedence for ruff/black. There are other formatters that rely on line breaks. For example, prettier doesn't collapse an object expression if it has a line break after the last property's value:

```javascript
let a = { 
	a: 1,
  	b: 2
}
```

Whereas the following will be collapsed because it doesn't have a line break after `b`

```javascript

let a = { 
	a: 1,
  	b: 2 }
```

I could see a similar rule where the comprehension wouldn't collapse if there's a line break before the closing `]`, but I'm very hesitant of adding it because:

* It makes formatting changes non-reversible. A comprehension never collapses after the formatter expanded it once
* It's non obvious why the formatter doesn't collapse some comprehensions but does collapse others
* It will lead to less consistent code because users can now overrule the formatter. 

Black's/Ruff's magic trailing comma suffers the same problem. 

I'm more open if we could identify a formatting rule that makes comprehensions more readable in general without requiring manual intervention. Are there specific patterns in the body that should never collapse? E.g. if there's any nesting?

---

_Comment by @Martin19037 on 2024-06-06 08:43_

@MichaReiser  Thank you for your feedback. Your arguments sparked a discussion within our team, what the actual requirements of such a feature would be, and that the initial idea of relying on line breaks is not sufficient or feasible.

Initially, we though that the decision, whether to collapse or expand a given comprehension is mostly subjective. After checking a number of actual examples from our code base, we noticed a pattern that may describe what we are looking for. We usually collapse comprehensions that do not stray too far from the "visual span", as in, you can see and perceive the whole comprehension without having to scan your eyes along horizontally too much, if that makes sense to you.

Another toy example that may show what I mean. Here are two comprehensions that would produce the same AST:
```python
[x for x in range(3) if x == 0]

[descriptive_name for descriptive_name in range(3) if descriptive_name == 0]
```

We would prefer the top one collapsed, since it is short enough. The bottom one we would expand, as it is more effort to scan/read along the line to know what is going on, even though it has the same complexity as the top one.

Using this knowledge, our suggestion would be to introduce a maximum comprehension (or generator) expression length (opt-in of course, to preserve Black compatibility). Unlike the line length parameter, it only describes the length of a collapsed comprehension expression itself, regardless of indentation or nesting. I do not know any implementation details, but I imagine it working like so:
- If a comprehension can be collapsed using existing rules, the collapsed single-line form is measured against the max comprehension expression length
- If we are below the expression length, the collapsed form can be kept
- Otherwise, it has to be expanded

Regarding nesting, I imagine this also working for deeply nested and highly indented cases. If a nested comprehension expression is short enough, it may be collapsed within a larger, expanded one. The overall line-length still wins over this, if indentation or nesting is too high.

Does this make sense at all? Also is this technically feasible to implement, or are we dreaming too much here?

---

_Comment by @Avasam on 2024-06-11 18:07_

This is something I've suggested on the formatter alpha/beta feedback as well, and probably my only reticence in using Black or Ruff as a formatter (still on autopep8 + add-trailing-comma ). And bad unwrapping is the main reason (amongst others) I don't use prettier.
https://github.com/astral-sh/ruff/discussions/7310#discussioncomment-7027958

The tl;dr of my post above: there's magic commas for most of my needs where I want to force a vertical style, but nothing as such for comprehensions and conditional expressions. (unless you count using `# fmt: skip` on most of them). Although i could be happy enough with a single `#` ?

---

Similar comment from @Booplicate : https://github.com/astral-sh/ruff/discussions/7310#discussioncomment-7428754

---

I feel like @Martin19037 's description fits pretty well. Having a conditional/ternary/comprehension-specific length (total line length? count just the comprehension itself?) could be weird at first, but it fits all the requirements established by @MichaReiser (consistent, reversible, no manual intervention).

Although there's also the question of whether the `in` operator in comprehensions should be unwrapped as well.

---

_Comment by @aronhoff on 2024-11-15 17:36_

+1, this would be a really good addition.

I would be happy with @Martin19037's suggestion for the comprehension specific line length.

If we need more of a sledgehammer solution for some reason, even a bool setting that expands all comprehensions into the multiline format would work. I find it very rare that one would be trivial enough not to expand. It is so much easier to understand what's happening in the expanded format, even if the lines end up short. I see these comprehensions as the exception rather than the rule.

I was considering adding ruff to one of my projects, but seeing that it would collapse all comprehensions turned me away.

---

_Comment by @Booplicate on 2024-11-17 13:33_

Have separate line length would probably be the best. Alternatively, never unwrapping wrapped lines could also work.

---

_Comment by @clairem-sl on 2024-12-05 06:43_

Ha, I was about to suggest something similar to the lines of "Provide an option to not collapse comprehensions into one line"

Not sure how AST looks like, but in my mind, comprehensions have 3 "parts":

* Expression -- the thing that will be added into list / returned by generator
* Iteration -- the `for` part, contains at least 1 rule
* Optional Guard -- the `if` part, if exist, contains at least one rule

So _in addition_ to the overall line length (after collapsing), it could also have additional rules:

* If there are more than N1 Iterations, do not collapse
* If there are more than N2 Guards, do not collapse
* If there are a total of more than N3 'parts' (expression + iterations + guards), do not collapse

I'd still love to have a per-project knob to just "do not collapse any comprehension", though.

---

_Comment by @NeilGirdhar on 2025-01-01 23:45_

I think this might be a good time to suggest a more "ideal solution" (in my eyes) that the formatter starts evaluating line complexity and "goodness" of various line breaks.

---

_Comment by @lundybernard on 2025-05-31 01:16_

+1 for an opt-in option to disable formatting for comprehensions.

It is extremely frustrating when the formatter decides to squash a perfectly good, readable comprehension into a single line.

---

*What follows is some examples of my frustration with the formatter, and notes for anyone who wants to keep using Ruff but keep their readable comprehensions.*

```python
keys = [
    key 
    for key in key_generator 
    if key.startswith('a') 
    and key.endswith('9')
]
```
becomes:
```python
keys = [
    key for key in key_generator if key.startswith('a') and key.endswith('9')
]
```

What's worse, forcing the formatter to behave requires some really ugly comments
```python
# You cannot hide the # fmt: comments on the open/close lines of the collection
keys = [  # fmt: on, or # fmt: skip, do not work here
    key for key in key_generator if key.startswith('a') and key.endswith('9')
]  # fmt: off, does not work here.

# this doesn't work
keys = [ # fmt: skip
    ...
]

# but these do.  
# fmt: on/off must be on its own line.
# fmt: skip needs to be inside the comprehension... and you may need more than one.

# fmt: off
keys = [
    ...
]
# fmt: on

keys = [
    keys  # fmt: skip
    for key in key_generator
    if key.startswith('a')  # fmt: skip
    and key.endswith('9')
]
```

The least-ugly, but still undesirable solution I found, was just adding empty comments, which gets the job done for now, with minimal crufty comments.

```python
# This works, and requires less visual clutter... It is more readable to me, but... "why are there empty comments?"
keys = [
    key  #
    for key in key_generator
    if key.startswith('a')  #
    and key.endswith('9')  #
]
```
This solution does not let you use extra indentation, which is perfectly valid, and can improve readability significantly in more complex comprehensions.

This final example demonstrates using additional indentation to improve legibility of a very complex comprehension:

```python
# For this to remain formatted, it must be wrapped in # fmt: off/on comments
# fmt: off
matrix = [
    (key + '-07', value / 10)
    for key in key_generator
        if key.startswith('a')
        and key.endswith('9')
    for value in value_list
        if value >= 1
]
# fmt: on
```


---

_Comment by @Avasam on 2025-06-11 23:24_

Avoiding "half-wrapping" (like is requested for params in https://github.com/astral-sh/ruff/issues/9992) would also _help_ comprehension formatting.

If the opening and closing brackets don't fit on the same line, then wrap per keyword.

---

_Comment by @MichaReiser on 2025-06-28 06:19_

Rustfmt's default is to use lower limits for these inline expressions. This seems reasonable and is probably not too hard to implement https://rust-lang.github.io/rustfmt/?version=v1.8.0&search=#use_small_heuristics

Finding the right percentages could be challenging 

---

_Comment by @NeilGirdhar on 2025-06-28 06:29_

> Finding the right percentages could be challenging

Why did you settle on the approach of percentages of line width?  Have you considered something closer to what the most advaned text formatters (like TeX) do, which is to set a "badness" for various break positions?  This is much closer to what you're actually trying to optimize since a human reader feels different line break positions differently.  Human readers don't try to make certain constructs take a certain fraction of the page width.

Thus, instead of weird line width percentages, you might have:
* break within an expression: 10
* break in a ternary condition before `if`: 3
* break after a comma in a function call: 2
* expression with 10 terms on one line: 1 (rather than setting a width percentage to try to chop them—this doesn't get affected by variable name lengths)
* etc

Also, the badness approach is amenable to dynamic programming, which makes it very efficient.

---

_Comment by @MichaReiser on 2025-06-28 07:07_

I don't think we made a decision. I only posted it as reference and expressed that I'm open to it because it's relatively simple to implement. I also never found rustfmt's formatting annoying. So maybe all that's needed is a simple solution like this. The best way to find out is to look at ecosystem changes 

Implementing the badness approach would require extending the formatter infrastructure in much larger degrees. It does have some limited support for priorities but overusing it leads to exponential runtime 

Edit: Ruff does already prioritize break points. The part that's missing is *expression with 10 terms on one line: 1 (rather than setting a width percentage to try to chop them—this doesn't get affected by variable name lengths)* where I feel like the line length seems a reasonable enough approximation of what we want. E.g. would `a.b` count as one, two or even three terms? Either way can lead to undesired breakage. But we can try to come up with some heuristics and always force a line break if we can. I'm just not convinced that this will give us much better results than line width (and introduces more complexity and is slower)

---

_Comment by @NeilGirdhar on 2025-06-28 07:57_

> I don't think we made a decision. I only posted it as reference and expressed that I'm open to it because it's relatively simple to implement.

Fair enough, but I hope simple to implement isn't going to entrench a limited solution.

> Implementing the badness approach would require extending the formatter infrastructure in much larger degrees. It does have some limited support for priorities but overusing it leads to exponential runtime

Sorry, but I think you're mistaken.  TeX and other formatters use this approach for every token.  This is a classic dynamic programming problem that is solved by  the _Knuth-Plass Algorithm_.  It has *quadratic* time and space cost.  It will never be exponential runtime for any input.  And, because of the nature of Python code, whenever you close a function, you've essentially back at the base case.  Therefore, its worst case is quadratic in the number of tokens in a function totalled over all functions.

> . E.g. would a.b count as one, two or even three terms?

I think the idea is to try to imagine it from a human perspective.  I would say that's at least 2 tokens, just from the feel of it.  It doesn't really matter what you choose though since you can adjust the corresponding badness to get the effect you want.

A good way to set the badnesses would be to use a hyperparameter optimizer, run over well-established codebases, and use as a loss function the size of the diff.  This should efficiently tune all of the badnesses for you.

> But we can try to come up with some heuristics and always

This is exactly the problem.  With each heuristic, you knock out a few issues, but create dozens more issues of special cases that people will want fixed.  There's a reason that TeX didn't go this route with typesetting: it produces very poor results except in the exact cases that you imagined when you came up with your heuristics.

>  I'm just not convinced that this will give us much better results than line width (and introduces more complexity and is slower)

I don't think it's slower.  Besides the quadratic runtime, the analysis is quite simple whereas you'll need to check of your heuristics against each piece of code, which is linear in both the number of heuristics and the complexity of the heuristic. 

As for complexity, your heuristics can individually be more complex than the things you assign badness to.  Either way, I think both solutions are fairly easy to implement.

Is it just the dynamic programming that's daunting?

---

_Comment by @MichaReiser on 2025-06-28 14:32_

Let's keep this discussion focused on how we can improve the comprehension formatting with how the formatter works today. Ideally, that shouldn't even matter and we can focus on when and how comprehension should be broken onto multiple lines. 

Feel free to open another issue about weight based formatting. I see it as very unlikely that we change our formatter into that direction. One of the main reason for black is that finding the right weights can be a struggle and changing them may improve some formatting but makes other formatting worse. 

---

_Comment by @Avasam on 2025-06-28 16:53_

I usually have a "soft line length" of 80 and a hard one of 100. So if I set a "percentage" for comprehensions (in my case 80% (100 * 0.8 = 80) ) that would work.
Not sure why percentage instead of configuring an arbitrary number (other than it was the example you used, maybe to enable by default w/o conflicting with absolute line length?). But I don't really care either way, both works for me.

---

_Comment by @Germanki on 2025-08-04 11:50_

+1 that this is also a desired functionality. 

---

_Comment by @kevinjdolan on 2025-10-06 19:26_

+1 on this feature — I feel so strongly as that I have in the past enforced multi-line comprehensions in different formatters. Obviously I don’t think that it should be enforced across the board in the ruff formatter, but it should at least be configurable to be allowed, imo.

Is this an implementation resource issue or still a philosophical debate? I’m happy to look into your contribution process if desirable 

---

_Comment by @kevind-kizen on 2025-10-07 02:31_

@Martin19037 @MichaReiser I know that philosophical consensus has not been reached, but I submitted a PR that I think roughly covers a good solution. Happy to debate on merits, but I think this brings a much-needed readability option to users, and I do not believe it is in any way in conflict with PEP8. It is backwards-compatible, as it requires explicit opt-in.

---

_Comment by @kevind-kizen on 2025-10-07 15:25_

I would like to open the debate again here.

My PR implementation is as follows:

```
Added comprehension-line-break with two modes:

"auto" (default): Current behavior - collapses comprehensions to single line if they fit
"preserve": Preserves multi-line formatting from source code
Configuration in pyproject.toml:

[tool.ruff.format]
comprehension-line-break = "preserve"
Example Behavior

Input:

{
    obj["key"]: obj["value"]
    for obj
    in get_fields()
    if obj["key"] != "name"
}
Output with "auto" (default):

{obj["key"]: obj["value"] for obj in get_fields() if obj["key"] != "name"}
Output with "preserve":

{
    obj["key"]: obj["value"]
    for obj in get_fields()
    if obj["key"] != "name"
}
```

I do not believe there is any PEP8-justifiable reason to prefer the single-line version to the multi-line version.

1. Readability First: PEP 8 explicitly states "Readability counts" and "code is read much more often than it is written." Our preserve option supports this by allowing developers to maintain readable multi-line formatting for complex comprehensions.
2. Line Length Compliance: Both modes respect the 79-character limit:
    - auto mode: Collapses only when it fits within line length
    - preserve mode: Maintains multi-line format, ensuring compliance with line length
3. No Explicit Rules Against Multi-line Comprehensions: PEP 8 does not mandate that comprehensions must be on a single line. It only provides general guidance about line breaking and readability.
4. Consistency Principle: PEP 8 emphasizes consistency within a project. Our option allows teams to choose and maintain a consistent style for their comprehensions.

If determinism is really so critical, I am open to adding a `force` mode that would force comprehensions to be broken onto multiple lines. I still don't really see the case for preventing it from happening when the developer decides it's a good idea.

---

_Comment by @kevind-kizen on 2025-10-07 15:55_

I think that simply allowing the preservation of line breaks when the developer makes the decision to break the lines along the comprehension keywords (which is what I did in the PR) is very similar in philosophy to the magic-trailing-commas configuration option.

I am now curious -- I might run a systematic analysis over public python repositories to better understand what people typically do in real codebases.

---

_Comment by @kevind-kizen on 2025-10-07 22:12_

Please see this repo for analysis code. It's rough, but I think it paints a broad picture of the community's common practices:

https://github.com/kevind-kizen/python-comprehension-analysis

Report output:

```
======================================================================
COMPREHENSION ANALYSIS REPORT
======================================================================

Total Comprehensions: 419,540
Unique Repositories: 943

----------------------------------------------------------------------
COMPREHENSION TYPES
----------------------------------------------------------------------
  list        :  267,852 (63.84%)
  generator   :   94,054 (22.42%)
  dict        :   43,519 (10.37%)
  set         :   14,115 ( 3.36%)

----------------------------------------------------------------------
SINGLE-LINE vs MULTI-LINE (by type)
----------------------------------------------------------------------

  DICT:
    Single-line:   27,925 (64.17%)
    Multi-line:    15,594 (35.83%)

  GENERATOR:
    Single-line:   68,737 (73.08%)
    Multi-line:    25,317 (26.92%)

  LIST:
    Single-line:  196,376 (73.32%)
    Multi-line:    71,476 (26.68%)

  SET:
    Single-line:   11,304 (80.09%)
    Multi-line:     2,811 (19.91%)

  OVERALL:
    Single-line:  304,342 (72.54%)
    Multi-line:   115,198 (27.46%)

----------------------------------------------------------------------
NESTED COMPREHENSIONS
----------------------------------------------------------------------
  With nesting:       9,132 ( 2.18%)
  Without nesting:  410,408 (97.82%)

  By type:
    dict        :    1,583 ( 3.64% of dict)
    generator   :    1,257 ( 1.34% of generator)
    list        :    6,174 ( 2.31% of list)
    set         :      118 ( 0.84% of set)

----------------------------------------------------------------------
LINE COUNT DISTRIBUTION
Multi-line comprehensions only, excluding bracket-only lines
----------------------------------------------------------------------
    1 line(s):   27,156 (23.57%)
    2 line(s):   41,948 (36.41%)
    3 line(s):   21,304 (18.49%)
    4 line(s):    7,654 ( 6.64%)
    5 line(s):    4,993 ( 4.33%)
    6 line(s):    3,467 ( 3.01%)
    7 line(s):    2,418 ( 2.10%)
    8 line(s):    1,727 ( 1.50%)
    9 line(s):    1,209 ( 1.05%)
   10 line(s):      758 ( 0.66%)

----------------------------------------------------------------------
CHARACTER COUNT STATISTICS (Overall, whitespace collapsed)
----------------------------------------------------------------------
  Mean:         67.5 characters
  Percentiles:
    p  0:         9.0 characters
    p  1:        21.0 characters
    p  5:        27.0 characters
    p 10:        31.0 characters
    p 25:        41.0 characters
    p 50:        55.0 characters
    p 75:        77.0 characters
    p 90:       110.0 characters
    p 95:       141.0 characters
    p 99:       257.0 characters
    p100:    17,864.0 characters

----------------------------------------------------------------------
CHARACTER COUNT: SINGLE-LINE vs MULTI-LINE (whitespace collapsed)
----------------------------------------------------------------------
  SINGLE-LINE (304,342 comprehensions):
    Mean:         49.5 characters
    Percentiles:
      p  0:         9.0 characters
      p  1:        21.0 characters
      p  5:        26.0 characters
      p 10:        29.0 characters
      p 25:        37.0 characters
      p 50:        47.0 characters
      p 75:        59.0 characters
      p 90:        72.0 characters
      p 95:        82.0 characters
      p 99:       102.0 characters
      p100:       416.0 characters

  MULTI-LINE (115,198 comprehensions):
    Total length statistics:
      Mean:        115.3 characters
      Percentiles:
        p  0:        14.0 characters
        p  1:        42.0 characters
        p  5:        56.0 characters
        p 10:        63.0 characters
        p 25:        76.0 characters
        p 50:        95.0 characters
        p 75:       125.0 characters
        p 90:       176.0 characters
        p 95:       229.0 characters
        p 99:       410.0 characters
        p100:    17,864.0 characters

----------------------------------------------------------------------
INDIVIDUAL LINE LENGTH STATISTICS (Multi-line comprehensions)
----------------------------------------------------------------------

    With indentation (348,649 lines total):
      (Multi-line comprehensions only, excludes blank lines and lines with only brackets)
      Mean:         39.3 characters
      Percentiles:
        p  0:         2.0 characters
        p  1:         3.0 characters
        p  5:         8.0 characters
        p 10:        14.0 characters
        p 25:        22.0 characters
        p 50:        35.0 characters
        p 75:        54.0 characters
        p 90:        70.0 characters
        p 95:        79.0 characters
        p 99:       101.0 characters
        p100:       331.0 characters

    Stripped (whitespace removed) (348,649 lines total):
      (Multi-line comprehensions only, excludes blank lines and lines with only brackets)
      Mean:         36.1 characters
      Percentiles:
        p  0:         1.0 characters
        p  1:         2.0 characters
        p  5:         5.0 characters
        p 10:        10.0 characters
        p 25:        19.0 characters
        p 50:        32.0 characters
        p 75:        51.0 characters
        p 90:        67.0 characters
        p 95:        76.0 characters
        p 99:        98.0 characters
        p100:       325.0 characters

----------------------------------------------------------------------
STRUCTURAL STATISTICS
----------------------------------------------------------------------

    Bracket/brace formatting:
      Both brackets on own lines:       96,687 (83.93%)
      Only opening on own line:          3,152 ( 2.74%)
      Only closing on own line:            734 ( 0.64%)
      Neither on own lines:             14,625 (12.70%)

----------------------------------------------------------------------
NON-NESTED MULTI-LINE COMPREHENSION STRUCTURE
----------------------------------------------------------------------

OVERALL (All Types)
======================================================================
Total analyzed: 109,383

  Most common structural patterns (top 20):
    Format: [content] [content] ... (one bracket pair per line)
    '...' prefix = line does not start with a keyword
    '[...]' = line with no keywords
    '[...]*' = multiple consecutive lines with no keywords
    '[]' = blank line
    '[#]' = comment-only line
    '#' suffix = line ends with a comment
    Multiple keywords = first is at start, rest are interior

    29,255 (26.75%) | [...] [for,in]
    18,651 (17.05%) | [...for,in]
    11,942 (10.92%) | [...]* [for,in]
     8,281 ( 7.57%) | [...] [for,in] [if]
     7,699 ( 7.04%) | [...for,in,if]
     3,434 ( 3.14%) | [...] [for,in] [...in,if]
     2,342 ( 2.14%) | [...if] [for,in]
     2,140 ( 1.96%) | [...for,in] [if]
     1,764 ( 1.61%) | [...] [for,in] [...]*
     1,688 ( 1.54%) | [...]* [...for,in]
     1,419 ( 1.30%) | [...] [for,in] [for,in]
     1,072 ( 0.98%) | [...for,in] [...]
       982 ( 0.90%) | [...] [for,in] [...]
       926 ( 0.85%) | [...in] [for,in]
       894 ( 0.82%) | [...for,in] [...in,if]
       600 ( 0.55%) | [...] [...for,in]
       542 ( 0.50%) | [...] [if] [...] [for,in]
       525 ( 0.48%) | [...] [for,in] [if] [...]*
       496 ( 0.45%) | [...] [for,in] [if] [...]
       454 ( 0.42%) | [...] [for,in,if]


DICT COMPREHENSIONS
======================================================================
Total analyzed: 14,402

  Most common structural patterns (top 15):
    Format: [content] [content] ... (one bracket pair per line)
    '...' prefix = line does not start with a keyword
    '[...]' = line with no keywords
    '[...]*' = multiple consecutive lines with no keywords
    '[]' = blank line
    '[#]' = comment-only line
    '#' suffix = line ends with a comment
    Multiple keywords = first is at start, rest are interior

     4,001 (27.78%) | [...] [for,in]
     2,589 (17.98%) | [...for,in]
     1,188 ( 8.25%) | [...] [for,in] [if]
     1,121 ( 7.78%) | [...]* [for,in]
     1,099 ( 7.63%) | [...for,in,if]
       940 ( 6.53%) | [...] [for,in] [...in,if]
       362 ( 2.51%) | [...if] [for,in]
       327 ( 2.27%) | [...] [for,in] [...]*
       169 ( 1.17%) | [...] [for,in] [...]
       154 ( 1.07%) | [...] [for,in] [for,in]
       141 ( 0.98%) | [...] [for,in,if]
       122 ( 0.85%) | [...for,in] [if]
       119 ( 0.83%) | [...for,in] [...in,if]
       103 ( 0.72%) | [...] [for,in] [...]* [if]
        90 ( 0.62%) | [...for,in] [...]


GENERATOR COMPREHENSIONS
======================================================================
Total analyzed: 24,424

  Most common structural patterns (top 15):
    Format: [content] [content] ... (one bracket pair per line)
    '...' prefix = line does not start with a keyword
    '[...]' = line with no keywords
    '[...]*' = multiple consecutive lines with no keywords
    '[]' = blank line
    '[#]' = comment-only line
    '#' suffix = line ends with a comment
    Multiple keywords = first is at start, rest are interior

     6,363 (26.05%) | [...] [for,in]
     5,720 (23.42%) | [...for,in]
     1,955 ( 8.00%) | [...] [for,in] [if]
     1,622 ( 6.64%) | [...]* [for,in]
     1,484 ( 6.08%) | [...for,in,if]
       767 ( 3.14%) | [...in] [for,in]
       478 ( 1.96%) | [...if] [for,in]
       447 ( 1.83%) | [...] [for,in] [...in,if]
       429 ( 1.76%) | [...for,in] [if]
       363 ( 1.49%) | [...] [for,in] [...]*
       342 ( 1.40%) | [...] [for,in] [for,in]
       251 ( 1.03%) | [...for,in] [...]
       178 ( 0.73%) | [...] [for,in] [...]* [if]
       148 ( 0.61%) | [...] [for,in] [for,in] [if]
       140 ( 0.57%) | [...] [for,in] [...]


LIST COMPREHENSIONS
======================================================================
Total analyzed: 67,833

  Most common structural patterns (top 15):
    Format: [content] [content] ... (one bracket pair per line)
    '...' prefix = line does not start with a keyword
    '[...]' = line with no keywords
    '[...]*' = multiple consecutive lines with no keywords
    '[]' = blank line
    '[#]' = comment-only line
    '#' suffix = line ends with a comment
    Multiple keywords = first is at start, rest are interior

    18,521 (27.30%) | [...] [for,in]
     9,684 (14.28%) | [...for,in]
     9,137 (13.47%) | [...]* [for,in]
     4,819 ( 7.10%) | [...for,in,if]
     4,751 ( 7.00%) | [...] [for,in] [if]
     1,880 ( 2.77%) | [...] [for,in] [...in,if]
     1,552 ( 2.29%) | [...]* [...for,in]
     1,525 ( 2.25%) | [...for,in] [if]
     1,478 ( 2.18%) | [...if] [for,in]
     1,034 ( 1.52%) | [...] [for,in] [...]*
       846 ( 1.25%) | [...] [for,in] [for,in]
       716 ( 1.06%) | [...for,in] [...]
       638 ( 0.94%) | [...] [for,in] [...]
       598 ( 0.88%) | [...for,in] [...in,if]
       477 ( 0.70%) | [...] [...for,in]


SET COMPREHENSIONS
======================================================================
Total analyzed: 2,724

  Most common structural patterns (top 15):
    Format: [content] [content] ... (one bracket pair per line)
    '...' prefix = line does not start with a keyword
    '[...]' = line with no keywords
    '[...]*' = multiple consecutive lines with no keywords
    '[]' = blank line
    '[#]' = comment-only line
    '#' suffix = line ends with a comment
    Multiple keywords = first is at start, rest are interior

       658 (24.16%) | [...for,in]
       387 (14.21%) | [...] [for,in] [if]
       370 (13.58%) | [...] [for,in]
       297 (10.90%) | [...for,in,if]
       167 ( 6.13%) | [...] [for,in] [...in,if]
        77 ( 2.83%) | [...] [for,in] [for,in]
        64 ( 2.35%) | [...for,in] [if]
        62 ( 2.28%) | [...]* [for,in]
        40 ( 1.47%) | [...] [for,in] [...]*
        39 ( 1.43%) | [...for,in] [...in,if]
        37 ( 1.36%) | [...] [for,in] [for,in] [if]
        35 ( 1.28%) | [...] [for,in] [...]
        34 ( 1.25%) | [...] [for,in] [if] [...]*
        30 ( 1.10%) | [...] [for,in] [if] [...]
        24 ( 0.88%) | [...if] [for,in]
```

---

_Comment by @kevind-kizen on 2025-10-07 22:26_

@MichaReiser 

**TLDR:** About 1/4 of all comprehensions across 943 github repos were multi-line. I think this demonstrates substantial community adoption of the pattern and warrants being supported by the ruff formatter, to the developer's discretion. A few common patterns jump out, but the general rule that I recommend implementing is to simply preserve line-breaks when they are included immediately before a comprehension keyword. 

I think that it is simple, clear, and allows for consistent formatting while allowing the developer the _choice_ to make their code more readable -- it requires no complex analysis of the code to find the optimal line-breaks.

Highlights:

- Analyzed 419k examples of comprehensions across 943 github repos tagged as language: python.
- Of these, 27% of all comprehensions were multi-line.
- Set comprehensions had the lowest percentage (19%), while Dict comprehensions had the highest (36%)
- The average character length of single-line vs multi-line comprehensions (with whitespace collapsed into a single character) was about 50chars vs 115chars.
- Within the multi-line comprehension set, 50th percentile for character length was 95 characters.
- 75th percentile for line length (normalizing tabs to 2 spaces), was 70 characters
- Of multi-line comprehensions, 83% included the brackets on their own lines (my preference)
- An astonishing 12% include both the opening and closing brackets on lines with other expressions (this surprised me)
- There were about 100k non-nested multi-line comprehensions to analyze for structure
- Across all comprehension types, the most common pattern was 2 lines, such as:
```
[
  item.get()
  for item in collection
]
```
- Interestingly, 17% had single-line bodies, such as:
```
[
  item.get() for item in collection
]
```
- Most of the patterns seem to keep the `for` and `in` keywords on the same line, 

---

_Comment by @Avasam on 2025-10-07 22:28_

@kevind-kizen Are you ignoring repos already formatted by black and ruff? That'll obviously heavily skew the numbers towards the existing Ruff format.

---

_Comment by @kevind-kizen on 2025-10-07 23:40_

Good point -- I am not excluding those in this analysis. I added a heuristic to check for possible formatting with black/ruff. It definitely does significantly skew the output. Overall, 33.5% of of all comprehensions are multi-line excluding those, whereas in the original dataset it was only 27%.

The full output:

```
======================================================================
COMPREHENSION ANALYSIS REPORT
======================================================================

Total Comprehensions: 148,194
Unique Repositories: 575

----------------------------------------------------------------------
COMPREHENSION TYPES
----------------------------------------------------------------------
  list        :  100,543 (67.85%)
  generator   :   32,710 (22.07%)
  dict        :   13,000 ( 8.77%)
  set         :    1,941 ( 1.31%)

----------------------------------------------------------------------
SINGLE-LINE vs MULTI-LINE (by type)
----------------------------------------------------------------------

  DICT:
    Single-line:    8,637 (66.44%)
    Multi-line:     4,363 (33.56%)

  GENERATOR:
    Single-line:   24,296 (74.28%)
    Multi-line:     8,414 (25.72%)

  LIST:
    Single-line:   72,935 (72.54%)
    Multi-line:    27,608 (27.46%)

  SET:
    Single-line:    1,448 (74.60%)
    Multi-line:       493 (25.40%)

  OVERALL:
    Single-line:  107,316 (72.42%)
    Multi-line:    40,878 (27.58%)

----------------------------------------------------------------------
NESTED COMPREHENSIONS
----------------------------------------------------------------------
  With nesting:       3,589 ( 2.42%)
  Without nesting:  144,605 (97.58%)

  By type:
    dict        :      469 ( 3.61% of dict)
    generator   :      521 ( 1.59% of generator)
    list        :    2,578 ( 2.56% of list)
    set         :       21 ( 1.08% of set)

----------------------------------------------------------------------
LINE COUNT DISTRIBUTION
Multi-line comprehensions only, excluding bracket-only lines
----------------------------------------------------------------------
    1 line(s):    7,026 (17.19%)
    2 line(s):   19,375 (47.40%)
    3 line(s):    6,162 (15.07%)
    4 line(s):    2,679 ( 6.55%)
    5 line(s):    1,764 ( 4.32%)
    6 line(s):    1,139 ( 2.79%)
    7 line(s):      808 ( 1.98%)
    8 line(s):      552 ( 1.35%)
    9 line(s):      334 ( 0.82%)
   10 line(s):      255 ( 0.62%)

----------------------------------------------------------------------
CHARACTER COUNT STATISTICS (Overall, whitespace collapsed)
----------------------------------------------------------------------
  Mean:         65.0 characters
  Percentiles:
    p  0:         9.0 characters
    p  1:        21.0 characters
    p  5:        27.0 characters
    p 10:        31.0 characters
    p 25:        40.0 characters
    p 50:        54.0 characters
    p 75:        75.0 characters
    p 90:       105.0 characters
    p 95:       133.0 characters
    p 99:       249.0 characters
    p100:    16,775.0 characters

----------------------------------------------------------------------
CHARACTER COUNT: SINGLE-LINE vs MULTI-LINE (whitespace collapsed)
----------------------------------------------------------------------
  SINGLE-LINE (107,316 comprehensions):
    Mean:         48.8 characters
    Percentiles:
      p  0:         9.0 characters
      p  1:        20.0 characters
      p  5:        25.0 characters
      p 10:        29.0 characters
      p 25:        36.0 characters
      p 50:        46.0 characters
      p 75:        58.0 characters
      p 90:        72.0 characters
      p 95:        82.0 characters
      p 99:       106.0 characters
      p100:       374.0 characters

  MULTI-LINE (40,878 comprehensions):
    Total length statistics:
      Mean:        107.4 characters
      Percentiles:
        p  0:        21.0 characters
        p  1:        38.0 characters
        p  5:        51.0 characters
        p 10:        58.0 characters
        p 25:        71.0 characters
        p 50:        89.0 characters
        p 75:       118.0 characters
        p 90:       165.0 characters
        p 95:       217.0 characters
        p 99:       396.0 characters
        p100:    16,775.0 characters

----------------------------------------------------------------------
INDIVIDUAL LINE LENGTH STATISTICS (Multi-line comprehensions)
----------------------------------------------------------------------

    With indentation (119,732 lines total):
      (Multi-line comprehensions only, excludes blank lines and lines with only brackets)
      Mean:         38.1 characters
      Percentiles:
        p  0:         2.0 characters
        p  1:         3.0 characters
        p  5:         9.0 characters
        p 10:        14.0 characters
        p 25:        23.0 characters
        p 50:        35.0 characters
        p 75:        51.0 characters
        p 90:        66.0 characters
        p 95:        75.0 characters
        p 99:       100.0 characters
        p100:       308.0 characters

    Stripped (whitespace removed) (119,732 lines total):
      (Multi-line comprehensions only, excludes blank lines and lines with only brackets)
      Mean:         34.9 characters
      Percentiles:
        p  0:         1.0 characters
        p  1:         2.0 characters
        p  5:         6.0 characters
        p 10:        11.0 characters
        p 25:        19.0 characters
        p 50:        31.0 characters
        p 75:        47.0 characters
        p 90:        63.0 characters
        p 95:        72.0 characters
        p 99:        98.0 characters
        p100:       300.0 characters

----------------------------------------------------------------------
STRUCTURAL STATISTICS
----------------------------------------------------------------------

    Bracket/brace formatting:
      Both brackets on own lines:       28,336 (69.32%)
      Only opening on own line:          1,979 ( 4.84%)
      Only closing on own line:            243 ( 0.59%)
      Neither on own lines:             10,320 (25.25%)

----------------------------------------------------------------------
NON-NESTED MULTI-LINE COMPREHENSION STRUCTURE
----------------------------------------------------------------------

OVERALL (All Types)
======================================================================
Total analyzed: 38,599

  Most common structural patterns (top 20):
    Format: [content] [content] ... (one bracket pair per line)
    '...' prefix = line does not start with a keyword
    '[...]' = line with no keywords
    '[...]*' = multiple consecutive lines with no keywords
    '[]' = blank line
    '[#]' = comment-only line
    '#' suffix = line ends with a comment
    Multiple keywords = first is at start, rest are interior

    12,722 (32.96%) | [...] [for,in]
     5,003 (12.96%) | [...for,in]
     3,792 ( 9.82%) | [...]* [for,in]
     1,776 ( 4.60%) | [...for,in,if]
     1,765 ( 4.57%) | [...] [for,in] [if]
     1,501 ( 3.89%) | [...for,in] [if]
     1,328 ( 3.44%) | [...]* [...for,in]
       773 ( 2.00%) | [...if] [for,in]
       758 ( 1.96%) | [...for,in] [...]
       700 ( 1.81%) | [...] [for,in] [...in,if]
       602 ( 1.56%) | [...for,in] [...in,if]
       487 ( 1.26%) | [...] [for,in] [...]*
       453 ( 1.17%) | [...] [for,in] [for,in]
       427 ( 1.11%) | [...] [...for,in]
       325 ( 0.84%) | [...for,in] [...]*
       302 ( 0.78%) | [...] [for,in,if]
       288 ( 0.75%) | [...for,in,if] [...]
       281 ( 0.73%) | [...in] [for,in]
       271 ( 0.70%) | [...] [for,in] [...]
       146 ( 0.38%) | [...]* [for,in] [...]*


DICT COMPREHENSIONS
======================================================================
Total analyzed: 4,018

  Most common structural patterns (top 15):
    Format: [content] [content] ... (one bracket pair per line)
    '...' prefix = line does not start with a keyword
    '[...]' = line with no keywords
    '[...]*' = multiple consecutive lines with no keywords
    '[]' = blank line
    '[#]' = comment-only line
    '#' suffix = line ends with a comment
    Multiple keywords = first is at start, rest are interior

     1,314 (32.70%) | [...] [for,in]
       575 (14.31%) | [...for,in]
       260 ( 6.47%) | [...] [for,in] [if]
       243 ( 6.05%) | [...for,in,if]
       239 ( 5.95%) | [...] [for,in] [...in,if]
       184 ( 4.58%) | [...]* [for,in]
       128 ( 3.19%) | [...if] [for,in]
       113 ( 2.81%) | [...] [for,in,if]
        91 ( 2.26%) | [...for,in] [if]
        84 ( 2.09%) | [...for,in] [...in,if]
        69 ( 1.72%) | [...] [for,in] [...]*
        61 ( 1.52%) | [...] [for,in] [for,in]
        50 ( 1.24%) | [...for,in] [...]
        45 ( 1.12%) | [...] [for,in] [...]
        45 ( 1.12%) | [...]* [...for,in]


GENERATOR COMPREHENSIONS
======================================================================
Total analyzed: 8,083

  Most common structural patterns (top 15):
    Format: [content] [content] ... (one bracket pair per line)
    '...' prefix = line does not start with a keyword
    '[...]' = line with no keywords
    '[...]*' = multiple consecutive lines with no keywords
    '[]' = blank line
    '[#]' = comment-only line
    '#' suffix = line ends with a comment
    Multiple keywords = first is at start, rest are interior

     2,867 (35.47%) | [...] [for,in]
     1,329 (16.44%) | [...for,in]
       461 ( 5.70%) | [...] [for,in] [if]
       457 ( 5.65%) | [...]* [for,in]
       406 ( 5.02%) | [...for,in,if]
       244 ( 3.02%) | [...for,in] [if]
       223 ( 2.76%) | [...in] [for,in]
       173 ( 2.14%) | [...if] [for,in]
       149 ( 1.84%) | [...for,in] [...]
       117 ( 1.45%) | [...] [for,in] [for,in]
       106 ( 1.31%) | [...] [for,in] [...in,if]
        99 ( 1.22%) | [...for,in] [...]*
        96 ( 1.19%) | [...] [for,in] [...]*
        89 ( 1.10%) | [...for,in] [...in,if]
        64 ( 0.79%) | [...] [for,in] [...]* [if]


LIST COMPREHENSIONS
======================================================================
Total analyzed: 26,022

  Most common structural patterns (top 15):
    Format: [content] [content] ... (one bracket pair per line)
    '...' prefix = line does not start with a keyword
    '[...]' = line with no keywords
    '[...]*' = multiple consecutive lines with no keywords
    '[]' = blank line
    '[#]' = comment-only line
    '#' suffix = line ends with a comment
    Multiple keywords = first is at start, rest are interior

     8,480 (32.59%) | [...] [for,in]
     3,123 (12.00%) | [...]* [for,in]
     3,042 (11.69%) | [...for,in]
     1,226 ( 4.71%) | [...]* [...for,in]
     1,135 ( 4.36%) | [...for,in] [if]
     1,102 ( 4.23%) | [...for,in,if]
       982 ( 3.77%) | [...] [for,in] [if]
       555 ( 2.13%) | [...for,in] [...]
       468 ( 1.80%) | [...if] [for,in]
       405 ( 1.56%) | [...for,in] [...in,if]
       361 ( 1.39%) | [...] [...for,in]
       330 ( 1.27%) | [...] [for,in] [...in,if]
       318 ( 1.22%) | [...] [for,in] [...]*
       253 ( 0.97%) | [...] [for,in] [for,in]
       236 ( 0.91%) | [...for,in,if] [...]


SET COMPREHENSIONS
======================================================================
Total analyzed: 476

  Most common structural patterns (top 15):
    Format: [content] [content] ... (one bracket pair per line)
    '...' prefix = line does not start with a keyword
    '[...]' = line with no keywords
    '[...]*' = multiple consecutive lines with no keywords
    '[]' = blank line
    '[#]' = comment-only line
    '#' suffix = line ends with a comment
    Multiple keywords = first is at start, rest are interior

        62 (13.03%) | [...] [for,in] [if]
        61 (12.82%) | [...] [for,in]
        57 (11.97%) | [...for,in]
        31 ( 6.51%) | [...for,in] [if]
        28 ( 5.88%) | [...]* [for,in]
        25 ( 5.25%) | [...for,in,if]
        25 ( 5.25%) | [...] [for,in] [...in,if]
        24 ( 5.04%) | [...for,in] [...in,if]
        22 ( 4.62%) | [...] [for,in] [for,in]
        22 ( 4.62%) | [#] [...] [for,in]
        10 ( 2.10%) | [...for,in] [...]*
         9 ( 1.89%) | [...] [for,in] [for,in] [if]
         9 ( 1.89%) | [...for,in,if] [...]
         8 ( 1.68%) | [#] [...]# [for,in]
         5 ( 1.05%) | [...for,in] [if] [...]
```

---

_Comment by @MichaReiser on 2025-10-08 04:48_

Thanks for the analysis. 

>  I think this demonstrates substantial community adoption of the pattern and warrants being supported by the ruff formatter, to the developer's discretion. 

I don't agree with the conclusion that it requires that ruff preserves comprehensions exactly as written. I do agree that we need a way that splits comprehensions in fewer places. I still want to try the approach with a more limited width for comprehensions first to see if it achieves a similar outcome without having to introduce a new option and require manual formatting

---

_Comment by @lundybernard on 2025-10-08 06:23_

I would really appreciate the ability to disable formatting comprehensions entirely in pyproject.toml, or a nicer way to exclude a single comprehension.  
I have a few projects where I would like to use Ruff but don't because it mangles a few specific comps.

Wrapping a special case 2 new lines for `# fmt: off` and `# fmt: on` just feels clunky and looks ugly.

Even fixing it so that this style of comment works for the entire expression would a good improvement.
```python
matrix = [  # fmt: skip
    ...
```

My personal preference and recommendation, is that every comp with more than a single clauses be split up with each clause on its own line.
If the output expression includes any computation, it should also get its own line.

for example:
```python
Y = [x**2 for x in X]
Y = [
    x**2
    for x in X
    if x >= 0
]
Y = [
    x for x in X
    if x >=0
]
```

**An option to include line-breaks after each expression and clause would work for most cases.**


Still, I think there will always be some blocks which simply don't fit neatly into a rigid formatting rule, and require more careful though to keep them expressive and readable.

These examples defy simple conventions
```python
matrix = [
    (key + '-07', value / 10)
    for key in key_generator
        if key.startswith('a')
        and key.endswith('9')
    for value in value_list
        if value >= 1
]

# conditional expressions within the output expression
result = [
    x**4 if x > 4 
    else
    x**2 if x > 2 
    else x
    for x in range(10)
]

# computing intermediate values, and re-using expensive computations
result = [
    (x, doubled, squared)
    for x in values
    if (doubled := x*2) < 1
    if (squared := x**2) > .01
]
```

---

_Comment by @kevind-kizen on 2025-11-13 18:18_

@MichaReiser Any update on your implementation? I'd love to see really any movement on this as my new company uses the ruff formatter and this has been an important personal formatting rule that I've followed for years!

---

_Comment by @MichaReiser on 2025-11-14 07:57_

Sorry no. We've all been busy with other features/formatter styles

---

_Comment by @boxed on 2025-12-15 15:42_

Having used the Elm formatter for many years, it seems crazy that the formatter would ever join lines that use programmer thought were more readable as multiple lines. In the Elm formatter there are two "modes" for an expression: horizontal and vertical. If there are any newlines in the source code of the expression, then the vertical mode is chosen. 

This simple rule would mean the magic trailing comma rule would be obsolete (and it's not really generally useable as some expressions don't allow a trailing comma), covers this ticket, #12870, #18436, #9393, #14277, #12312, and probably many more that I didn't find. There are a lot of issues with titles like "disable ruff on..." which I assume some of them are actually due to this.

---

_Comment by @boxed on 2025-12-16 14:01_

@Martin19037 I have this feature implemented here: https://github.com/boxed/ruff/ (with binaries for macOS and windows: https://github.com/boxed/ruff/releases/tag/skip-line-joining-1.0). I tried it myself on my work project that currently is not formatted with ruff and I think with this change ruff is acceptable to me, while before it was not. 

---

_Comment by @lanqion on 2026-01-05 08:43_

A minimally intrusive workaround to preserve the line breaks is to add an empty comment (i.e. a single `#`) before one of the line breaks:

```python
[
    i + n  #
    for i in range(5)
    for n in range(5)
    if i < 3
]
```



---

_Comment by @lundybernard on 2026-01-05 20:58_

> A minimally intrusive workaround to preserve the line breaks is to add an empty comment (i.e. a single `#`) before one of the line breaks:


I agree, this is a usable workaround.  
I recommend making the comment `# nofmt` so that it is clear to readers why the random comment exists.

---
