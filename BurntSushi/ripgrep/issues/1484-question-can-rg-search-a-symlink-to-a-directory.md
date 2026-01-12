```yaml
number: 1484
title: "[Question] Can rg search a symlink to a directory that is explicitly ignored by a different name?"
type: issue
state: closed
author: danemacmillan
labels:
  - invalid
assignees: []
created_at: 2020-02-14T17:03:04Z
updated_at: 2020-02-14T20:39:57Z
url: https://github.com/BurntSushi/ripgrep/issues/1484
synced_at: 2026-01-12T16:13:23Z
```

# [Question] Can rg search a symlink to a directory that is explicitly ignored by a different name?

---

_@danemacmillan_

Hi,

#### What version of ripgrep are you using?

```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

brew

#### What operating system are you using ripgrep on?

MacOS Catalina 10.15.3 (19D76)

#### Describe your question, feature request, or bug.

I have the following directory structure:
```
project/releases/{dated-release-1,dated-release-2,dated-release-n}
project/current -> (symlink to ./releases/dated-release-n)
```
The `current` symlink is a sibling of the `releases` directory. `current` is a symlink to one of the dated release directories contained within `releases`. Each dated release directory is just a more up to date snapshot of a codebase.

Given that each dated release directory is almost the same, it would be nice for `rg` to ignore them. In fact, ignore all of them. The only dated release directory I'm interested in is the one symlinked to `current`.

The problem I'm having is that specifying an ignore rule for the `releases` directory also prevents the `current` symlink and its contents from being searched. I think I understand why this would happen.

Is it possible to search under the `current` symlink while ignoring all the `releases`'s dated release directories? I realize this is a paradox of sorts, so I'm asking in the off chance I overlooked this in the documentation.

To be clear, `rg` is being invoked with the ability to follow symlinks, and does work when the ignore rule is removed.

Thanks!

---

_Comment by @BurntSushi on 2020-02-14 18:13_

In the future, could you please include more details in your bug report? In particular, please **show** your problem rather than just describing it. e.g., Instead of "The problem I'm having is that specifying an ignore rule for the releases directory also prevents the current symlink and its contents from being searched.", please show exactly what you tried.

For example:

```
$ mkdir /tmp/ripgrep-1484
$ cd /tmp/ripgrep-1484
$ mkdir -p project/releases/{dated-release-1,dated-release-2,dated-release-n}
$ ln -s project/releases/dated-release-n current
$ touch project/releases/dated-release-{1,2,n}/test
$ tree
.
├── current -> project/releases/dated-release-n
└── project
    └── releases
        ├── dated-release-1
        │   └── test
        ├── dated-release-2
        │   └── test
        └── dated-release-n
            └── test

6 directories, 3 files

$ rg --files
project/releases/dated-release-2/test
project/releases/dated-release-1/test
project/releases/dated-release-n/test

$ rg --files -L
current/test
project/releases/dated-release-n/test
project/releases/dated-release-1/test
project/releases/dated-release-2/test

$ echo '/project/releases' > .ignore

$ rg --files -L
current/test
```

Which seems to do exactly what you want.


---

_Label `question` added by @BurntSushi on 2020-02-14 18:13_

---

_Comment by @danemacmillan on 2020-02-14 20:23_

Will do--I'll keep that in mind for the future.

After noticing your example worked, I dug deeper. While my custom ignore file was being overlooked, the `.ignore` file in the example directory at `/project/releases/.ignore` was working. My custom ignore file contained the same content, though, so it should have gotten ignored at that point, instead of getting detected by the inline `.ignore` file.

I have a `RIPGREP_CONFIG_PATH` environment variable set; its contents were:

```
--hidden
--search-zip
--smart-case
--ignore-vcs
--ignore-file=$XDG_CONFIG_HOME/ripgrep/ignore
```

The contents of the `$XDG_CONFIG_HOME/ripgrep/ignore` file are:

```
releases
```

I would then run the following command on the example structure:

```
rg --files -L
```

The output would still list:

```
current/test
project/releases/dated-release-n/test
project/releases/dated-release-1/test
project/releases/dated-release-2/test
```

I then tried differing paths for the `--ignore-file` option, one at a time:

```
--ignore-file=~/.config/ripgrep/ignore
--ignore-file="$HOME/.config/ripgrep/ignore"
```

I then tried passing the option to `rg` directly:

```
rg --files -L --ignore-file=$XDG_CONFIG_HOME/ripgrep/ignore
```

That worked. The `releases` directory was ignored, while the `current` symlink content was being shown. Great.

That got me thinking about environment variables, so I tried an absolute path in the config&mdash;_and it worked_:

```
--ignore-file=/Users/danemacmillan/.config/ripgrep/ignore
```

My takeaway from this is that environment variables are not understood when used in the `RIPGREP_CONFIG_PATH` config file. Is that right?

I don't know if this is something you'd want to support, but I'll propose it for your consideration.

Thanks a bunch for helping me through this.

---

_Comment by @BurntSushi on 2020-02-14 20:39_

> My takeaway from this is that environment variables are not understood when used in the RIPGREP_CONFIG_PATH config file. Is that right?

Correct. In particular, from the [section on config files in the GUIDE](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#configuration-file):

> In particular, there is no escaping. Each line is given to ripgrep as a single command line argument verbatim.

The config format is deliberately simplistic. While it does prevent some conveniences like environment variables, it makes the file very easy to read with no additional layer of escaping. e.g., If environment variables were supported then there would need to be some sort of escaping mechanism to permit literals of the form `$var`.

---

_Closed by @BurntSushi on 2020-02-14 20:39_

---

_Label `question` removed by @BurntSushi on 2020-02-14 20:39_

---

_Label `invalid` added by @BurntSushi on 2020-02-14 20:39_

---
