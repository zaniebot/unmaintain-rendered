```yaml
number: 2020
title: Multiline with replace (and passthru) duplicates lines from the input in the output
type: issue
state: closed
author: janickr
labels:
  - bug
assignees: []
created_at: 2021-10-14T18:57:48Z
updated_at: 2022-08-29T17:56:33Z
url: https://github.com/BurntSushi/ripgrep/issues/2020
synced_at: 2026-01-12T16:13:24Z
```

# Multiline with replace (and passthru) duplicates lines from the input in the output

---

_@janickr_

#### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

homebrew:
brew install ripgrep

#### What operating system are you using ripgrep on?
MacOS Big Sur

ProductName:	macOS
ProductVersion:	11.6
BuildVersion:	20G165

#### Describe your bug.

Give a high level description of the bug.

#### What are the steps to reproduce the behavior?

When performing a multiline search and replace with passthru, rg duplicates lines in the output

#### What is the actual behavior?

```
 ~$ seq 0 9 | rg -U  -e '1\n2' --passthru -r 'x' --debug                                                                      
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
0
x
3
4
5
6
7
8
9
3
4
5
6
7
8
9
```

#### What is the expected behavior?

I expect only 1\n2 to be replaced by x and the rest of the input to be left unchanged

```
0
x
3
4
5
6
7
8
9
```

#### A clue, maybe
When invoking the same command without passthru and replace has the expected output:
```
~$ seq 0 9 | rg -U  -e '1\n2$'                                                                                                          
1
2
```
But invoking without passthru but with replace also outputs too many lines
```
~$ seq 0 9 | rg -U  -e '1\n2$' -r 'x'                                                                                                         
x
3
4
5
6
7
8
9
```
I would expect it to output only the replacement:
```
x
```


---

_Comment by @balupton on 2021-12-02 10:27_

Seems the `--passthru` isn't necessary to reproduce this, as I'm getting it with just `--multiline` and `--replace`:

https://github.com/BurntSushi/ripgrep/issues/2095

---

_Label `bug` added by @BurntSushi on 2021-12-02 15:47_

---

_Comment by @gthb on 2022-08-29 16:17_

A second, maybe more motivating example: I wanted to improve on `jq`'s pretty-printing by postprocessing it to put single-property objects on one line:
```sh
rg --passthru -U '\{\n\s*("[^"]*":[^,\n]+)\n\s*\}' --replace '{ $1 }'
```
but that hits this problem, so with this input:
```json
{
  "foo": [
    {
      "foo": [
        {
          "bar": "x"
        },
        {
          "bar": "y"
        }
      ]
    },
    {
      "bar": "z"
    }
  ]
}
```
I get:
```
{
  "foo": [
    {
      "foo": [
        { "bar": "x" },
        { "bar": "y" }
      ]
    },
    { "bar": "z" }
  ]
}
      ]
    },
    { "bar": "z" }
  ]
}
  ]
}
```

---

_Comment by @BurntSushi on 2022-08-29 16:24_

Hmmm, I wonder if this is a duplicate of #2095? If so, it might be fixed on master?

---

_Comment by @gthb on 2022-08-29 17:50_

> Hmmm, I wonder if this is a duplicate of #2095? If so, it might be fixed on master?

Well, yep! Tried out:
```sh
git clone git@github.com:BurntSushi/ripgrep.git
cargo install --path ripgrep
~/.cargo/bin/rg --passthru -U '\{\n\s*("[^"]*":[^,\n]+)\n\s*\}' --replace '{ $1 }' rgbug.json | cat
```
Sure enough, that outputs:
```
{
  "foo": [
    {
      "foo": [
        { "bar": "x" },
        { "bar": "y" }
      ]
    },
    { "bar": "z" }
  ]
}
```

---

_Comment by @BurntSushi on 2022-08-29 17:56_

Sweet!

---

_Closed by @BurntSushi on 2022-08-29 17:56_

---
