```yaml
number: 20561
title: "[`ruff`] improve handling of intermixed comments inside from-imports"
type: pull_request
state: merged
author: amyreese
labels:
  - bug
  - formatter
  - style
assignees: []
merged: true
base: main
head: amy/import-dangling-comments
created_at: 2025-09-24T20:51:15Z
updated_at: 2025-10-07T16:35:55Z
url: https://github.com/astral-sh/ruff/pull/20561
synced_at: 2026-01-10T17:34:34Z
```

# [`ruff`] improve handling of intermixed comments inside from-imports

---

_Pull request opened by @amyreese on 2025-09-24 20:51_

Resolves the crash when attempting to format code like:

```
from x import (a as # whatever
b)
```

And chooses to format it as:
```
from x import (
    a as b,  # whatever
)
```

Fixes issue #19138


---

_Review requested from @MichaReiser by @amyreese on 2025-09-24 20:51_

---

_Comment by @github-actions[bot] on 2025-09-24 20:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `bug` added by @MichaReiser on 2025-09-25 07:46_

---

_Label `formatter` added by @MichaReiser on 2025-09-25 07:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/alias.rs`:30 on 2025-09-25 07:57_

Passing 0 for the width is incorrect here. The width is used to determine how much space the `Printer` needs to reserve for the comments. Zero means that they take zero width (or the width is ignored) and they always fit if the content before fits on the line. 

The reason `line_suffix` has a width is that Ruff excludes pragma comments from the width calculation, but all other comments need to fit into the 88-character limit, or the line is split over multiple lines. 

You can use `trailing_comments` if you want to format the comments as end-of-line comments

---

_@MichaReiser requested changes on 2025-09-25 07:59_

Thanks for looking into this. We'll need to add tests (see `resources/fixtures`). I also suggest playing with different comment combinations. E.g. the following now flips the order of comments which is always undesired:

```py
from x import (a # more 
 as # whatever
b)
```

Gets formatted to

```py
from x import (
    a as b,  # more
    # whatever
)
```

This one doesn't flip the comment order but it places the `other` comment that used to be before `b` after `b`

```py

from x import (a
 as # whatever
 # other
b)

```

Not suggesting that you should or that it makes your life easier in any way. But an alternative fix is to update `place_comment` so that it makes comments between `as` and the first alias leading comments of the first alias (as was done in https://github.com/astral-sh/ruff/pull/20589)

---

_Comment by @amyreese on 2025-09-30 18:02_

Working on adding some test fixtures to validate results, and considering alternative approaches.

---

_Comment by @amyreese on 2025-09-30 19:19_

In this given example putting comments in every possible position, Black will maintain placement of all the comments, rather than re-flowing any elements together and merging comments:

```
# alpha
from x import (
    # bravo
    a  # charlie
    # delta
    as  # echo
    # foxtrot
    b  # golf
    # hotel
    ,  # india
    # juliet
)  # kilo
```

Do we want to ensure that we follow Black's style and preserve all of these comments in their original positions? Or do we want to have an opinion and re-flow/merge various comments to ensure that `a as b,` is always on one line when possible?

---

_Renamed from "[ruff] fix crash with dangling comments in importfrom alias" to "[ruff] fix crash with dangling comments in import from alias" by @MichaReiser on 2025-10-01 06:35_

---

_Comment by @MichaReiser on 2025-10-01 06:42_

> Do we want to ensure that we follow Black's style and preserve all of these comments in their original positions? Or do we want to have an opinion and re-flow/merge various comments to ensure that a as b, is always on one line when possible?

We've used Black's comment formatting as inspiration and we try to match it for "common" comment placements but it's not a strict requirement that we have to match black precisely (I also found Black inconsistent about when it tries to be smart/opinionated and when it's strict about preserving comment placements).

We do try to preserve own-line comments as own-line comments and end-of line comments should remain end-of-line comments. It's also important to preserve the ordering of comments. 

Trying your example with a few "similar" syntax elements (this is the formatted output):

```py
# alpha
(
    # bravo
    a  # charlie
    # delta
    +  # echo
    # foxtrot
    b,  # golf
    # hotel
    # india
    # juliet
)  # kilo

# scrambles own line and end of line comments

# alpha
with (
    # bravo
    a as (  # charlie
    # delta  # echo
        # foxtrot
        b
    ),  # golf
    # hotel
    # india
    # juliet
):  # kilo
    ...

match x:
    # alpha
    case (
        # bravo
        a  # charlie
        # delta
        as  # echo
        # foxtrot
        b,  # golf
        # hotel
        # india
        # juliet
    ):  # kilo
        ...

```

---

_Comment by @amyreese on 2025-10-01 20:17_

Updated to use `trailing_comments`. It did not seem feasible to make it fully preserve all comment positions like black does, but I ensured that comment order is at least preserved, and this provides the "opinion" of keeping `a as b` on the same line. Updated fixtures/snapshots with the example from yesterday.

---

_Review requested from @MichaReiser by @amyreese on 2025-10-01 20:17_

---

_@MichaReiser reviewed on 2025-10-02 06:24_

Does this change reflow comments of already formatted code?

Could we do something similar to the `MatchAs` formatting

https://github.com/astral-sh/ruff/blob/63b3f7adae7fcc8910baa902a2d20bfdc4a28ad9/crates/ruff_python_formatter/src/pattern/pattern_match_as.rs#L27-L44

I'm asking because the formatting of 

```py
from x import (a # more 
 as # whatever
b)


from x import (a
 as # whatever
 # other
b)
```

to 

```py
from x import (
    a as b,  # more
    # whatever
)


from x import (
    a as b,  # whatever
    # other
)
```

or

```py
# alpha
from x import (
    # bravo
    a
    
    as  # echo
    # foxtrot
    b  # golf
    # hotel
)
```

where the `foxdrot` comment now comes after `b` (it was a leading comment but it now becomes a trailing comment and the indent for `golf` is off)

```py
# alpha
from x import (
    # bravo
    a as b,  # echo
    # foxtrot
      # golf
    # hotel
)
```


The indent of `# golf` is problematic because this is now a case where the formatter needs two passes to reach convergence. We need to fix this at least and add them as test cases. 

---

_Comment by @amyreese on 2025-10-02 14:58_

Hmm, I'm seeing different results from you when I test this locally:

```
$ cargo run --bin ruff_python_formatter -- --emit stdout scratch.py
...

from x import (
    a as b,  # more  # whatever
)


from x import (
    a as b,  # whatever
    # other
)

# alpha
from x import (
    # bravo
    a as b,  # echo
    # foxtrot  # golf
    # hotel
)
```

---

_Comment by @MichaReiser on 2025-10-02 15:23_

Ah, sorry. I think pulling the changes this morning failed and I then tested against the previous revision. Adding those tests might still make sense because they demonstrate other issues that your PR fixes.

---

_Comment by @amyreese on 2025-10-03 14:53_

I'm revisiting the comment placement mechanisms, and attempting to better associate comments within an alias node to the identifiers they belong with.

---

_Comment by @amyreese on 2025-10-04 00:03_

Reworked the ways comments are associated, specifically for the `Alias` and `StmtImportFrom` nodes, preserving comment placement at every position within a from-import and matching black's formatting:

```
$ cargo run --bin ruff_python_formatter -- --emit stdout crates/ruff_python_formatter/resources/test/fixtures/black/cases/import_comments.py
   Compiling ruff_db v0.0.0 (/Users/amethyst/workspace/ruff/crates/ruff_db)
   Compiling ruff_python_formatter v0.0.0 (/Users/amethyst/workspace/ruff/crates/ruff_python_formatter)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 3.18s
     Running `target/debug/ruff_python_formatter --emit stdout crates/ruff_python_formatter/resources/test/fixtures/black/cases/import_comments.py`
# ensure trailing comments are preserved
import x  # comment
from x import a  # comment
from x import a, b  # comment
from x import a as b  # comment
from x import a as b, b as c  # comment

# ensure intermixed end- and own-line comments are all preserved
from x import (  # one
    # two
    a  # three
    # four
    ,  # five
    # six
)  # seven

from x import (  # alpha
    # bravo
    a  # charlie
    # delta
    as  # echo
    # foxtrot
    b  # golf
    # hotel
    ,  # india
    # juliet
)  # kilo
```

Passing ruff's output black confirms no changes:

```
$ cargo run -p ruff -- format - < crates/ruff_python_formatter/resources/test/fixtures/black/cases/import_comments.py | uvx black -
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.11s
     Running `target/debug/ruff format -`
# ensure trailing comments are preserved
import x  # comment
from x import a  # comment
from x import a, b  # comment
from x import a as b  # comment
from x import a as b, b as c  # comment

# ensure intermixed end- and own-line comments are all preserved
from x import (  # one
    # two
    a  # three
    # four
    ,  # five
    # six
)  # seven

from x import (  # alpha
    # bravo
    a  # charlie
    # delta
    as  # echo
    # foxtrot
    b  # golf
    # hotel
    ,  # india
    # juliet
)  # kilo
All done! ✨ 🍰 ✨
1 file left unchanged.
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:2003 on 2025-10-06 06:31_

Nit: The `place_comments` logic tends to be complicated and its often non obvious what cases each branch is handling. That's why we tend to add inline comments that show examples of the patterns they match on (see the comments above).

Could you add such comments for this branch and to `handle_alias_comment` too? 

```
// this should be a dangling comment but only if it comes before the `,`
// ```python
// from foo import (
//      baz # comment     
//      , 
//      bar 
// )
// ```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/other/alias.rs`:35 on 2025-10-06 06:32_


```suggestion
            write!(f, [token("as")])?;
```

---

_@MichaReiser approved on 2025-10-06 06:36_

Nice!

Given that we now preserve the original placement in more cases, it should also mean that we don't reformat any existing code (and, thus, doesn't require any preview gating)

---

_Comment by @amyreese on 2025-10-06 19:34_

Documented placement functions and changes, as well as in the alias formatter.  Also added more test cases and updated the logic to better match black style and merge lines/comments when no own-line comments are between the alias and the trailing comma:

Eg,
```py
from foo import (
    bar  # one
    ,  # two
```

Becomes:
```py
from foo import (
    bar,  # one  # two
)
```

Edit: to be clear, this change is also closer to the existing behavior of Ruff, so even though it changes formatting, it's no different than what Ruff already does *today*:

```
main $ cargo run -p ruff --  format --diff -
...

from foo import (
    bar  # comment
    ,  # another
    baz,
)
^D
@@ -1,5 +1,4 @@
 from foo import (
-    bar  # comment
-    ,  # another
+    bar,  # comment  # another
     baz,
 )

> [1]
```

---

_Merged by @amyreese on 2025-10-07 15:14_

---

_Closed by @amyreese on 2025-10-07 15:14_

---

_Branch deleted on 2025-10-07 15:14_

---

_Label `style` added by @amyreese on 2025-10-07 15:15_

---

_Renamed from "[ruff] fix crash with dangling comments in import from alias" to "[ruff] improve handling of intermixed comments inside from-imports" by @amyreese on 2025-10-07 16:29_

---

_Renamed from "[ruff] improve handling of intermixed comments inside from-imports" to "[`ruff`] improve handling of intermixed comments inside from-imports" by @amyreese on 2025-10-07 16:35_

---
