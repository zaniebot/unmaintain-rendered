```yaml
number: 1723
title: Would like path_of
type: issue
state: closed
author: njaard
labels:
  - C-enhancement
assignees: []
created_at: 2020-03-04T23:47:26Z
updated_at: 2021-12-08T21:15:12Z
url: https://github.com/clap-rs/clap/issues/1723
synced_at: 2026-01-12T16:14:11Z
```

# Would like path_of

---

_@njaard_

### Use case

It's common to get a filename from clap with `value_of_os` and its companion `values_of_os`. You often want to turn it into a `Path` in order to pass it to `std::fs::File::open()`.

The only way I've been able to find to turn an `OsStr` into a `Path` is with `Path::new()`, which is a bit wordy. Therefor, it'd be nice be able to get Path objects directly.

### Proposed solution

It'd be nice if there were a function like `Matches::path_of(name: &str) -> Option<&Path>` so that you can get a `Path` in a single function call. 

Alternate names for `path_of` might be the consistent but less concise `value_of_path` but that somehow reads grammatically different than `value_of_os`, not to mention being longer. Therefor, `path_of` sounds better.

Additionally, you'd want `paths_of` the way there is `values_of_os`

Hypothetically, one could also make it reject (at `get_matches`-time) impossible `Path`s (on Windows, not all Paths are valid).

### Alternative solutions

The standard library could get some sort of convenient shorthand for turning `OsStr` into `Path`. I think adding the function into `clap` serves the purpose better because it self-documents better while keeping std clean and explicit.


---

_Label `T: new feature` added by @njaard on 2020-03-04 23:47_

---

_Renamed from "Would like value_of_path" to "Would like path_of" by @njaard on 2020-03-04 23:47_

---

_Comment by @CreepySkeleton on 2020-03-05 07:36_

I'm not very excited about adding yet another output format, but we can make it if paths are really so common, perhaps under `value_of_as_path` and `values_of_as_paths` names. cc @pksunkara @Dylan-DPC what do you think?

> Hypothetically, one could also make it reject (at get_matches-time) impossible Paths (on Windows, not all Paths are valid).

That's definitely up to user. Even `Path::from` doesn't validate it.

---

_Comment by @njaard on 2020-03-05 19:14_

> I'm not very excited about adding yet another output format, but we can make it if paths are really so common, perhaps under `value_of_as_path` and `values_of_as_paths` names.

CLI tools of course have filenames as a common use case.

> That's definitely up to user. Even Path::from doesn't validate it.

Well, I was just speculating, but naturally if the user doesn't want it validated, they could just use `value_of_os`.


---

_Comment by @CreepySkeleton on 2020-03-05 19:16_

Except, what does "valid path" means?

---

_Comment by @njaard on 2020-03-05 19:24_

> Except, what does "valid path" means?

On Windows, a Path may not contain `<`, `>`, `|`, `"`, `?`, `*`, and outside of drive letters, `:` [according to Wikipedia](https://en.wikipedia.org/wiki/Filename#Reserved_characters_and_words). (I don't use Windows)

On Linux, I think there are no restrictions other than of course the null termination.

---

_Comment by @CreepySkeleton on 2020-03-05 19:38_

This kind of checking would be largely useless. The os facilities would check it anyway (`std::fs::*` in Rust), so I don't see the point to check it on our side.

Come to think about it, `value_of_as_path` is a trivial `value_of().map(Path::from)`; I'm very unconvinced about your proposal,  no offense. Let's hear out the other maintainers @TeXitoi @pksunkara @Dylan-DPC .

---

_Comment by @TeXitoi on 2020-03-05 20:12_

It would be only me, I'd say no. I'm not really convinced by the added feature.

---

_Comment by @pksunkara on 2020-03-06 10:35_

It is definitely trivial. I would consider this one of those things where if more people request it, I wouldn't mind adding it.

---

_Comment by @CreepySkeleton on 2020-03-06 11:37_

@pksunkara Which one? The checking or the conversion?

---

_Comment by @pksunkara on 2020-03-06 11:43_

Conversion

---

_Comment by @njaard on 2020-03-13 22:13_

Another compelling argument in favor is that I believe it's more teachable: If a user has an &str, they can pass it directly into `File::open`, however, `OsStr` cannot be, it needs to be converted into a `Path` and not making it easy and automatic acts as another barrier to writing correct code.

---

_Comment by @CreepySkeleton on 2020-03-13 22:57_

>  If a user has an &str, they can pass it directly into File::open, however, OsStr cannot be

Good try, [but it can 😮](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=6ea05e62eea4e6297d1ca97e6cdf9efe)

---

_Comment by @njaard on 2020-03-18 20:17_

> > If a user has an &str, they can pass it directly into File::open, however, OsStr cannot be
> 
> Good try, [but it can open_mouth](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=6ea05e62eea4e6297d1ca97e6cdf9efe)

Doh. Yes because File accepts `AsRef<Path>`, but when one writes basic application code, I think `&Path` is more common. At least it is for me.


---

_Comment by @CreepySkeleton on 2020-03-18 23:36_

> Doh. Yes because File accepts AsRef<Path>, but when one writes basic application code, I think &Path is more common. At least it is for me.

Well, the argument was about flattening the learning curve, wasn't it? I've swept through `std::fs::*` and found that the API uses `AsRef<Path>` instead of `&Path` literally everywhere. In fact, you would want to use `Path` explicitly only when you want to query some property of the `Path` itself, like `extension` or `is_file`.

I see your point about `Path`s being commonplace, but it could use some acknowledgement. If and when at least two more people come here to ask for it, I'll have become open to accepting such a PR.

---

_Comment by @CreepySkeleton on 2020-06-30 10:13_

Closing as inactive. I don't think this feature is of any use, and, evidently, almost nobody wants it.

---

_Closed by @CreepySkeleton on 2020-06-30 10:13_

---

_Comment by @Dylan-DPC-zz on 2020-06-30 10:19_

Please don't close any issues just because nobody commented they needed it. There could always be a future feature request. 

---

_Reopened by @Dylan-DPC-zz on 2020-06-30 10:19_

---

_Referenced in [clap-rs/clap#751](../../clap-rs/clap/issues/751.md) on 2020-07-07 14:33_

---

_Referenced in [epage/clapng#142](../../epage/clapng/issues/142.md) on 2021-12-06 19:13_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:14_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:14_

---

_Comment by @epage on 2021-12-08 21:15_

I believe https://github.com/clap-rs/clap/issues/2683 should supersede this, closing it.

---

_Closed by @epage on 2021-12-08 21:15_

---
