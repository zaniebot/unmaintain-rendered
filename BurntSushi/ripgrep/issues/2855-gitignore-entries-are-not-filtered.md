```yaml
number: 2855
title: .gitignore entries are not filtered
type: issue
state: closed
author: Rasmus-Bertell
labels:
  - invalid
assignees: []
created_at: 2024-07-16T20:45:03Z
updated_at: 2024-07-18T17:19:17Z
url: https://github.com/BurntSushi/ripgrep/issues/2855
synced_at: 2026-01-12T16:13:25Z
```

# .gitignore entries are not filtered

---

_@Rasmus-Bertell_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

❯ rg --version
ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

```zsh
❯ sudo pacman -S ripgrep
```

[ripgrep package](https://archlinux.org/packages/extra/x86_64/ripgrep/)

### What operating system are you using ripgrep on?

Archlinux 6.9.9-arch1-1

### Describe your bug.

I have a gitignore file defined for my project but ripgrep still gets results from the vendor directory.

### What are the steps to reproduce the behavior?

```zsh
❯ composer create-project laravel/laravel example-app
❯ cd example-app
❯ echo "/vendor" > .gitignore # Laravel already has a default .gitignore so this step is not really needed
❯ rg todo

vendor/psy/psysh/src/ExecutionLoop/RunkitReloader.php
21: * @todo Remove RunkitReloader once we drop support for PHP 7.x :(

...

vendor/psy/psysh/src/Formatter/CodeFormatter.php
72:        // @todo something better here?
142:     * @todo consider switching \token_get_all() out for PHP-Parser-based formatting at some point

❯ echo "/vendor" > .ignore
❯ rg todo # No matches as expected

❯
```

### What is the actual behavior?

```zsh
❯ rg --debug todo &> log.txt
```

[log.txt](https://gist.github.com/Rasmus-Bertell/bccd3ae1e3cc92b2079e76ed7ab11afb)

### What is the expected behavior?

I would expect .gitignore and .ignore files to behave the same but with different precedence.

---

_Comment by @BurntSushi on 2024-07-16 20:59_

They don't behave the same. `.gitignore` only works in a git repo unless `--no-require-git` is passed. I don't see any `git init` in your repro, so this appears to be behaving as expected.

---

_Closed by @BurntSushi on 2024-07-16 20:59_

---

_Closed by @BurntSushi on 2024-07-16 20:59_

---

_Label `invalid` added by @BurntSushi on 2024-07-16 20:59_

---

_Comment by @Rasmus-Bertell on 2024-07-17 05:41_

Thanks for clarifying this. Didn't know .gitignore needed a git repo, though on the hindsight it makes sense. Perhaps make it a bit more clear in the documentation.

---

_Comment by @TinyLittleWheatley on 2024-07-18 17:11_

> They don't behave the same. `.gitignore` only works in a git repo unless `--no-require-git` is passed. I don't see any `git init` in your repro, so this appears to be behaving as expected.

This behavior confuses me. Why we need a .git directory to use .gitignore exclusion rules? Is there an specific scenario that requires this check?

I understand that a .gitignore file is expected to be near a .git directory but for example when i use Exercism cli to download an exercise, it doesn't initialize a git repository but there is a .gitignore just in case user decides to use git or see what will be submitted and what will be ignored by cli tool when sending a solution.

I assume the tradeoff has been considered at time of implementation. just commenting about my specific use case...

---

_Comment by @BurntSushi on 2024-07-18 17:19_

Yes, it has been considered. The specific technical reason is that `.gitignore` files should not cross git repository boundaries. For example, my `$HOME` is a git repository. And it has a `.gitignore`. But it would be wildly inappropriate to apply that `.gitignore` to repositories checked out in a sub-directory of `$HOME`. For example, `~/rust/ripgrep`.

`--no-require-git` exists for folks that don't want ripgrep to care about git repository boundaries.

---
