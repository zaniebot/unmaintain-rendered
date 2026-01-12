```yaml
number: 1373
title: Configuration file is ignored
type: issue
state: closed
author: dnicolson
labels:
  - invalid
assignees: []
created_at: 2019-09-11T15:39:47Z
updated_at: 2021-01-03T20:55:12Z
url: https://github.com/BurntSushi/ripgrep/issues/1373
synced_at: 2026-01-12T16:13:23Z
```

# Configuration file is ignored

---

_@dnicolson_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Homebrew.

#### What operating system are you using ripgrep on?

macOS 10.14.6 (18G95).

#### Describe your question, feature request, or bug.

The `.ripgreprc` configuration file appears to be ignored.

#### If this is a bug, what are the steps to reproduce the behavior?

```
mkdir test
cd test
echo test > .hidden
rg test .
rg test . --hidden
./.hidden
1:test
cat $HOME/.ripgreprc
--binary
--hidden
```

#### If this is a bug, what is the actual behavior?

The configuration file is ignored.

```
DEBUG|grep_regex::literal|grep-regex/src/literal.rs:59: literal prefixes detected: Literals { lits: [Complete(test)], limit_size: 250, limit_class: 10 }
DEBUG|globset|globset/src/lib.rs:430: glob converted to regex: Glob { glob: "**/*~", re: "(?-u)^(?:/?|.*/)[^/]*\\~$", opts: GlobOptions { case_insensitive: false, literal_separator: true, backslash_escape: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('~')]) }
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 1 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 1 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|ignore::walk|ignore/src/walk.rs:1639: ignoring ./.hidden: Ignore(IgnoreMatch(Hidden))
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|globset/src/lib.rs:435: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

#### If this is a bug, what is the expected behavior?

The file should've matched without needing the `--hidden` switch.


---

_Comment by @BurntSushi on 2019-09-11 15:46_

You haven't made it clear whether you've set the `RIPGREP_CONFIG_PATH` environment variable or not. Please [consult the guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file) for more info. Could you please confirm that it is set?

---

_Comment by @dnicolson on 2019-09-11 15:54_

Setting the `RIPGREP_CONFIG_PATH` to the full path resolved the issue. Is there any reason why the current directory is checked but not the home directory? The documented example uses the home directory.

---

_Comment by @BurntSushi on 2019-09-11 16:08_

Not sure what you're talking about. The rule is simple: if `RIPGREP_CONFIG_PATH` is set, then ripgrep reads the file at that location for configuration. If it isn't set, then ripgrep doesn't look anywhere for configuration.

---

_Closed by @BurntSushi on 2019-09-11 16:08_

---

_Label `invalid` added by @BurntSushi on 2019-09-11 16:08_

---

_Comment by @Justsoos on 2019-10-31 22:03_

I have the same problem, it is a homebrew's missing duty to notice user like:
```
adding such to your .zshrc or .bashrc:
export RIPGREP_CONFIG_PATH=$HOME/.ripgreprc
```

---

_Comment by @dnicolson on 2019-10-31 22:07_

I'm confused as to why the home directory is not checked by default, my `.gitconfig`, `.ruby-version`, `.jsbeautifyrc` and `.eslintrc.json`, `.tool-versions`, etc. files are...

---

_Comment by @okdana on 2019-10-31 22:16_

I think it's because, at the time, there was no off-the-shelf library for looking up config files in the right place (`$HOME` vs `$XDG_CONFIG_HOME` vs whatever Windows does vs ...), and @BurntSushi didn't want to implement it himself

See https://github.com/BurntSushi/ripgrep/issues/196#issuecomment-362850321

---

_Comment by @BurntSushi on 2019-10-31 22:23_

Because if I did that, the angry denizens of the Internet will unite and whine about how there is yet another config file polluting their home directory. So if I instead support the XDG spec, then some subset of mac users will complain about how macOS doesn't use XDG. And even if I fix that, there is still Windows to consider, which is an entirely different beast.

Compare that to just setting an env var, and it is comparatively quite simple. Moreover, this is pretty carefully documented in the guide. This is the only issue I've seen that has complained about it.

Feel free to search the issue tracker. The feature request for config files documents the full progression. Until someone develops a Rust crate that implements config file discovert correctly and otherwise meets my standards, this is how ripgrep will behave.

---

_Comment by @cornfeedhobo on 2021-01-03 18:23_

> Compare that to just setting an env var,

I think a lot of people consider `ack` as an early ripgrep, and it's long followed a pattern of searching current and parent directories for `.ackrc`. I'm not sure if it stops at `$HOME`, and I don't know if it merges down with rules closest to the working directory, like `editorconfig` does, but this seems _even easier_ than asking everyone to set an environment variable.

This has the added benefit of allowing a repository embed a config.

@BurntSushi I'm probably hijacking this issue, but I really think the general topic of better support around config files should be explored. I'm reaching out here, before making another issue, to see if you open to at least discussing this

---

_Comment by @BurntSushi on 2021-01-03 18:43_

No. I think setting an environment variable is flexible enough. I'm not interested in pursuing an option where files scattered about a directory tree can modify the runtime arguments and behavior of ripgrep. (And also, your proposed scheme seems to implicitly codify dropping a config file into `$HOME`, which would likely anger people. See my previous comments.)

---

_Comment by @cornfeedhobo on 2021-01-03 19:08_

Seems like everyone has accepted it for other extremely popular tools. I see editorconfig as much more widely adopted than ripgrep (in my experience), and I've never heard anyone complain about this. Since people don't have to opt-in to dropping a config file anywhere, I can't imagine whom you are worried about offending (read the previous comment already). 

Edit: I don't want that to come off as insensitive. I'm sure you deal with a lot of crap and are always angering someone. I just don't see how something that isn't forced could inspire anger, while not supporting it does.

Responding directly to the environment variable premise, consider the use case of embedding in a repository. The user is required to set this value to `$PWD` every time they navigate to this directory. They can adopt tools like `direnv`, but again, cumbersome.

---

_Comment by @BurntSushi on 2021-01-03 19:23_

I didn't say an environment variable was perfect. I said it was _flexible enough._ 

The people I'm worried about angering are the people that get upset by the proliferation of dotconfig files in their home directory. They demand that their tools follow the XDG spec. If you haven't heard of these people complaining before, then I'm not sure what to tell you. I see it a lot and I refuse to be the target of their zeal.

Just because other projects do this doesn't mean I'm obliged to do it or that it's a good idea. Not all projects are the same. The only reason I added config files in the first place was for Windows users because they solve a real need. Otherwise, I would be happy with aliases and/or wrapper scripts on Unix. Addressing the use case of having arbitrarily nested configuration in a directory tree is not only something that I think is not worth it, but will actively result in more confusing UX.

Finally, ripgrep is tool that folks likely want to be able to run on any files without fear that those files will modify its behavior. The feature you've proposed means that ripgrep's behavior will change based on whatever config someone else might have set. This could even include causing ripgrep to run arbitrary executables via the `--pre` flag. That's a security problem in addition to all the weird UX behavior that might result from ripgrep respecting config files from random stuff you download.

---

_Reopened by @BurntSushi on 2021-01-03 19:23_

---

_Closed by @BurntSushi on 2021-01-03 19:25_

---

_Comment by @cornfeedhobo on 2021-01-03 19:43_

> If you haven't heard of these people complaining before, then I'm not sure what to tell you. I see it a lot and I refuse to be the target of their zeal.

Sorry you've had to deal with them. I'm sure it's not great, but can't they just use `grep`? No one has to pander to them.

> Otherwise, I would be happy with aliases and/or wrapper scripts on Unix.

Isn't this what we were all doing with `grep` before rg? I had a bunch of wrappers for vcs. `rg` eliminates all but _**literally one**_  use case. For such a next generation tool, this seems like an arbitrary choice.

---

_Comment by @BurntSushi on 2021-01-03 19:57_

I just explained why it's not an arbitrary choice and I find your argument of "just" telling people to use grep to be uncompelling. In particular, your use case is far more niche. You can "just" source a file that sets the env var to the config you want when you want to change it for a project specific directory. Kind of like what people do with virtualenv when working on Python projects. It's a pretty standard idiom.

And finally, what ripgrep provides cannot be implemented with simple grep aliases/wrapper scripts.

> No one has to pander to them.

I don't have to deal with them because I've avoided being their target. That's the point. You continuing to argue this point is coming across as combative to me. _One_ of the reasons I don't want to do your idea is for my own emotional comfort by avoiding being a target. There is nothing to argue there. It's simply a consequence of mobs and the semantics of your proposal. Moreover, I've stated several other reasons why I don't want to use your proposal, one of which is security related. If you can't address the security and UX concerns, then there's no point in talking about anything else other than to quibble.

> I had a bunch of wrappers for vcs. rg eliminates all but literally one use case. For such a next generation tool, this seems like an arbitrary choice.

That sounds like a great improvement then. And what people consider "next generation" is typically pretty arbitrary.

---

_Comment by @cornfeedhobo on 2021-01-03 20:55_

> You continuing to argue this point is coming across as combative to me.

@BurntSushi Well, I can only say sorry you take it that way. Thanks for all your hard work. I hope you reconsider your position some day.

---
