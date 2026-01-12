```yaml
number: 1538
title: "Add support for `\\L` and `\\U` regex replace modifiers"
type: issue
state: closed
author: zx8
labels:
  - question
assignees: []
created_at: 2020-04-02T11:14:58Z
updated_at: 2020-04-03T23:33:24Z
url: https://github.com/BurntSushi/ripgrep/issues/1538
synced_at: 2026-01-12T16:13:23Z
```

# Add support for `\L` and `\U` regex replace modifiers

---

_@zx8_

I've been switching out more and more tools with single invocations of `rg` recently, and using `--replace` to great effect.

Today I tried the following, assuming it would convert my matches to lowercase/uppercase. Given the following file, `1.txt`:

```
foo
BAR
baz
```

**Expected Behaviour**

```sh
$ rg '(BAR)' -r '\L$1' 1.txt
2:bar

$ rg '(foo)' -r '\U$1' 1.txt
1:FOO
```

**Actual Behaviour**

```sh
$ rg '(BAR)' -r '\L$1' 1.txt
2:\LBAR

$ rg '(foo)' -r '\U$1' 1.txt
1:\Ufoo
```

---

It would be _really_ cool if ripgrep supported these modifiers when replacing!

---

_Comment by @BurntSushi on 2020-04-03 23:33_

I've given this some thought, and I'm not sure I want to step down this road. I'd really like to keep ripgrep focused on searching. The replacement flag is a simple convenience that catches a lot of use cases, and comes at minimal cost since it's implemented in the regex engine itself. I'd really like to avoid both re-implementing that feature _and_ introducing another vector for extending the DSL with additional features. It's easy to see how folks will want more and more functions until ripgrep contains some subset of awk's initial basis.

I think a good replacement tool would be great to build on top of ripgrep's libraries, and I encourage folks to do that. But I don't think expanding this functionality in ripgrep is a good fit at this time.

---

_Closed by @BurntSushi on 2020-04-03 23:33_

---

_Label `question` added by @BurntSushi on 2020-04-03 23:33_

---
