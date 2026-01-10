```yaml
number: 9471
title: Base set RUF100 positive on a pragma for a superset rule
type: issue
state: open
author: arthur-st
labels:
  - question
assignees: []
created_at: 2024-01-11T16:01:09Z
updated_at: 2024-01-15T07:01:13Z
url: https://github.com/astral-sh/ruff/issues/9471
synced_at: 2026-01-10T11:09:51Z
```

# Base set RUF100 positive on a pragma for a superset rule

---

_Issue opened by @arthur-st on 2024-01-11 16:01_

Let's imagine a repository structure like this:

```
base/.ruff.toml
.ruff.toml
main.py
```

Where `base/.ruff.toml` is a stock config file and `.ruff.toml` is this:
```
extend = 'base/.ruff.toml
extend-select =  ["FBT"]
```

Imagine then a 2-stage CI setup that blocks commits violating `base/.ruff.toml`, and merely annotates, in a non-blocking way, commits violating `.ruff.toml`. If someone adds a `# noqa: FBT001` pragma to relevant code in `main.py`, `base/.ruff.toml` starts blocking `main.py` commits based on `RUF100`, since the directive in the pragma is unused, due to the rule not being enabled in `base/.ruff.toml`.

What are the options here? Presently, I'm “forwarding” `RUF100` into the outermost Ruff configuration declaration, but that is a functionality compromise.


Somewhat related, another question/feature request. Would it be possible to namespace Ruff output by config file?

Consider the following config structure:
```
.ruff.toml
foo/.ruff.toml
foo/bar/.ruff.toml
```

Imagine that config files import config from the parent folder, in a daisy chain. What I would like to see is Ruff output specifying the namespace of each linting message, e.g., `root:RUF001`, `foo:FBT001`,  and `bar:PIE800`.

---

_Comment by @charliermarsh on 2024-01-14 02:54_

Hmm... I don't know that there are any better options here as compared to what you're doing already. In general, we can't really know what the superset of rules would be for any given file.

---

_Label `question` added by @charliermarsh on 2024-01-14 02:54_

---

_Comment by @arthur-st on 2024-01-15 07:01_

Fair enough. I'm thinking through the question myself for another turn, and there really isn't a robust way to tell what is a superset for an “out-of-path”. On a linear (well, directly nested) path it's a bit less arbitrary, but I wouldn't be surprised to learn that there can be nuanced setups for that as well.

In practical terms, then, I think what I've come up with is good enough. The annotations are surfaced prominently, which is good enough in these specific circumstances for a rule like RUF100.

What about the second question?

I was imagining a solution like walking back through all config imports, getting their labels through config or runtime generation, and then building the scope up via extensions, e.g.:
```
.ruff.toml <- rules A1, A2
foo/.ruff.toml <-  import .ruff.toml, extend-select B1, B2
foo/bar/.ruff.toml <- import foo/.ruff.toml, extend-select C1, C2
```
resulting in a mapping like
```
root = A1, A2
foo = B1, B2
bar = C1, C2
```
that can then be used to label the output. But I'm not sure if this makes within the data model of Ruff, or if it has any overlaps with your plans for it.

---
