---
number: 2639
title: "[Derive] `#[doc = macro_name!()]` doesn't play well with doc comments"
type: issue
state: open
author: rami3l
labels:
  - C-bug
  - E-hard
  - A-derive
assignees: []
created_at: 2021-07-29T19:22:59Z
updated_at: 2025-11-01T12:06:05Z
url: https://github.com/clap-rs/clap/issues/2639
synced_at: 2026-01-07T13:12:19-06:00
---

# [Derive] `#[doc = macro_name!()]` doesn't play well with doc comments

---

_Issue opened by @rami3l on 2021-07-29 19:22_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.54.0

### Clap Version

master

### Minimal reproducible code

```rust
#[test]
fn doc_comments_raw_macro() {
    macro_rules! doc_lorem_ipsum {
        () => {
            "Lorem ipsum"
        };
        (inner) => {
            "Fooify a bar and a baz"
        };
    }

    #[doc = doc_lorem_ipsum!()]
    #[derive(Clap, PartialEq, Debug)]
    struct LoremIpsum {
        #[doc = doc_lorem_ipsum!(inner)]
        #[clap(short, long)]
        foo: bool,
    }

    let help = get_long_help::<LoremIpsum>();
    assert!(help.contains("Lorem ipsum"));
    assert!(help.contains("Fooify a bar and a baz"));
}
```


### Steps to reproduce the bug with the above code

Run the test above.

### Actual Behaviour

Assertions fail with no comments help message detected:

```yaml
%%% LONG_HELP %%%:=====
clap_derive 

USAGE:
    clap_derive [FLAGS]

FLAGS:
    -f, --foo
            

    -h, --help
            Prints help information

    -V, --version
            Prints version information

=====
```

### Expected Behaviour

Assertions pass with all help messages specified by the macro call.

```yaml
%%% LONG_HELP %%%:=====
clap_derive 
<- We should see some help messages here...

USAGE:
    clap_derive [FLAGS]

FLAGS:
    -f, --foo      <- And here...
            

    -h, --help
            Prints help information

    -V, --version
            Prints version information

=====
```

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @rami3l on 2021-07-29 19:22_

---

_Comment by @rami3l on 2021-07-29 20:47_

I investigated the current codebase for quite a while, and the main problem we have here seems to be the following:

When will the macro be expanded? Ideally `clap_derive` should never bother to deal with the macro call itself, but should instead send it safely to call site. Is this feasible?

---

_Comment by @epage on 2021-07-29 21:12_

I went down a different rabbit hole, in hopes that bumping MSRV and maybe proc macro deps might be of assistance.  I'm guessing not but at least we have https://github.com/clap-rs/clap/pull/2640

I'll look into that part.  Part of the problem is we do post-processing on the doc comment to better format it.  iirc we have a verbatim flag which could work with it.  I'll dig a little into these parts.

---

_Comment by @epage on 2021-07-29 21:17_

From https://github.com/rust-lang/rust/pull/83366

> Expansion of macro expressions in "inert" attributes occurs after decorators have executed, analogously to macro expressions appearing in the function body or other parts of decorator input.
>
> There is currently no way for decorators to accept macros in key-value position if macro expansion must be performed before the decorator executes (if the macro can simply be copied into the output for later expansion, that can work).

Still fairly weak on my proc macros but I think this is confirming that we have to pass along the macro call for later expansion, which means it is unlikely to work with `clap_derive` without some re-work.  As-is, we *always* do post-processing, even with verbatim is enabled.

---

_Comment by @epage on 2021-07-29 21:18_

I guess one option is we can either generate or embed some const-fns that do the post-processing for us.

---

_Comment by @rami3l on 2021-07-29 21:49_

@epage Yeah, I saw that post-processing part.

Before, with only doc literals, it's reasonable to do this in the derive macro. However, when the doc macros come into play, it seems impossible (at least to both of us) to keep it this way: the call site has to be changed.

But don't worry, once the macro call has been transferred to the call side, we should be able to use something like the [`tt_call`](https://github.com/dtolnay/tt-call) hack to perform comptime preprocessing. (I used it in my own app [`pacaptr`](https://github.com/rami3l/pacaptr/blob/f4c8ca74ca2010f5f31cafb451f15bd05df63fd7/src/pm/mod.rs#L184) and it works, although it's rather hard to get right.)

It's true that `const fn` might be another possibility. It's however also quite complicated to operate on `&str` these days with only `const fn`... :(

Of course, we have to be very careful about `clap`'s dependencies... But in the worst case, we can at least move the preprocessing part to runtime.

---

_Comment by @rami3l on 2021-07-29 23:57_

@epage Wait a second, does https://github.com/rust-lang/rust/pull/87264 include the eager expansion that we want? If so, we can wait for it!

(And in that case I know exactly which function should be modified!)

---

_Label `C: derive macros` added by @pksunkara on 2021-07-30 07:34_

---

_Label `D: hard` added by @pksunkara on 2021-07-30 07:34_

---

_Referenced in [ajeetdsouza/zoxide#256](../../ajeetdsouza/zoxide/issues/256.md) on 2021-08-26 20:22_

---

_Referenced in [epage/clapng#194](../../epage/clapng/issues/194.md) on 2021-12-06 21:18_

---

_Comment by @brunoczim on 2022-01-19 13:51_

This is a big problem to me. I need to generate commands from a macro, and the macro documents commands automatically, but it needs to interpolate a few strings inside documentation.

I am using `#[doc = concat!("foo ", $bar, " baz")]` to achive this, but it breaks clap and so I cannot both document and generate help/about at once.

---

_Comment by @rami3l on 2022-01-19 16:29_

@brunoczim Since https://github.com/rust-lang/rust/pull/87264 has already landed with `expand_expr`, I think I can take a look.

... But don't expect that to be available in `clap` soon... We have to wait for https://github.com/rust-lang/rust/issues/90765 and there's still an MSRV to be bumped :(

---

_Comment by @brunoczim on 2022-01-19 20:06_

I'd appreciate it, thanks!

---

_Referenced in [clap-rs/clap#6158](../../clap-rs/clap/issues/6158.md) on 2025-10-30 18:21_

---

_Comment by @epage on 2025-10-30 18:23_

Was looking  at this again because of #6158 and rust-lang/rust#90765 has still not stabilized.

---

_Comment by @rami3l on 2025-11-01 12:06_

@epage Indeed, we'd need those for this to work as well, thanks for looking into it again! So my above analysis was not entirely correct...

---
