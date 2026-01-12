```yaml
number: 1002
title: Is it possible to search file names and content at same time with same regex?
type: issue
state: closed
author: jessegrosjean
labels:
  - question
assignees: []
created_at: 2018-08-04T16:08:07Z
updated_at: 2019-09-25T18:06:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1002
synced_at: 2026-01-12T16:13:22Z
```

# Is it possible to search file names and content at same time with same regex?

---

_@jessegrosjean_

I'd like to search for a pattern in both file name and content. Ideally at the same time. Is this possible, if not is there a recommended workaround?

---

_Comment by @BurntSushi on 2018-08-04 16:20_

No. ripgrep doesn't even expose a way to search file paths with a regex at all. The only way to match on file paths is with globs. If you want regexes, you need to use pipelines, e.g., `rg --files | rg 'foo\w+'`.

I don't think I can suggest a work around because the desired semantics aren't clear. What problem are you trying to solve? What is your input? What's your desired output?

---

_Label `question` added by @BurntSushi on 2018-08-04 16:20_

---

_Comment by @jessegrosjean on 2018-08-04 17:54_

> If you want regexes, you need to use pipelines, e.g., `rg --files | rg 'foo\w+'`.

I'm using ripgrep to implement file search in my app. I want the search results to include files if name OR content matches the pattern.

I can use your suggestion to solve the problem, I just wanted to make sure I wasn't missing a more direct built in solution.

Thanks!

---

_Closed by @jessegrosjean on 2018-08-04 17:54_

---

_Comment by @zachliu on 2019-09-25 17:25_

@jessegrosjean @BurntSushi What if I want the search results to include files if name AND content matches the pattern?

---

_Comment by @BurntSushi on 2019-09-25 17:46_

@zachliu `rg content-pattern | rg file-pattern` will get you pretty close. Or just do `rg -g 'file-glob-pattern' content-pattern`.

---

_Comment by @zachliu on 2019-09-25 18:06_

@BurntSushi many thanks :+1: 

---
