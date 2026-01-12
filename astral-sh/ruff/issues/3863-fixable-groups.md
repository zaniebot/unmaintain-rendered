```yaml
number: 3863
title: fixable groups
type: issue
state: closed
author: ghost
labels:
  - core
assignees: []
created_at: 2023-04-03T13:45:58Z
updated_at: 2023-10-06T03:41:45Z
url: https://github.com/astral-sh/ruff/issues/3863
synced_at: 2026-01-12T15:54:44Z
```

# fixable groups

---

_@ghost_

How about supporting different groups of fixables in the configuration file?
So you can e.g. add a group for safe fixes that you always want to be fixed automatically like e.g. sorting imports.
And another group for fixes you only want applied automatically when you are focusing on improving code quality.

CLI could look something like this, e.g. for a group called "safe" defined in the config:
`ruff check --fix safe .`



---

_Label `core` added by @charliermarsh on 2023-04-04 02:26_

---

_Comment by @evanrittenhouse on 2023-04-09 20:26_

I can start looking into this - work's a bit busy, so if it's urgent I may ask to be unassigned.

As far as the `.toml` changes required, I'm thinking something like this.
```toml
# ...
[fixable_groups]
safe = ["T001", "T002", "T003"]
code_quality = ["T001", "T004", "T006", "invalid_rule]
# ...
```
Groups shouldn't be mutually exclusive. Usage could be something like:
```shell
ruff check --fix code_quality .
```
If a rule is invalid, like in the case of `code_quality`, we should simply print a warning for that rule and perform autofixes on the valid rules in the group.

Let me know if that sounds like a decent design - if so, I can get started on implementation.

E: We could also do `--fix-group` instead of `--fix` - I'm trying to get a handle on the CLI implementation, but tough so far. If `--fix-group` would be easier than overriding `--fix`, would be happy to do it that way instead.

---

_Comment by @Avasam on 2023-04-13 14:08_

This could probably be achieved through different config files and `extend` if  extend supported an array. (though you could just call ruff multiple times instead of having an "all" config).

`base.ruff.toml`
```toml
# ...
```

`safe.ruff.toml`
```toml
extend = "./base.ruff.toml"
extend-select = ["T001", "T002", "T003"]
```

`code_quality.ruff.toml`
```toml
extend = "./base.ruff.toml"
extend-select = ["T001", "T004", "T006"]
```

`pyproject.toml` (all)
```toml
[tool.ruff]
extend = ["./safe.ruff.toml", "./code_quality.ruff.toml"]
```

---

`ruff check --config ./safe.ruff.toml`
`ruff check --config ./code_quality.ruff.toml`
`ruff check` (defaults to `pyproject.toml`)

---

_Comment by @ghost on 2023-04-13 14:16_

Hi everyone,

thanks for pointing out a way to do this right now, Avasam!
In the long run I'd prefer the solution suggested by evanrittenhouse, as it is a lot easier to understand und requires fewer files.
Thank you for considering to implement this!

---

_Comment by @evanrittenhouse on 2023-04-14 02:39_

Cool. I'll get to work on implementing this

---

_Comment by @charliermarsh on 2023-04-14 02:48_

@evanrittenhouse - Before we start on implementation, I think we should make sure that we're aligned on design -- I want to make sure you don't sink time into something only for us to run into disagreements on core API and such later on :)

In the proposal here, is the idea such that users are responsible for defining those fix groups? I had historically thought of fixable levels as core metadata on the rule itself, e.g., you could have rules that are considered very safe to fix, rules that are experimental, rules that are risky and so may change semantics. If that were the case, users wouldn't be required to define fix groups. Instead, we could just define them in Ruff, and then users could specify an autofix safety level when running with `--fix`. Do you think that would be too inflexible?

(I also want to \cc @MichaReiser who's been thinking about configuration broadly and may well disagree with what I've written here :))


---

_Comment by @evanrittenhouse on 2023-04-14 03:00_

In my opinion, I like the idea of user-defined groups _somewhere_ down the line. Safety levels would certainly be a helpful piece of information for rules, and I think it's quite valuable to add, but I wonder if that alone could shoehorn users into checks they don't want/need. They'd also allow us to only compare rules on one dimension (safe, unsafe, etc.), though we could probably get around that by implementing something like rule "traits" instead (handles imports, handles trailing commas, etc.).

For example, you could gather all rules around imports and run them together, or @xyxz-web's suggestion around "code quality" rules which could vary on a per-user basis.

Very open to more discussion on the design though! I haven't been around as long as some other folks so I definitely could be missing some stuff. 

---

_Comment by @ghost on 2023-04-14 07:46_

Personally I'd prefer to have the flexibility to define my own groups, though having a way to figure out which fixes are considered safe would also be very helpful.

---

_Closed by @zanieb on 2023-10-06 03:41_

---
