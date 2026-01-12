```yaml
number: 1537
title: Repeated group does not match
type: issue
state: closed
author: kerrickstaley
labels: []
assignees: []
created_at: 2020-04-02T00:18:50Z
updated_at: 2020-04-02T00:38:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1537
synced_at: 2026-01-12T16:13:23Z
```

# Repeated group does not match

---

_@kerrickstaley_

#### What version of ripgrep are you using?

```
ripgrep 12.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Via Homebrew.

#### What operating system are you using ripgrep on?

macOS 10.14.5.

#### Describe your question, feature request, or bug.

Ripgrep does not match the string `abc;de,fg` with the regex `;(.*,){1}`.

#### If this is a bug, what are the steps to reproduce the behavior?

This returns no lines:

```
echo 'abc;de,fg' > foo.txt
rg ';(.*,){1}' foo.txt
```

If I remove the `{1}` repetition on the group, I do get a match:
```
echo 'abc;de,fg' > foo.txt
rg ';(.*,)' foo.txt
```

I also get a match with PCRE:
```
echo 'abc;de,fg' > foo.txt
rg -P ';(.*,){1}' foo.txt
```

#### If this is a bug, what is the expected behavior?

I expect the first example to return a match.

I know this is almost certainly due to my misunderstanding Ripgrep's regex syntax, but I've been racking my brain and I can't figure out what I'm doing wrong.

---

_Closed by @BurntSushi on 2020-04-02 00:38_

---

_Comment by @BurntSushi on 2020-04-02 00:38_

Thanks for the great bug report! Should be fixed on master.

---
