---
number: 665
title: "arg_from_usage(): required values not being enforced when followed by another option"
type: issue
state: closed
author: ajdlinux
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2016-09-16T10:15:34Z
updated_at: 2018-08-02T03:29:53Z
url: https://github.com/clap-rs/clap/issues/665
synced_at: 2026-01-07T13:12:19-06:00
---

# arg_from_usage(): required values not being enforced when followed by another option

---

_Issue opened by @ajdlinux on 2016-09-16 10:15_

I've got a SubCommand defined with a few options that have required values:

```
SubCommand::with_name("format")
    .about("Prepare patch series for email")
    .arg_from_usage("--in-reply-to [Message-Id] 'Make the first mail a reply to the specified Message-Id'")
    .arg_from_usage("--no-from 'Don't include in-body \"From:\" headers when formatting patches authored by others'")
    .arg_from_usage("-v, --reroll-count=[N] 'Mark the patch series as PATCH vN'")
    .arg_from_usage("--stdout 'Write patches to stdout rather than files'")
    .arg_from_usage("--subject-prefix [Subject-Prefix] 'Use [Subject-Prefix] instead of the standard [PATCH] prefix'"),
```

If I use either the `--subject-prefix` or `-v` options alone, without specifying a value, it complains:

```
ajd@ajd:~/code/skiboot$ /home/ajd/code/git-series/target/debug/git-series format --subject-prefix 
error: The argument '--subject-prefix <Subject-Prefix>' requires a value but none was supplied
...
ajd@ajd:~/code/skiboot$ /home/ajd/code/git-series/target/debug/git-series format -v
error: The argument '--reroll-count <N>' requires a value but none was supplied
...
```

But if I use `--subject-prefix` followed by `-v 2`, it gives me an empty value for `--subject-prefix`:

```
ajd@ajd:~/code/skiboot$ /home/ajd/code/git-series/target/debug/git-series format --subject-prefix -v 2
v2-0000-cover-letter.patch
v2-0001-hw-phb3-add-OPAL-call-to-get-current-PHB-CAPI-state.patch
...
```

And vice versa:

```
ajd@ajd:~/code/skiboot$ /home/ajd/code/git-series/target/debug/git-series format -v --subject-prefix banana
0000-cover-letter.patch
0001-hw-phb3-add-OPAL-call-to-get-current-PHB-CAPI-state.patch
...
```

This is shown in both 2.10.0 and 2.12.1.

(Apologies for the poor nature of this report, I'm in the middle of yak shaving so I may well have missed something here.)


---

_Renamed from "Required values not being enforced when followed by another option" to "arg_from_usage(): required values not being enforced when followed by another option" by @ajdlinux on 2016-09-16 10:15_

---

_Comment by @kbknapp on 2016-09-16 21:21_

Thanks for filing this report, this looks like a bug and I'll start investigating it! There may be a way to force what you're looking for by using the `Arg::from_usage` instead of `App::arg_from_usage`, because that allows you setting additional properties like, `Arg::number_of_values` or other such settings.

Also, a side note not related to this issue, if you have multiple `from_usage` args to declare which _don't_ require any of the additional `Arg` methods you can use `App::args_from_usage` to save some typing:

``` rust
.args_from_usage("
        --in-reply-to [Message-Id] 'Make the first mail a reply to the specified Message-Id'
        --no-from 'Don't include in-body \"From:\" headers when formatting patches authored by others'
    -v, --reroll-count=[N] 'Mark the patch series as PATCH vN'
        --stdout 'Write patches to stdout rather than files'
        --subject-prefix [Subject-Prefix] 'Use [Subject-Prefix] instead of the standard [PATCH] prefix'")
```


---

_Label `T: bug` added by @kbknapp on 2016-09-16 21:25_

---

_Label `P2: need to have` added by @kbknapp on 2016-09-16 21:25_

---

_Label `C: args` added by @kbknapp on 2016-09-16 21:25_

---

_Label `C: parsing` added by @kbknapp on 2016-09-16 21:25_

---

_Label `W: 2.x` added by @kbknapp on 2016-09-16 21:25_

---

_Added to milestone `2.12.2` by @kbknapp on 2016-09-16 21:25_

---

_Comment by @kbknapp on 2016-09-16 21:35_

@ajdlinux I'm not able to reproduce this. Can you post a link to the file where this happening, it may be related to another setting or something that you're using.

[This is what I used to test](https://gist.github.com/kbknapp/f60653971d9b3a34886e9c77051d8907)


---

_Comment by @ajdlinux on 2016-09-17 14:31_

Branch: https://github.com/ajdlinux/git-series/tree/format-subject-prefix
File: https://github.com/ajdlinux/git-series/blob/format-subject-prefix/src/main.rs

I'm not very familiar with this codebase (forked it just to add a single new option) so there might be something in there I'm missing. Will look into it shortly when I've got a bit more time.


---

_Added to milestone `2.17.0` by @kbknapp on 2016-10-24 13:35_

---

_Removed from milestone `2.12.2` by @kbknapp on 2016-10-24 13:35_

---

_Added to milestone `2.20.0` by @kbknapp on 2016-12-27 04:35_

---

_Removed from milestone `2.17.0` by @kbknapp on 2016-12-27 04:35_

---

_Comment by @kbknapp on 2016-12-30 21:06_

@ajdlinux sorry it's take so long! I've been doing a lot of traveling and then the holidays came around. I'm finally able to reproduce this, and it's next up on the fix list! I'll let you know what I find.

---

_Label `P1: urgent` added by @kbknapp on 2016-12-30 21:09_

---

_Label `P2: need to have` removed by @kbknapp on 2016-12-30 21:09_

---

_Comment by @kbknapp on 2016-12-30 21:27_

This is fixed in #796 

One thing to note is that you should add `Arg::empty_values(false)` to `--subject-prefix` because by default clap allows empty values unless you specificy a minimum number of values.

---

_Closed by @homu on 2016-12-30 23:44_

---

_Comment by @ajdlinux on 2016-12-31 00:47_

Thanks @kbknapp, apologies for not following this up, I'd forgotten about this!

---

_Referenced in [clap-rs/clap#1105](../../clap-rs/clap/issues/1105.md) on 2017-11-13 04:00_

---
