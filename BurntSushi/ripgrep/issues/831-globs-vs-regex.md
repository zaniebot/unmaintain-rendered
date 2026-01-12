```yaml
number: 831
title: Globs vs regex?
type: issue
state: closed
author: wcpines
labels:
  - question
assignees: []
created_at: 2018-02-22T23:45:53Z
updated_at: 2018-02-23T00:05:04Z
url: https://github.com/BurntSushi/ripgrep/issues/831
synced_at: 2026-01-12T16:13:22Z
```

# Globs vs regex?

---

_@wcpines_

#### What version of ripgrep are you using?

```
ripgrep 0.6.0
-AVX -SIMD
```

#### What operating system are you using ripgrep on?

``` System Version: macOS 10.13.3 (17D47)
      Kernel Version: Darwin 17.4.0
      Boot Volume: Macintosh HD
```


#### Describe your question, feature request, or bug.

I'm just not really grasping the difference between 

```rg -g '*something*' 'def some_method_somewhere'```

and 

```rg -g '.*something.*' 'def some_method_somewhere'```

Should I assume globs work like Unix and `*` are wildcards?   

Also, do double quotes behave the same way?  



---

_Comment by @BurntSushi on 2018-02-22 23:52_

> Should I assume globs work like Unix and `*` are wildcards?

Yes. As documented:

>     -g, --glob <GLOB>...                    
>            Include or exclude files and directories for searching that match the given
>            glob. This always overrides any other ignore logic. Multiple glob flags may be
>            used. Globbing rules match .gitignore globs. Precede a glob with a ! to exclude
>            it.

Globs in `.gitignore` files are indeed standard Unix globs. The syntax is documented exhaustively here: https://docs.rs/globset/0.3.0/globset/#syntax

> Also, do double quotes behave the same way?

I don't think I can answer this question. It depends on your shell. Bash, for example, most definitely treats `'...'` differently than `"..."`.

---

_Comment by @wcpines on 2018-02-22 23:57_

Thanks for such a fast reply, wow.  Very helpful. 

---

_Label `question` added by @BurntSushi on 2018-02-23 00:05_

---

_Closed by @BurntSushi on 2018-02-23 00:05_

---
