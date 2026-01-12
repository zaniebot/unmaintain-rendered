```yaml
number: 2935
title: surprising behavior of replacement using number capture groups when combined with multiple search patterns
type: issue
state: open
author: hugues-aff
labels:
  - question
assignees: []
created_at: 2024-11-16T23:30:42Z
updated_at: 2024-11-17T12:32:54Z
url: https://github.com/BurntSushi/ripgrep/issues/2935
synced_at: 2026-01-12T16:13:25Z
```

# surprising behavior of replacement using number capture groups when combined with multiple search patterns

---

_@hugues-aff_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 14.1.1 (rev 4649aa9700)

features:+pcre2
simd(compile):+NEON
simd(runtime):+NEON
```

### How did you install ripgrep?

homebrew

### What operating system are you using ripgrep on?

macOS 14.7

### Describe your bug.

ripgrep behavior when combining multiple patterns and replacement strings using numbered capture groups is very surprising.

### What are the steps to reproduce the behavior?

Run `rg -e '(foo)' -e '(bar)' -r '$1' -o` in a directory with files containing strings `foo` and `bar`

### What is the actual behavior?

Lines that contain `foo` get printed as `foo`, as expected
Lines that contain `bar` get printed as empty, which is rather surprising since neither of the patterns allow empty string

If I switch the replacement to `-r '$2'`, I see:

Lines that contain `foo` get printed as empty
Lines that contain `bar` get printed as `bar`

### What is the expected behavior?

I would expect the replacement index to reference whichever pattern actually matched, resulting in:

Lines that contain `foo` get printed as `foo`
Lines that contain `bar` get printed as `bar`

As a workaround, I can achieve this behavior right now using `-r '$1$2'` which works but is rather unintuitive

My preferred solution would be to allow a 1:1 mapping for `-r` to `-e`, but failing that, it seems like the indexing into capture groups when given multiple patterns should either be changed to be more intuitive, or at least documented if we're worried about breaking backwards compatibility

---

_Comment by @BurntSushi on 2024-11-17 00:01_

Please provide an MRE.

---

_Comment by @hugues-aff on 2024-11-17 07:37_

```
hugues.bruant@HBruant-M-C600F bugs % cat - >test <<EOF
heredoc> foo
heredoc> bar
heredoc> baz
heredoc> qux
heredoc> EOF
hugues.bruant@HBruant-M-C600F bugs % cat test
foo
bar
baz
qux
hugues.bruant@HBruant-M-C600F bugs % rg -e '(foo)' -e '(bar)' -r '$1' -o
test
1:foo
2:
hugues.bruant@HBruant-M-C600F bugs % rg -e '(foo)' -e '(bar)' -r '$2' -o
test
1:
2:bar
hugues.bruant@HBruant-M-C600F bugs % rg -e '(foo)' -e '(bar)' -r '$1$2' -o
test
1:foo
2:bar
hugues.bruant@HBruant-M-C600F bugs %
```

---

_Comment by @BurntSushi on 2024-11-17 12:32_

Yeah the issue here is that ripgrep doesn't really provide multi pattern support everywhere. When you provide more than one pattern, it "just" stitches them all together into one regex.

---

_Label `question` added by @BurntSushi on 2024-11-17 12:32_

---
