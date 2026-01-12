```yaml
number: 735
title: "End of options marker: `--`"
type: issue
state: closed
author: jdub
labels: []
assignees: []
created_at: 2016-11-08T04:22:13Z
updated_at: 2018-08-02T03:29:56Z
url: https://github.com/clap-rs/clap/issues/735
synced_at: 2026-01-12T16:14:09Z
```

# End of options marker: `--`

---

_@jdub_

I can't seem to find anything in the documentation or other issues about the end of options terminator, `--`.

I'm rejigging Servo's `bindgen` command line parsing with `clap`, and it relies on passing all arguments after `--` directly to Clang, for example:

```
bindgen example.h --use-core --raw-line "#![no_std]" -- -std=c++11
```

For now, it's easy enough to strip `--` and everything after it out before passing the arguments to `clap`, but it'd be rad to support this directly.

---

_Comment by @kbknapp on 2016-11-08 20:25_

`clap` supports this directly. The way it works is it'll store everything after `--` in a positional arg. The only caveat is this positional arg needs have `multiple(true)` set.

i.e.

``` rust
App::new("test")
    .arg(Arg::with_name("vals")
        .multiple(true))
```

Then running `$ test -- one two three four` will result in `vals` being `["one", "two", "three", "four"]`.


---

_Comment by @kbknapp on 2016-11-08 20:25_

Obviously the example above is contrived, but with more options and flags, you get the idea :wink:


---

_Label `T: RFC / question` added by @kbknapp on 2016-11-08 20:25_

---

_Comment by @jdub on 2016-11-08 21:12_

Aha! Nice. I reckon this is example-worthy. I'll send you a PR. Thanks for your help!


---

_Comment by @jdub on 2016-11-08 21:24_

*chuckle* https://github.com/jdub/rust-bindgen/commit/4f3ac0c9aa7b6aee400c4746362c2d3b44de0fe2


---

_Comment by @kbknapp on 2016-11-08 21:36_

Glad it shortened things up a bit :stuck_out_tongue_winking_eye: 


---

_Closed by @kbknapp on 2016-12-10 05:06_

---
