```yaml
number: 1179
title: "Feature request: Include compressed files when both -t and -z are enabled"
type: issue
state: closed
author: haqle314
labels:
  - question
assignees: []
created_at: 2019-01-28T21:05:58Z
updated_at: 2019-01-29T03:51:48Z
url: https://github.com/BurntSushi/ripgrep/issues/1179
synced_at: 2026-01-12T16:13:23Z
```

# Feature request: Include compressed files when both -t and -z are enabled

---

_@haqle314_

The flag `-z` should automatically update the patterns used by`-t`,
allowing optional compressed file type extension.

Let's say I have 2 markdown files, one is compressed, one is not.
```
echo "hello" > test1.md
echo "hello" | gzip > test2.md.gz
```
## Current behavior 
Compressed markdown file is not searched
```
rg -z -l -t markdown hello
# test1.md
```
## Suggested behavior
Also consider compressed markdown files when `-z` is specified.
```
rg -z -l -t markdown hello
# test1.md
# test2.md.gz
```

Currently, to get this behavior, I have to manually redefine the `markdown` type
```
rg --type-add 'markdown:*.{md,markdown,mdown,mkdn}{,.gz,.bz2,.xz,.lzma,.lz4}' -z -l -t markdown hello
# test1.md
# test2.md.gz
```

---

_Comment by @BurntSushi on 2019-01-28 21:22_

I'm going to have to decline. I don't think the `-t/--type` flag should be automatically expanded like this. This is what type definitions are for. You also don't need to re-create the glob definitions, e.g., `--type-add 'markdown:include:md,bzip2,gzip,lz4,lzma,xz'` will do what you want and will automatically get updated as new globs are added to the existing definitions. Alternatively, just use `rg -g '*.md*' -z foo`.

---

_Closed by @BurntSushi on 2019-01-28 21:22_

---

_Label `question` added by @BurntSushi on 2019-01-28 21:22_

---

_Comment by @haqle314 on 2019-01-29 03:51_

Your `--type-add` suggestion doesn't produce the desired behavior though.
Now every compressed file is considered, not just compressed markdown files.
```
echo "hello" | gzip > test3.py.gz
rg --type-add 'markdown:include:md,bzip2,gzip,lz4,lzma,xz' -z -l -tmarkdown hello
# test1.md
# test3.py.gz
# test2.md.gz
```

You're right in that I can roll it `-g '*.md*'` or something similar. But that's
not ideal either, sometime, people use `.` as a delimiter and you end up with
files such as `something.md.else.txt`

Also, I don't know if this matters or not, but the type markdown now has a duplicate
for each of the original glob pattern.
```
rg --type-add 'markdown:include:md,gzip' --type-list | grep ^markdown
# markdown: *.gz, *.markdown, *.markdown, *.md, *.md, *.mdown, *.mdown, *.mkdn, *.mkdn
```


---
