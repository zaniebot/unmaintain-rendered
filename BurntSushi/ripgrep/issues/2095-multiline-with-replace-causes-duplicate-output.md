```yaml
number: 2095
title: "`--multiline` with `--replace` causes duplicate output"
type: issue
state: closed
author: balupton
labels:
  - bug
assignees: []
created_at: 2021-12-02T09:56:36Z
updated_at: 2022-05-11T18:44:59Z
url: https://github.com/BurntSushi/ripgrep/issues/2095
synced_at: 2026-01-12T16:13:24Z
```

# `--multiline` with `--replace` causes duplicate output

---

_@balupton_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

cargo

#### What operating system are you using ripgrep on?

```
Darwin redacted 21.1.0 Darwin Kernel Version 21.1.0: Wed Oct 13 17:33:23 PDT 2021; root:xnu-8019.41.5~1/RELEASE_X86_64 x86_64
```

#### Describe your bug.

``` bash
cat <<EOF > test.bash
#!/usr/bin/env bash

zero=one

a=one

if true; then
	a=(
		a
		b
		c
	)
	true
fi

a=two

b=one
EOF

# all of these are the same


rg  --multiline --only-matching '^(?P<indent>\s*)a=(?P<value>(?ms:[(].*?[)])|.*?)$' --replace '${value}' ./test.bash

rg  --multiline --only-matching '^(?P<indent>\s*)a=(?P<value>[(](?ms:.*?)[)]|.*?)$' --replace '${value}'  test.bash

rg  --multiline --only-matching '^(?P<indent>\s*)a=(?P<value>[(](?ms:.*?)[)]|[^(].*?)$' --replace '${value}'  test.bash

rg  --multiline --only-matching '^(?P<indent>\s*)a=(?P<value>[(](?ms:.*?)[)]|[^(](?-ms:.*?))$' --replace '${value}' test.bash

rg  --multiline --only-matching '^(?-ms:(?P<indent>\s*)a=(?P<value>[(](?ms:.*?)[)]|[^(].*?))$' --replace '${value}' test.bash

rg  --multiline --only-matching '(?-ms:^(?P<indent>\s*)a=(?P<value>[(](?ms:.*?)[)]|[^(].*?)$)' --replace '${value}' test.bash
# ^ this fails, rg really doesn't like flag modifiers prior to ^ and $

rg --multiline --only-matching '(?ms:^(?P<indent>\s*)a=(?P<value>[(].*?[)]|[^\n]*)$)' --replace '${value}' ./test.bash

rg --multiline --only-matching '(?ms:^(?P<indent>\s*)a=(?P<value>[(].*?[)]|(?-ms:.*))$)' --replace '${value}' ./test.bash

rg --multiline --only-matching '(?m:^(?P<indent>\s*)a=(?P<value>[(](?s:.*?)[)]|.*)$)' --replace '${value}' ./test.bash
```

outputs:

```
4:one
7:(
8:		q
9:		r
10:		s
11:	)
14:two
8:(
9:		q
10:		r
11:		s
12:	)
15:two
15:two
```

Instead of:

```
4:one
7:(
8:		q
9:		r
10:		s
11:	)
14:two
```

#### What are the steps to reproduce the behavior?

see above

#### What is the actual behavior?

see above

#### What is the expected behavior?

see above

####  What do you think ripgrep should have done?

It would be good if the `--multiline` handling could be disabled, in favour of the rust regex localised flag applied `(?ms:`


---

_Comment by @balupton on 2021-12-02 10:14_

Without the `--replace`, it works fine, no duplicates, but returns the entire match:

``` bash
rg --multiline --only-matching '(?ms:^(?P<indent>\s*)a=(?P<value>[(].*?[)]|[^\n]*)$)'  ./test.bash

rg --multiline --only-matching '(?ms:^(?P<indent>\s*)a=(?P<value>[(].*?[)]|(?-ms:.*))$)'  ./test.bash

rg --multiline --only-matching '(?m:^(?P<indent>\s*)a=(?P<value>[(](?s:.*?)[)]|.*)$)' ./test.bash
```

output:

```
5:a=one
8:	a=(
9:		a
10:		b
11:		c
12:	)
16:a=two
```


---

_Renamed from "`--multiline` is causing duplicate output" to "`--multiline` with `--replace` causes duplicate output" by @balupton on 2021-12-02 10:26_

---

_Comment by @balupton on 2021-12-02 10:50_

For the meantime, `--max-count=1` is a suitable workaround.

---

_Label `bug` added by @BurntSushi on 2021-12-02 15:46_

---

_Comment by @BurntSushi on 2021-12-02 15:46_

Yup, looks like a bug to me. Thanks for the report!

---

_Comment by @ghost on 2022-02-23 20:27_

I ran into this problem when working on a unified diff printer. It seems the replacement bytes retain the remainder of the subject buffer instead of only until the match end. I tried to fix it and it worked, but I'm not sure if my fix is a proper one.
See https://github.com/BurntSushi/ripgrep/pull/2149/files#diff-cbced35c4a7b8ae41aa579dd0395a63152e23d6edf91d2e1035f189d1d3337da

---

_Closed by @BurntSushi on 2022-05-11 18:44_

---
