```yaml
number: 986
title: Thoughts on a different format for zsh completions?
type: issue
state: closed
author: nerdrew
labels: []
assignees: []
created_at: 2017-06-10T05:29:00Z
updated_at: 2018-08-02T03:30:08Z
url: https://github.com/clap-rs/clap/issues/986
synced_at: 2026-01-12T16:14:10Z
```

# Thoughts on a different format for zsh completions?

---

_@nerdrew_

I have a custom zsh completion file for rustup that I like. I have attached my custom one and the clap generated completions. Objections to using a different zsh completion format?

Other questions: for rustup it is nice to have completions that are context specific, i.e. when using `rustup run <toolchain> <blah>` it is nice to have toolchain completed to the actually installed toolchains `rustup toolchain list`, does clap support specifying that kind of metadata on arguments? Essentially, a dynamically generated list of valid arguments.

[clap-generated-rustup.txt](https://github.com/kbknapp/clap-rs/files/1065614/clap-generated-rustup.txt)
[my-custom-rustup.txt](https://github.com/kbknapp/clap-rs/files/1065613/my-custom-rustup.txt)



---

_Comment by @kbknapp on 2017-06-16 15:33_

I'm totally open to new and better formats for ZSH completions. In fact, I'm a ZSH completion expert by **no means** 😄 My problem is finding time to not only implement them, but also the time to learn them first.

If you'd like to dive into the ZSH generation, I'd be more than willing to mentor you through clap's internal workings. All the ZSH related bits can be found in [`src/completions/zsh.rs`](https://github.com/kbknapp/clap-rs/blob/master/src/completions/zsh.rs)

> does clap support specifying that kind of metadata on arguments? Essentially, a dynamically generated list of valid arguments.

Yes, using [`Arg::possible_values`](https://docs.rs/clap/2.24.2/clap/struct.Arg.html#method.possible_values). Since the valid arguments aren't actually built until runtime, it's absolutely possible to build a list of "valid toolchains" dynamically which are passed off to `possible_values`. The problem you'll run into is then passing those values to this ZSH completion script that does *not* get dynamically updated.

This is the topic of #568 

---

_Label `T: RFC / question` added by @kbknapp on 2017-06-16 15:33_

---

_Closed by @kbknapp on 2018-07-22 02:28_

---
