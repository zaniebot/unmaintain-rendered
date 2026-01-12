```yaml
number: 52
title: rg interprets too much as text
type: issue
state: closed
author: lambda
labels:
  - bug
  - libripgrep
assignees: []
created_at: 2016-09-24T05:09:10Z
updated_at: 2017-02-18T21:20:27Z
url: https://github.com/BurntSushi/ripgrep/issues/52
synced_at: 2026-01-12T16:13:21Z
```

# rg interprets too much as text

---

_@lambda_

I ran `rg foo` on a directory with a variety of different files, and it printed out a bunch of binary junk, including a bell, but luckily not anything that screwed up my terminal, on some binary files, including a bzipped tar file.

Here's the beginning of the file, via xxd; notice the lack of nulls:

```
0000000: 425a 6839 3141 5926 5359 8271 61b0 02b7  BZh91AY&SY.qa...
0000010: baff ffff ffff ffff ffff ffff ffff ffff  ................
0000020: ffff ffff ffff ffff ffff ffff ffff ffff  ................
0000030: ffff ffe8 c57f 36d3 addf 5dc7 b9f5 eebe  ......6...].....
0000040: ef8e 58de f6fa faef bbde f5f7 d96d edf5  ..X..........m..
0000050: 7bcf 41be df55 c46e cefa b9bd dbca 0ae6  {.A..U.n........
0000060: dbed 6f7d e5db d77d becb 6bcf 61a5 e7bd  ..o}...}..k.a...
0000070: df7d bbee fad7 2f7a eedf 79d7 aa37 9efb  .}..../z..y..7..
0000080: d79d 377b b7b0 643e 839d 776e cbbb efbc  ..7{..d>..wn....
0000090: fabe b7db 43dd f6bd e3d5 ade5 b6e4 fb61  ....C..........a
00000a0: bef7 bef8 a6ba df6d df7d eedb ef7c fbef  .......m.}...|..
00000b0: 9b6b dbd7 5bbb 6e7b 9b69 57b6 ebaf 7d8d  .k..[.n{.iW...}.
00000c0: e3a3 bedf 5cf3 7af9 33db deac 0747 5eef  ....\.z.3....G^.
00000d0: 7de7 c8f4 5f5a fb2f 7be3 df13 4fb7 dcfb  }..._Z./{...O...
00000e0: ed35 f6e2 df73 5f7d deef 3ddd cbaf bede  .5...s_}..=.....
00000f0: fbe6 f7dd bbaa f2fa f7de 5bef 6edf 7dee  ..........[.n.}.
0000100: f55b deef 5f47 6ef6 dbcf b7dd a17d 877b  .[.._Gn......}.{
0000110: 7d75 baee f573 3ded ef9f 77d6 f37d 1f7d  }u...s=...w..}.}
0000120: ee7d eb3b 59e8 ecdb eee3 b61e ecfb dbd0  .}.;Y...........
0000130: 5eda 3ebb b3ae 5f7b 74de c6be 5e59 ed5a  ^.>..._{t...^Y.Z
0000140: f97d 7bd9 b19f 5afa ebbe fbb4 bc67 addb  .}{...Z......g..
0000150: 537b b45e fafa fbed 3adb 5df5 df7d bb6a  S{.^....:.]..}.j
0000160: 77bd d6f7 3b77 df6e efba defb 9cfb 6525  w...;w.n......e%
0000170: 7db3 6f77 4697 ade3 3ddb edef 6abe e77b  }.owF...=...j..{
0000180: 7df7 df3a db3c f4d7 63ea ebee fbdd 5d76  }..:.<..c.....]v
0000190: becf 37b3 b7d9 edad 7b8a a3be ddab b77d  ..7.....{......}
00001a0: 9dc0 6f7b deaf 5d8c ef77 af74 dbbe fbef  ..o{..]..w.t....
00001b0: 7d95 d75d 5ade e674 ddb9 34f7 77cf 7416  }..]Z..t..4.w.t.
00001c0: 9f2e 3d37 d2df 2d3b be97 1d72 faf9 d9eb  ..=7..-;...r....
00001d0: 9edd 4ef7 7d7d 1bef b8cf 6bde fbc7 5df5  ..N.}}....k...].
00001e0: eefb eefb d7af b7dd ea6e b9d7 6d35 7acf  .........n..m5z.
```

GNU grep just reports:

```
Binary file es-raid-tools.tbz matches
```


---

_Comment by @BurntSushi on 2016-09-24 05:24_

Nice. `rg` only looks for a `NUL` byte. I thought that was what GNU grep did too, but clearly I was wrong.


---

_Label `bug` added by @BurntSushi on 2016-09-24 05:24_

---

_Comment by @BurntSushi on 2016-09-26 01:41_

I think this bug is going to be tough to fix without actually having a file I can use to reproduce the bug. I've tried running `ripgrep` on a few archives myself and it seems to correctly detect it as binary.

Looking at the source of GNU grep confirms that it is only detecting binary by looking for NUL bytes. (Well, it has one other way: by looking for "holes" in a file, which imply a NUL byte.) The other possibility is that GNU grep is searching a larger buffer, but I don't _think_ that's the case.


---

_Comment by @lambda on 2016-09-26 02:40_

This particular tarball consists of a variety of proprietary content, so not sure I can release it. However, I checked and the first NUL is at offset 2331.

It [looks like GNU grep scans every buffer that it reads for nulls](http://git.savannah.gnu.org/cgit/grep.git/tree/src/grep.c#n1515):

``` c
  for (bool firsttime = true; ; firsttime = false)
    {
      if (nlines_first_null < 0 && eol && binary_files != TEXT_BINARY_FILES
          && (buf_has_nulls (bufbeg, buflim - bufbeg)
              || (firsttime && file_must_have_nulls (buflim - bufbeg, fd, st))))
        {
          if (binary_files == WITHOUT_MATCH_BINARY_FILES)
            return 0;
          if (!count_matches)
            done_on_match = out_quiet = true;
          nlines_first_null = nlines;
          nul_zapper = eol;
          skip_nuls = skip_empty_lines;
        }
```


---

_Comment by @BurntSushi on 2016-09-26 02:44_

Ah... Yes! I misread the code. `rg` only looks at the very first one. Thanks for pointing that out!


---

_Comment by @lambda on 2016-09-26 02:45_

It also looks like [GNU grep avoids printing output that has encoding errors](http://git.savannah.gnu.org/cgit/grep.git/tree/src/grep.c#n1099):

``` c
static bool
print_line_head (char *beg, size_t len, char const *lim, char sep)
{
  if (binary_files != TEXT_BINARY_FILES)
    {
      char ch = beg[len];
      bool encoding_errors = buf_has_encoding_errors (beg, len);
      beg[len] = ch;
      if (encoding_errors)
        {
          encoding_error_output = true;
          return false;
        }
    }
```


---

_Comment by @BurntSushi on 2016-09-26 02:59_

Hmm, I think that only happens if `-a` is set, which I think means it's pretty solidly relying on NUL byte detection to determine whether a file is binary or not. I believe `TEXT_BINARY_FILES` is the default for GNU grep. (See the `--binary-files` flag.) Namely, GNU grep will say "Binary file matched" instead of trying to print any matches.

But yeah, that is a neat trick since `rg` also has a `-a` option.


---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Label `libripgrep` added by @BurntSushi on 2017-01-11 03:04_

---

_Closed by @BurntSushi on 2017-02-18 21:20_

---
