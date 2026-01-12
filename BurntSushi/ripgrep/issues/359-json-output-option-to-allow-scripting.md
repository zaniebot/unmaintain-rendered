```yaml
number: 359
title: json output option to allow scripting
type: issue
state: closed
author: timotheecour
labels: []
assignees: []
created_at: 2017-02-12T06:28:05Z
updated_at: 2017-02-13T03:19:32Z
url: https://github.com/BurntSushi/ripgrep/issues/359
synced_at: 2026-01-12T16:13:21Z
```

# json output option to allow scripting

---

_@timotheecour_

Could we have a json output option?
use case: making it easier to integrate in other tools [1]
eg: 

```
rg '\bWORLD\b' $args --output=json
{"path":"foo/bar.txt","line":3,"offset":258,"column":4,"length":5,"match":"WORLD" ,"context":"hello WORLD !" }
 ```
offset corresponds to byte offset in file (useful for efficient access for the other tool)
length is in bytes (in case of non-ascii)

The alternative (manual parsing) is error prone, eg:
* if filename is `foo/bar:13:foo.txt` (contains `:`)
*  if the escape codes colorizing the matched word are hard to find because the context contains some escape codes itself (eg it can actually happens if it's a log file)
* relying on --column doesn't work for multiple matches in 1 line (ok just saw --vimgrep but doens't show colors, so we can't get the extent of each match)

[1] NOTE: one of these other tools is multiline search built on top of ripgrep




---

_Comment by @BurntSushi on 2017-02-12 15:26_

I think what you're asking for is "structured output," where JSON might be your preference. In that case, I think this is a dupe of #244. To be clear, I do *not* want to be in the business of producing a bunch of different output formats because there is so much complexity involved. (Every option that influences output has to be considered against the new format.)

> NOTE: one of these other tools is multiline search built on top of ripgrep

While I'm in favor of adding some type of structured output to ripgrep, I would rather see these types of tools built with [libripgrep](https://github.com/BurntSushi/ripgrep/milestone/1) than with ripgrep itself.

---

_Comment by @timotheecour on 2017-02-13 01:21_

@BurntSushi 

* since these structured output (json or not, doesn't matter) will be meant  for machine consumption, it would make sense to keep options to a bare minimum for this mode: ignore color, and have a single formatting option (doesn't matter so long all information is there to get to the match; byte offset as i said above would be useful, to allow efficient machine access to a match without having to parse the file).

* One option maybe: whether or not to show the corresponding matching line of text from the file.

* is there any doc on libripgrep, maybe a code snippet? can I link against it say from C-like languages?


---

_Comment by @BurntSushi on 2017-02-13 03:19_

Then this should be closed as a duplicate.

There are *many* options, and their effects all need to be individually reasoned through.

> is there any doc on libripgrep, maybe a code snippet?

Please see the link in my previous comment. It's not done yet. Libripgrep refers to a collection of libraries. The endgame is to make ripgrep itself very small with most logic in reusable libraries. A lot of it is already done.

> can I link against it say from C-like languages?

Someone (probably not me) will need to put in the work to build a C interface, and then ffi bindings for their favorite language.

---

_Closed by @BurntSushi on 2017-02-13 03:19_

---
