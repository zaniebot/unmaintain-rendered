```yaml
number: 1627
title: "Offset in `--json` output does not take into account BOM bytes"
type: issue
state: closed
author: acheronfail
labels:
  - wontfix
assignees: []
created_at: 2020-06-25T03:06:43Z
updated_at: 2020-06-25T22:58:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1627
synced_at: 2026-01-12T16:13:23Z
```

# Offset in `--json` output does not take into account BOM bytes

---

_@acheronfail_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
cargo install ripgrep
```

#### What operating system are you using ripgrep on?

```bash
# Arch Linux
Linux 5.4.43-1-lts x86_64 GNU/Linux
```

#### Describe your bug.

When using the `--json` flag, the `absolute_offset` provided by `rg` doesn't take into account Byte Order Marks of the file's encoding.

#### What are the steps to reproduce the behavior?

Running this:

```bash
printf "\xFF\xFE\x16\x04" > utf16le.txt
rg . ./utf16le.txt --json
```

Produces the following output:
```txt
{"type":"begin","data":{"path":{"text":"./utf16le.txt"}}}
{"type":"match","data":{"path":{"text":"./utf16le.txt"},"lines":{"text":"Ж"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"Ж"},"start":0,"end":2}]}}
{"type":"end","data":{"path":{"text":"./utf16le.txt"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":24618,"human":"0.000025s"},"searches":1,"searches_with_match":1,"bytes_searched":2,"bytes_printed":231,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.001884s","nanos":1884139,"secs":0},"stats":{"bytes_printed":231,"bytes_searched":2,"elapsed":{"human":"0.000025s","nanos":24618,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

As you can see the match object here:

```txt
{"type":"match","data":{"path":{"text":"./utf16le.txt"},"lines":{"text":"Ж"},"line_number":1,"absolute_offset":0,"submatches":[{"match":{"text":"Ж"},"start":0,"end":2}]}}
```

Provides an `absolute_offset` of `0`. If we read the range of bytes reported, we get this:

```bash
# cut's index starts at 1
cut -zb 1-3 ./utf16le.txt | xxd
```

We get the UTF16LE BOM, _not_ the matched portion that `rg` found:

```bash
# Note that the trailing `00` is outputted by `cut`
00000000: fffe 00                                  ...
```

#### What is the expected behavior?

I'm not sure if this is the expected behaviour or not, but I expected that the range provided by `rg` would map to the precise location of the matched bytes inside the file.

If this is not the case, is there a way we can somehow surface this information? :thinking: 

I'm making an [interactive replacement tool for ripgrep](https://github.com/acheronfail/repgrep), and right now I have to detect the presence of BOMs myself and increase the `absolute_offset` accordingly.


---

_Comment by @acheronfail on 2020-06-25 03:12_

Another thing that's similar to this that when no encoding is passed to `rg` (it defaults to `auto`) there's no way to know from the `--json` output what encoding `rg` used when matching the file. (My tool can just read the args passed to `rg` when the encoding is passed.)

Right now my tool uses `chardet` and attempts to guess the encoding when it reads the files, but it would be nice if the `begin` JSON message was able to output the encoding that `rg` tried when it read the file. :slightly_smiling_face: 

(I know that `rg` itself is guessing the encoding, so it's not perfect. But this would mean that my tool uses the same encoding that `rg` itself used.)

---

_Comment by @BurntSushi on 2020-06-25 13:05_

I'm afraid that `absolute_offset` is perhaps not as useful as I had hoped it would be when I added it. The BOM is really the least of your worries here, since you can at least look for it and deal with in a pretty simple way. The real problem is that the absolute offset is only computed based on the UTF-8 data that the searcher actually sees. If the original file is UTF-16, then the offsets will be completely off. Observe:

```
$ cat utf8
a
b
c

$ iconv -f utf-8 -t utf16 utf8 > utf16le

$ xxd utf16le
00000000: fffe 6100 0a00 6200 0a00 6300 0a00       ..a...b...c...

$ rg c utf16le --json
{"type":"begin","data":{"path":{"text":"utf16le"}}}
{"type":"match","data":{"path":{"text":"utf16le"},"lines":{"text":"c\n"},"line_number":3,"absolute_offset":4,"submatches":[{"match":{"text":"c"},"start":0,"end":1}]}}
{"type":"end","data":{"path":{"text":"utf16le"},"binary_offset":null,"stats":{"elapsed":{"secs":0,"nanos":64999,"human":"0.000065s"},"searches":1,"searches_with_match":1,"bytes_searched":6,"bytes_printed":219,"matched_lines":1,"matches":1}}}
{"data":{"elapsed_total":{"human":"0.003218s","nanos":3218044,"secs":0},"stats":{"bytes_printed":219,"bytes_searched":6,"elapsed":{"human":"0.000065s","nanos":64999,"secs":0},"matched_lines":1,"matches":1,"searches":1,"searches_with_match":1}},"type":"summary"}
```

The `c` match is reported in a line starting at absolute offset `4`, but its actual position in the original source file is thrown off by both the BOM and the fact that ASCII codepoints are encoded with two bytes each in UTF-16. There is no simple way to map the offsets reported by ripgrep back to offsets in the original file without re-computing them yourself.

It's unfortunate, but I don't see a simple way to solve this. This is probably a victim of de-coupling. The layer in ripgrep that handles transcoding has zero knowledge of the layer that handles searching and dealing with offsets.

> Another thing that's similar to this that when no encoding is passed to `rg` (it defaults to `auto`) there's no way to know from the `--json` output what encoding `rg` used when matching the file. (My tool can just read the args passed to `rg` when the encoding is passed.)

This might be plausible to do, and then you could at least do offset translation at that point. It would be work, but it would be possible. However, I think this is a separate issue and probably requires some careful planning. Would you mind creating a new issue? It would help very much if you could write as much detail as possible about your use case.

> (I know that `rg` itself is guessing the encoding, so it's not perfect. But this would mean that my tool uses the same encoding that `rg` itself used.)

It is not actually guessing the encoding. The only "sniffing" it does is BOM detection. BOMs are purportedly very strong indicators of the encoding in the file. If no BOM is present and no explicit encoding is provided by the end user, then ripgrep will always assume an ASCII-compatible (and UTF-8 by convention) encoding. This works decently well for latin-1 for example.

---

_Label `question` added by @BurntSushi on 2020-06-25 13:09_

---

_Comment by @acheronfail on 2020-06-25 21:58_

> The real problem is that the absolute offset is only computed based on the UTF-8 data that the searcher actually sees. If the original file is UTF-16, then the offsets will be completely off

Ah, I'd written some simple tests that only dealt with a single special character that itself took up two bytes in UTF8 as well, so this part slipped by me.

> There is no simple way to map the offsets reported by ripgrep back to offsets in the original file without re-computing them yourself.

> It's unfortunate, but I don't see a simple way to solve this. This is probably a victim of de-coupling. The layer in ripgrep that handles transcoding has zero knowledge of the layer that handles searching and dealing with offsets.

I guess the simplest way for me to get this working in a somewhat reliable manner, would be to trust the `line_number` reported by ripgrep and when reading the file start there and look for the match myself?

> However, I think this is a separate issue and probably requires some careful planning. Would you mind creating a new issue? It would help very much if you could write as much detail as possible about your use case.

Here it is: #1629 

---

_Comment by @BurntSushi on 2020-06-25 22:58_

> I guess the simplest way for me to get this working in a somewhat reliable manner, would be to trust the `line_number` reported by ripgrep and when reading the file start there and look for the match myself?

Probably, assuming you're using the same regex engine (which might either be Rust's regex engine or PCRE2).

I'm going to close this given the presence of #1629. I don't think there is much that can be done here unfortunately. Or at least, fixing it would require some rather large implementation work that I'm not particularly keen on doing or supporting.

---

_Closed by @BurntSushi on 2020-06-25 22:58_

---

_Label `question` removed by @BurntSushi on 2020-06-25 22:58_

---

_Label `wontfix` added by @BurntSushi on 2020-06-25 22:58_

---
