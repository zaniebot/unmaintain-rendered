```yaml
number: 7082
title: Expand if-expressions with leading and trailing comments
type: pull_request
state: closed
author: charliermarsh
labels:
  - formatter
assignees: []
base: main
head: charlie/if-exp
created_at: 2023-09-03T15:30:52Z
updated_at: 2023-09-04T08:40:02Z
url: https://github.com/astral-sh/ruff/pull/7082
synced_at: 2026-01-12T15:55:23Z
```

# Expand if-expressions with leading and trailing comments

---

_@charliermarsh_

## Summary

Black appears to expand if-expressions when they contain leading or trailing own-line comments, if they're the only expression in a set of brackets (parenthesized, or the only element in a list or tuple) -- see the [playground](https://black.vercel.app/?version=main&state=_Td6WFoAAATm1rRGAgAhARYAAAB0L-Wj4ALtAKtdAD2IimZxl1N_WlTWs6ra1iDKWjAPAF8A1eA3JJuhoRcrxg1GcFUyUKdBgvXAKf9vb2b2GNeOKaqRVL99qLhz_7CFFeh2p2wszSTX0xlzDVNKB5fS1mxpy8SWRl56WbjlWd1hmvliVVZomrrTLj3M3Xyu0RL7AZkbyqHEmsw9aiZ62mj0ufah9LqO-SCSCXgKLE-XDbDdUO1w3hw8Ou6v1uLDy_tFArdKueDmAAAAbEf-eL11oJwAAccB7gUAAIEns8KxxGf7AgAAAAAEWVo=).

This PR applies the same logic to our own if-expression formatting.

Closes https://github.com/astral-sh/ruff/issues/7066.

No change in similarity:

| project      | similarity index  | total files       | changed files     |
|--------------|------------------:|------------------:|------------------:|
| cpython      |           0.76083 |              1789 |              1632 |
| django       |           0.99957 |              2760 |                67 |
| transformers |           0.99927 |              2587 |               468 |
| twine        |           0.99982 |                33 |                 1 |
| typeshed     |           0.99978 |              3496 |              2173 |
| warehouse    |           0.99818 |               648 |                24 |
| zulip        |           0.99942 |              1437 |                32 |


---

_Label `formatter` added by @charliermarsh on 2023-09-03 15:30_

---

_Converted to draft by @charliermarsh on 2023-09-03 15:54_

---

_Comment by @charliermarsh on 2023-09-03 16:01_

It appears that this only applies when the `if-exp` is the only expression within the brackets (i.e., it's a single argument, or a single element in a list literal).

Black's preview style seems more consistent in that it always expands them, but it also parenthesizes whenever they expand (even if it's, e.g., a single function argument).

---

_Comment by @charliermarsh on 2023-09-03 16:01_

This probably needs a decision then as to whether we want to implement the preview style (always expand, as we are here, but also parenthesize) or the non-preview style (only expand like this if there are no other elements in the brackets).

---

_Marked ready for review by @charliermarsh on 2023-09-03 16:15_

---

_Comment by @charliermarsh on 2023-09-03 16:15_

Alright, I went with Black's non-preview style.

---

_Converted to draft by @charliermarsh on 2023-09-03 17:58_

---

_Comment by @charliermarsh on 2023-09-03 18:06_

Here's the change that modified Black's handling in preview mode, to parenthesize these more aggressively: https://github.com/psf/black/pull/2278. (We don't yet implement this, but we could.)

Note that if we implement Black's preview style, we should be sure to avoid a few of these cases of "unnecessary parentheses", which look like they plan to change in Black too: https://github.com/psf/black/issues/3545.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-03 18:08_

---

_Marked ready for review by @charliermarsh on 2023-09-03 18:08_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_if_exp.rs`:69 on 2023-09-04 07:14_

We already track whether we are inside of parentheses in `NodeLevel`

```suggestion
                && f.context().node_level().is_parenthesized()
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_if_exp.rs`:157 on 2023-09-04 07:15_

```suggestion
```

---

_@MichaReiser reviewed on 2023-09-04 07:19_

The code change looks good to me but I'm not convinced that we want to implement this behavior. 

Our principal is to only expand nodes if their content contain any comments. This isn't the case here because the comment belongs to the `ExprIf` and not the value. Meaning, we now format `ExprIf` inconsistently with any other node. I also don't see a reason why splitting helps with readability except if you argue that conditionals should always be split over multiple lines. 

This makes me wonder if this is just unintentional formatting in black and whether it is worth aligning our formatting .

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/expr_if_exp.rs`:69 on 2023-09-04 08:10_

I don’t think this is quite the same — the condition here is “parenthesized or the only item in a set of brackets (e.g., the only item in a tuple literal or list literal or call).”

---

_@charliermarsh reviewed on 2023-09-04 08:10_

---

_Comment by @charliermarsh on 2023-09-04 08:11_

Yeah, you’re probably right and I don’t feel strongly, I just biased towards fixing a deviation. We can close.

Should we implement Black’s preview style to always parenthesize these when split? It seems like a significant improvement for default function parameters. (Can consider separately.)

---

_Closed by @charliermarsh on 2023-09-04 08:11_

---

_Comment by @MichaReiser on 2023-09-04 08:38_

> Should we implement Black’s preview style to always parenthesize these when split? It seems like a significant improvement for default function parameters. (Can consider separately.)

Preview style formatting is tracked here https://github.com/astral-sh/ruff/issues/6935 and should generally be gated behind a preview flag.

---

_Branch deleted on 2023-09-04 08:40_

---
