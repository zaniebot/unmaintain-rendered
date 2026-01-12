```yaml
number: 3127
title: "Glob `[foo` is rejected"
type: issue
state: closed
author: martinvonz
labels:
  - enhancement
  - rollup
assignees: []
created_at: 2025-08-15T16:03:25Z
updated_at: 2025-09-20T01:08:49Z
url: https://github.com/BurntSushi/ripgrep/issues/3127
synced_at: 2026-01-12T16:13:25Z
```

# Glob `[foo` is rejected

---

_@martinvonz_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

### How did you install ripgrep?

APT

### What operating system are you using ripgrep on?

Ubuntu

### Describe your bug.

Glob `[foo` should be interpreted as a literal `[foo`.

### What are the steps to reproduce the behavior?

1. Run `rg -g '[foo' whatever`

### What is the actual behavior?

```
$ rg -g '[foo' whatever
rg: error parsing glob '[foo': unclosed character class; missing ']'
```


### What is the expected behavior?

It should return matches of "whatever" in the file called exactly `[foo`. `man glob.7` doesn't seem to mention any invalid patterns. It seems that e.g. `find -name '[foo'` interprets it that way. Gitignores also do, apparently (this came up in https://github.com/jj-vcs/jj/issues/7259).

---

_Comment by @BurntSushi on 2025-08-15 17:06_

`globset` doesn't conform to any known specification. And it should by default reject malformed globa like this. I reject the Unix tradition of "make sense of anything." However, there should be an opt-in mode for this, if only to make `ignore` work correctly. There are 1 or 2 other bugs like this as a result of inconsistent glob implementations (like curly braces).

---

_Label `bug` added by @BurntSushi on 2025-08-15 17:06_

---

_Label `bug` removed by @BurntSushi on 2025-08-15 17:06_

---

_Label `enhancement` added by @BurntSushi on 2025-08-15 17:06_

---

_Comment by @martinvonz on 2025-08-15 17:18_

Thanks for the update. I don't think I myself will have time to add that mode any time soon. Hopefully someone else will. Maybe someone who's affected by it.

---

_Comment by @mostafa630 on 2025-09-11 13:25_

@BurntSushi 
Hi, can I take this issue?

From my understanding, if the input is something like [abc, we should reject it because it represents an unclosed character class.
In this case, the characters would be treated as literals: '[', 'a', 'b'.

We can handle this by checking whether the remaining input contains a closing ] at start of  `parse_class` function . If not, we can return `ErrorKind::UnclosedClass` Then in the caller  when we detect that error  we could fall back to treating '[' as a literal by doing something like: 
```
'[' => match self.parse_class() {
    Ok(()) => {}
    Err(error) => match error.kind {
        ErrorKind::UnclosedClass => self.push_token(Token::Literal('['))?,
        _ => return Err(error),
    },
},
```
There’s a small edge case to consider: for example, if the input is [!].
Here, the class is technically closed, but its content may not be valid. We need to decide whether to treat this as an invalid class or  interpreting the characters as literals.

---

_Comment by @BurntSushi on 2025-09-11 13:30_

Sure, go for it. Remember that this should be an _opt-in_ behavior though.

---

_Comment by @mostafa630 on 2025-09-12 20:22_

@BurntSushi 
I’ve completed the changes for this PR: https://github.com/BurntSushi/ripgrep/pull/3145
All tests have passed. Please let me know if any further adjustments are needed.

---

_Label `rollup` added by @BurntSushi on 2025-09-19 21:01_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
