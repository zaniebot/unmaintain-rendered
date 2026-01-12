```yaml
number: 308
title: "--output like Ack (or: rethink --replace)"
type: issue
state: closed
author: mernen
labels:
  - doc
assignees: []
created_at: 2017-01-08T21:51:57Z
updated_at: 2017-01-10T21:43:45Z
url: https://github.com/BurntSushi/ripgrep/issues/308
synced_at: 2026-01-12T16:13:21Z
```

# --output like Ack (or: rethink --replace)

---

_@mernen_

First, a question: what exactly is the use case for `--replace`? I don't think I've ever seen any other tool offering this option, and I can't think of many uses.

I do, though, find Ack's `--output` useful every now and then. It's a similar feature, but it replaces the contents of the entire line rather than only the matched part. When coupled with `-h` (hiding line numbers and file names), it allows one to pipe the results to other tools to get statistics about the matches in ways I'm not sure how I'd achieve with ripgrep (or other competitors, for that matter).

```sh
$ # what are the std modules in use?
$ ack '\bstd::\w+' --output '$&' -h | sort | uniq -c | sort -nr
  29 std::path
  28 std::io
  18 std::os
  16 std::sync
  14 std::ffi
  10 std::error
   9 std::fs
   9 std::fmt
   7 std::str
   6 std::env
   5 std::cmp
   4 std::thread
   4 std::process
   4 std::mem
   4 std::collections
   3 std::borrow
   2 std::time
   2 std::result
   2 std::ops
   2 std::cell
   1 std::vec
   1 std::slice
   1 std::marker
   1 std::iter
   1 std::hash
   1 std::ascii
```

(ripgrep's `--replace` can be fully emulated in Ack using `` $` `` and ``$'`` in the output expression, but those perlisms are rather cryptic, and also pretty awkward to use on the CLI)

---

_Comment by @BurntSushi on 2017-01-08 22:02_

Ack's `--output` is isomorphic to ripgrep's `--replace`. Apparently, it works slightly differently. As you noticed, it enables one to replace *each* match if they want to. At the same time, it's pretty easy to replace the entire line as well. All you need to do is make the regex match the full line. So `\bstd::\w+` would be written as `^.*\bstd::\w+.*$`. To use `--replace`, you'd then want to create a capturing group as well. So, ripgrep's version of your `ack` command is:

```
$ rg '^.*(\bstd::\w+).*$' --replace '$1' --no-filename | sort | uniq -c | sort -nr
```

Which has the same output. The `--no-filename` is like ack's `-h` flag.

> (ripgrep's --replace can be fully emulated in Ack using $` and $' in the output pattern, but those perlisms are rather cryptic, and also pretty awkward to use on the CLI)

Hmm, yes, I'm actually not familiar with those perlisms. :-)

---

_Comment by @mernen on 2017-01-08 22:20_

Huh, indeed, it didn't occur to me at all to use `^.*` and `.*$` to replace the full line.

Since they are fully isomorphic, if there's no intention to change how `--replace` works, I suppose this issue should be closed — even though I still find `--output` more useful (i.e. simpler on the more common use case), I don't think it's a good idea to have both, and I can certainly live just with `--replace`.

---

_Comment by @BurntSushi on 2017-01-08 23:24_

Yeah, I can see how --output might be more convenient. Tricky issue. 

I think we can at least improve the docs of --replace to include an example that replaces the entire line.

---

_Label `doc` added by @BurntSushi on 2017-01-09 00:09_

---

_Comment by @mernen on 2017-01-09 00:14_

Just saw #34, about the grep feature `--only-matching`. I think it'd be the best of all worlds:

* As far as I can see, `-or` would be equivalent to Ack's `--output`
* The alternative use case (today's `--replace`) would still be available, and significantly less cryptic and annoying to type than Ack's ``--output '$`expr'"$'"``
* `-o` is a grep standard and works with other matches. I did consider adding a flag, but it seemed quite ridiculous to have one just for `--replace`. It didn't occur to me this same flag could have other (natural) uses.

---

_Comment by @BurntSushi on 2017-01-09 00:26_

@mernen Good point. It actually didn't occur to me that `--only-matching` would be combined with `--replace` like that, but it makes perfect sense!

---

_Closed by @BurntSushi on 2017-01-10 21:43_

---
