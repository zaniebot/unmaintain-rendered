```yaml
number: 3109
title: "[`tryceratops`]: Check to continue"
type: pull_request
state: closed
author: colin99d
labels:
  - rule
assignees: []
base: main
head: check_to_continue
created_at: 2023-02-22T03:11:28Z
updated_at: 2023-02-27T00:11:44Z
url: https://github.com/astral-sh/ruff/pull/3109
synced_at: 2026-01-12T15:55:12Z
```

# [`tryceratops`]: Check to continue

---

_@colin99d_

#2056 

---

_Label `rule` added by @charliermarsh on 2023-02-22 03:12_

---

_Comment by @colin99d on 2023-02-23 00:59_

@charliermarsh I am almost done, I just need to know what to make this rule run on. It looks like [the original rule](https://github.com/guilatrova/tryceratops/blob/main/src/tryceratops/analyzers/call.py#L140) was hooked into the Module AST item, but I dont see that option in ruff.

---

_Comment by @charliermarsh on 2023-02-24 18:53_

@colin99d - So I haven't looked at the rule deeply, but `Module` is just the top level of the AST: the module body, a list of statements. I guess this rule wants to do a full AST traversal, but we typically have `ast.rs` do the traversal and call out to the rules at the appropriate time.

---

_Comment by @colin99d on 2023-02-25 17:29_

Ok I got it figured out! Just need to make one small change with the line number I report, and this will be good to go.

---

_Marked ready for review by @colin99d on 2023-02-25 18:15_

---

_Comment by @charliermarsh on 2023-02-26 23:56_

I'm really very very genuinely sorry to do this again, but... I don't think we should merge this rule. It's marked as experimental in `tryceratops`, and while I try to avoid injecting too much of my own opinion into which rules are accepted, I feel like this rule is sort of misguided in that handling expected failure modes explicitly can be clearer and _more_ readable than hiding them under exception handling.

I know I asked for help closing out the `tryceratops` ticket, so I feel extra bad closing this PR. I didn't do a good job of validating that we _actually_ wanted each of the remaining rules -- I tend not to look at them closely until I see a PR, but I will try to do better going forward.


---

_Closed by @charliermarsh on 2023-02-26 23:56_

---

_Comment by @colin99d on 2023-02-27 00:09_

@charliermarsh, I was actually going to start the description of this rule with "I am morally against this rule, but here it is anyway". I definitly agree that this rule makes code worse.

---

_Comment by @charliermarsh on 2023-02-27 00:11_

Lol

---
