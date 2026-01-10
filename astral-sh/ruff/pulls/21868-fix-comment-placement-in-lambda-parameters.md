```yaml
number: 21868
title: Fix comment placement in lambda parameters
type: pull_request
state: merged
author: ntBre
labels:
  - bug
  - formatter
assignees: []
merged: true
base: main
head: brent/lambda-parameter-comments
created_at: 2025-12-09T17:35:34Z
updated_at: 2025-12-09T19:07:50Z
url: https://github.com/astral-sh/ruff/pull/21868
synced_at: 2026-01-10T16:42:11Z
```

# Fix comment placement in lambda parameters

---

_Pull request opened by @ntBre on 2025-12-09 17:35_

Summary
--

This PR makes two changes to comment placement in lambda parameters. First, we
now insert a line break if the first parameter has a leading comment:

```py
# input
(
    lambda
    * # comment 2
    x:
    x
)

# main
(
    lambda # comment 2
    *x: x
)

# this PR
(
    lambda
	# comment 2
    *x: x
)
```

Note the missing space in the output from main. This case is currently unstable
on main. Also note that the new formatting is more consistent with our stable
formatting in cases where the lambda has its own dangling comment:

```py
# input
(
    lambda # comment 1
    * # comment 2
    x:
    x
)

# output
(
    lambda  # comment 1
    # comment 2
    *x: x
)
```

and when a parameter without a comment precedes the split `*x`:

```py
# input
(
    lambda y,
    * # comment 2
    x:
    x
)

# output
(
    lambda y,
    # comment 2
    *x: x
)
```

This does change the stable formatting, but I think such cases are rare (expecting zero hits in the ecosystem report), this fixes an existing instability, and it should not change any code we've previously formatted.

Second, this PR modifies the comment placement such that `# comment 2` in these
outputs is still a leading comment on the parameter. This is also not the case
on main, where it becomes a [dangling lambda comment](https://play.ruff.rs/3b29bb7e-70e4-4365-88e0-e60fe1857a35?secondary=Comments). This doesn't cause any
instability that I'm aware of on main, but it does cause problems when trying to
adjust the placement of dangling lambda comments in #21385. Changing the
placement in this way should not affect any formatting here.

Test Plan
--

New lambda tests, plus existing tests covering the cases above with multiple
comments around the parameters (see lambda.py 122-143, and 122-205 or so more
broadly)

I also checked manually that the comments are now leading on the parameter:

```shell
❯ cargo run --bin ruff_python_formatter -- --emit stdout --target-version 3.10 --print-comments <<EOF
(
    lambda
        # comment 2
    *x: x
)
EOF
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.15s
     Running `target/debug/ruff_python_formatter --emit stdout --target-version 3.10 --print-comments`
# Comment decoration: Range, Preceding, Following, Enclosing, Comment
21..32, None, Some((Parameters, 37..39)), (ExprLambda, 6..42), "# comment 2"
{
    Node {
        kind: Parameter,
        range: 37..39,
        source: `*x`,
    }: {
        "leading": [
            SourceComment {
                text: "# comment 2",
                position: OwnLine,
                formatted: true,
            },
        ],
        "dangling": [],
        "trailing": [],
    },
}
(
    lambda
    # comment 2
    *x: x
)
```

But I didn't see a great place to put a test like this. Is there somewhere I can assert this comment placement since it doesn't affect any formatting yet? Or is it okay to wait until we use this in #21385?


---

_Label `formatter` added by @ntBre on 2025-12-09 17:35_

---

_Label `bug` added by @ntBre on 2025-12-09 17:35_

---

_Comment by @astral-sh-bot[bot] on 2025-12-09 17:48_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Marked ready for review by @ntBre on 2025-12-09 17:58_

---

_Review requested from @MichaReiser by @ntBre on 2025-12-09 17:58_

---

_@MichaReiser approved on 2025-12-09 18:58_

---

_Merged by @ntBre on 2025-12-09 19:07_

---

_Closed by @ntBre on 2025-12-09 19:07_

---

_Branch deleted on 2025-12-09 19:07_

---
