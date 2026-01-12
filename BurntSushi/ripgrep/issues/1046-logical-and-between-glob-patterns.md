```yaml
number: 1046
title: Logical AND between glob patterns
type: issue
state: closed
author: strindberg
labels: []
assignees: []
created_at: 2018-09-11T03:59:50Z
updated_at: 2018-09-11T14:30:45Z
url: https://github.com/BurntSushi/ripgrep/issues/1046
synced_at: 2026-01-12T16:13:22Z
```

# Logical AND between glob patterns

---

_@strindberg_

#### What version of ripgrep are you using?
ripgrep 0.10.0

I might be missing something, but I can't figure out a way to make ripgrep treat multiple glob patterns with logical AND. The background is, I have a wrapper script combining input from the user to set up conditions for which files to search. This wrapper script today uses find and grep, and I was interested in trying to use ripgrep instead.

What I am trying to achieve is for example: "all files with the string 'warehouse' in its name AND the file ending 'properties' should be searched". Now, in this case I could express it as a single glob, but since there might be a lot more conditions, the combinatorics quickly become unwieldy.

As far as I can see, the invocation ripgrep --glob '*warehouse*' --glob '*properties' matches all files that match EITHER of the globs, and I would need away to specify that BOTH must match.


---

_Comment by @BurntSushi on 2018-09-11 11:42_

I suspect this type of complicated filtering would be more appropriately done by `find`. For example, GNU grep's recursive search doesn't have the feature you're requesting either, and instead just has basic disjunctive `--include` rules.

---

_Comment by @strindberg on 2018-09-11 12:16_

Yes, you're probably right. Every tool should be an expert in its own field. However, I can see no easy way to combine ripgrep with find, i.e. use my existing find filters and feed it into ripgrep. I would like an option saying "use this list of files and don't recurse". Am I missing something and this is already possible?


---

_Comment by @BurntSushi on 2018-09-11 12:24_

@strindberg Ah. You do it the same way you would combine `find` with `grep`. e.g., `find ./ -name 'wat' -print0 | xargs -0 rg <pattern>`.

---

_Comment by @strindberg on 2018-09-11 14:22_

Oh, now I see. I was confused by the automatic recursive mode of ripgrep, and the fact that my find filter sometimes passed on directories. Making sure I only pass non-directories solved the problem. In a use case like mine, an option to turn off recursion in ripgrep would actually help a bit, but I can work around it.

---

_Comment by @BurntSushi on 2018-09-11 14:26_

@strindberg Glad you figured it out! And yeah, I forgot to include the `-type f` condition, which you probably also want with `grep` as well, but grep does print a helpful error message. 

---

_Closed by @BurntSushi on 2018-09-11 14:26_

---

_Comment by @strindberg on 2018-09-11 14:30_

Yes. grep has a -dskip option which automatically drops directories, which is useful. The reason why it is harder than it sounds to weed out directories is that if you use the "-prune" option in find, it is a bit complex to make sure that the right directories are pruned, but the resulting (printed) files are only just files.

---
