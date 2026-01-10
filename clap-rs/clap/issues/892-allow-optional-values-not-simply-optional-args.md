---
number: 892
title: Allow optional values (not simply optional args with value notation)
type: issue
state: closed
author: patcon
labels: []
assignees: []
created_at: 2017-03-09T18:11:39Z
updated_at: 2018-08-02T03:30:03Z
url: https://github.com/clap-rs/clap/issues/892
synced_at: 2026-01-10T01:26:37Z
---

# Allow optional values (not simply optional args with value notation)

---

_Issue opened by @patcon on 2017-03-09 18:11_

From my reading of the docs, this isn't possible, but feel free to correct me!

The distinction being that an optional _argument_ using value notation is currently possible

```
-b --bump=[type] 'bump the version point automatically (options: patch, minor, major)'
```

Which would allow all of:

```
~$ mycommand --some-arg --bump=patch
~$ mycommand --some-arg --bump=major
~$ mycommand --some-arg # doesn't bump version
```

But I don't think there's a way to allow an optional _value_, like so:

```
~$ mycommand --some-arg --bump=patch
~$ mycommand --some-arg --bump # could be same result as above
~$ mycommand --some-arg # doesn't bump version
```

Any help or guidance is appreciated! If this is already possible, I'm happy to help update the docs to better convey this distinction :)

---

_Referenced in [chef-boneyard/delivery-cli#33](../../chef-boneyard/delivery-cli/issues/33.md) on 2017-03-09 18:12_

---

_Comment by @kbknapp on 2017-03-09 22:40_

Try setting [`Arg::min_values(0)`](https://docs.rs/clap/2.20.5/clap/struct.Arg.html#method.min_values). This should work, but I also can't remember if I special cased 0 values...

Let me know if that doesn't work 😉 

---

_Label `T: RFC / question` added by @kbknapp on 2017-03-09 22:41_

---

_Comment by @patcon on 2017-03-10 02:32_

Thanks so so much bunch, @kbknapp! :)

EDIT: haha "so so much bunch"...

---

_Closed by @patcon on 2017-03-10 02:32_

---

_Comment by @nodakai on 2017-10-15 03:01_

It would be great if a brief note about this use case could be added to `Arg::min_values()`.

Also, in this use case the difference between `ArgMatches::value_of()` returning `Some` and `ArgMatches::is_present()` returning `true` is important, and I think this part of the documentation of `value_of()` should be revised.

> If the option wasn't present at runtime it returns `None`.

Because it seem to suggest that it returns `None` **iff** the option wasn't present.

```rust
if matches.is_present("bump") {
  if let Some(v) = matches.value_of("bump") {
    println!("bump == {:?}", v);
  } else {
    println!("bump"); // matters only under `min_values(0)`
  }
} else {
  println!("do not bump");
}
```

---

_Comment by @kbknapp on 2017-10-16 13:41_

@nodakai I'd be good adding that blurb or updating the docs if you could open a new issue for it. Could you point out a link to the sections which should be made more clear? I have the problem of guilty knowledge and it's hard to for me to see which parts aren't exactly clear since I wrote the library and am intimately familiar with it 😜 

---

_Referenced in [denoland/deno#2129](../../denoland/deno/pulls/2129.md) on 2019-04-20 06:42_

---

_Referenced in [TeXitoi/structopt#123](../../TeXitoi/structopt/issues/123.md) on 2019-05-25 16:15_

---

_Referenced in [TeXitoi/structopt#188](../../TeXitoi/structopt/issues/188.md) on 2019-05-27 11:56_

---

_Referenced in [TeXitoi/structopt#364](../../TeXitoi/structopt/issues/364.md) on 2020-03-25 18:08_

---

_Referenced in [model-checking/kani#1548](../../model-checking/kani/pulls/1548.md) on 2022-08-18 19:29_

---
