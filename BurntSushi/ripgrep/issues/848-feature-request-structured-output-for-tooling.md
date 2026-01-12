```yaml
number: 848
title: "Feature request: structured output for tooling"
type: issue
state: closed
author: powercode
labels:
  - duplicate
assignees: []
created_at: 2018-03-08T18:35:28Z
updated_at: 2018-03-09T11:59:28Z
url: https://github.com/BurntSushi/ripgrep/issues/848
synced_at: 2026-01-12T16:13:22Z
```

# Feature request: structured output for tooling

---

_@powercode_

#### What version of ripgrep are you using?
```
ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX
```

#### What operating system are you using ripgrep on?

Microsoft Windows 10.0.16299

#### Describe your question, feature request, or bug.

It would be great if there was an option to get ripgrep to produce output similar to protobuf,
that tools could use to write highly efficient and accurate parsing out the output.

So that the output would be something like

```
# fieldI1 = path
# field 2 = linenumber
# field 3 = line
#field 4  = matchOffsetPair
# field 5 = contextline (integer index and variable path)

<sizeofrecord>
1 # next is path
<sizeOfField> # if type is variable length
"some path"
2
93(4 bytes of data)  # or a compressed integer
3
"Some matching line of lines"
4 # first match
14-4 (8 bytes of data), or two compressed integers
4 # second match
22-4 
```

---

_Label `duplicate` added by @BurntSushi on 2018-03-09 11:59_

---

_Comment by @BurntSushi on 2018-03-09 11:59_

Duplicate of #244.

---

_Closed by @BurntSushi on 2018-03-09 11:59_

---
