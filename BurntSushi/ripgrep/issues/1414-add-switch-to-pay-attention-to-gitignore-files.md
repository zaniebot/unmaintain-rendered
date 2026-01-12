```yaml
number: 1414
title: Add switch to pay attention to .gitignore files when not in a git repo
type: issue
state: closed
author: JohnC32
labels:
  - enhancement
  - help wanted
  - question
assignees: []
created_at: 2019-10-29T20:58:06Z
updated_at: 2022-05-15T03:48:31Z
url: https://github.com/BurntSushi/ripgrep/issues/1414
synced_at: 2026-01-12T16:13:23Z
```

# Add switch to pay attention to .gitignore files when not in a git repo

---

_@JohnC32_

Could you add a switch e.g. --force-gitignore which would force usage of .gitignore files even when there's no .git directory?

Give a large repo, it's often convenient to archive versions of it and not save the .git directory. Currently ripgrep 11.0.2 does not pay attention to .gitignore files if there's no .git directory. This makes it impossible to use rg on such repo's. Another use case is when using Perforce. Perforce pays attention to .gitignore files and does not have a .git directory. A work around is to create an empty .git directory, but in our environment this causes problems because git based tools fail when there's an empty .git directory.

Thanks for ripgrep - it's really powerful!

---

_Label `question` added by @BurntSushi on 2019-10-29 21:03_

---

_Comment by @BurntSushi on 2019-10-29 21:06_

What about adding an empty _file_ called `.git` instead?

You could also symlink `.gitignore` to `.ignore`.

I'm really not too inclined to add even more flags specifically for changing when `.gitignore` is interpreted.

> Another use case is when using Perforce. Perforce pays attention to .gitignore files and does not have a .git directory.

What is the standard way to detect a Perforce repo? If Perforce specifically recognizes gitignore files, then we could add the Perforce check in addition to the `.git` check. Although, this will likely have unacceptable performance overhead.

---

_Comment by @JohnC32 on 2019-10-29 21:17_

An empty .git directory or empty .git file does cause git based tools to fail. Both external tools and internal tools. For example, "touch .git; git status" yields "fatal: Invalid gitfile format: .git".

In Perforce, you set in their config P4IGNORE to .gitignore. You can also place P4IGNORE in the environment. Thus, detecting a Perforce repo is going to require something else. You could look for the existence of a .perforce file, but even this isn't required.

We have a large repo with many hundreds of .gitignore files and developed on multiple platforms. Symlink'ing won't work because of Windows. Adding rules to generate .ignore from .gitignore files is challenging because of the variety of different build systems we use.

I assume you do the .git check once, could you update it check for another file, e.g. .ripgrep-force-gitignore?

---

_Comment by @JohnC32 on 2019-10-29 21:25_

Out of curiosity, why does ripgrep require a .git directory to pay attention to the .gitignore files? In a quick scan of the code and doc, I couldn't find a reason.

---

_Comment by @BurntSushi on 2019-10-29 21:29_

See https://github.com/BurntSushi/ripgrep/issues/934 and https://github.com/BurntSushi/ripgrep/commit/e65ca21a6cebceb9ba79fcd164da24478cc01fb0

I guess it seems like there isn't much of a choice here other than to add a flag. It's just pretty unfortunate and I'd really rather that ripgrep not grow an unbounded number of flags for every weird case. In this case, it really seems like `.gitignore` is being misused. But if this can be implemented without much complexity, then I'd be willing to accept a PR.

---

_Label `enhancement` added by @BurntSushi on 2019-10-29 21:29_

---

_Label `help wanted` added by @BurntSushi on 2019-10-29 21:29_

---

_Comment by @JohnC32 on 2019-10-29 21:40_

Thanks - I will look into adding a switch and create a PR assuming it can be done cleanly.

---

_Comment by @akarle on 2019-10-30 18:39_

Hi @BurntSushi,

Firstly, thank you for ripgrep! It's an awesome tool and I'm a long time fan/user.

I'm working with @JohnC32 on an implementation of this.

It was pretty trivial to add a switch to force reading gitignores outside a git repo (basically just an option for the Ignore class).

However, I stumbled across a cleaner (IMHO) way of doing it.

I noticed you have a "custom ignore" functionality baked in, which, if I understand correctly, is being used to read patterns from `.rgignore` files. As far as I can tell, there is no switch that lets the user create new custom ignores.

Would you be open to adding a switch which allows arbitrary files to be read as ignores (basically just passing them to `add_custom_ignore_filename`)? I believe this is slightly cleaner while still resolving our use case (we can run with `--ignore-file-name .gitignore` which will add `.gitignore` as a custom ignore name, causing it to work outside of a git repository).

Thank you for your time and help! I have a prototype of the `--ignore-file-name` switch, but wanted to reach out to see which solution you thought was cleaner before putting up the PR.

Thanks,
Alex

---

_Comment by @BurntSushi on 2019-10-30 19:45_

@akarle That is an interesting approach, but it's not quite the same. In particular, `.gitignore` has _lower_ precedence that `.ignore` (and `.rgignore`). This is very much intentional, since it permits users to override `.gitignore` patterns with new patterns in `.ignore`. However, `add_custom_ignore_filename` specifically processes those files with the highest possible precedence, such that patterns in `.ignore` would no longer override it. This would be very confusing for users who would otherwise expect `.ignore` to override `.gitignore`.

---

_Comment by @akarle on 2019-10-30 20:11_

@BurntSushi thank you for the fast response and for the clarification!

Hmm I was aware that it was a higher priority, but hadn't considered the impact of not allowing `.ignore` to override `.gitignore`.

I had originally assumed that `.ignore` was for a different SCM system (maybe CVS), but I now see that appears to be without merit. Under this assumption, I hadn't considered users of `.gitignore`s to also be using `.ignore`.

I now see that `.ignore` is a specific file used for search tools exclusively (rather than SCM systems). This makes much more sense, and I can see why we wouldn't want to override them with `--ignore-file-name .gitignore`.

I'll look back into the original "pay attention to .gitignore files outside git repos" switch :)

Thanks again,
Alex

---

_Closed by @BurntSushi on 2020-02-17 22:16_

---

_Comment by @Freed-Wu on 2022-05-15 03:42_

I try to compile rg and found it will test failure in no_require_git. Is it related to `--no-require-git`? Did I do something wrong?

```
failures:

---- feature::f1414_no_require_git stdout ----
thread 'main' panicked at '
printed outputs differ!

expected:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bar
foo

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

got:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
bar

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
', tests\feature.rs:738:5
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace


failures:
    feature::f1414_no_require_git

test result: FAILED. 263 passed; 1 failed; 0 ignored; 0 measured; 0 filtered out; finished in 27.38s
```

---

_Comment by @Freed-Wu on 2022-05-15 03:48_

And Although I have installed `asciidoctor`, the `rg.1` still cannot be generated in OUT_DIR. What is the probable cause?

<https://github.com/msys2/MINGW-packages/issues/11657>

---
