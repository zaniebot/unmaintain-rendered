```yaml
number: 1207
title: finding zero bytes in utf-16 encoded files
type: issue
state: closed
author: LesnyRumcajs
labels:
  - enhancement
assignees: []
created_at: 2019-02-28T19:42:09Z
updated_at: 2019-04-06T14:35:09Z
url: https://github.com/BurntSushi/ripgrep/issues/1207
synced_at: 2026-01-12T16:13:23Z
```

# finding zero bytes in utf-16 encoded files

---

_@LesnyRumcajs_

#### What version of ripgrep are you using?
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
cargo install ripgrep

#### What operating system are you using ripgrep on?

Fedora 29

#### Describe your question, feature request, or bug.
I'm struggling to find files that contain 00 bytes. I created an UTF-16 LE file with text `test` (hexdump)
```
00000000: fffe 7400 6500 7300 7400 0a00            ..t.e.s.t...
```
Given that there are 00 bytes inside I'm issuing a command `rg -cuuu '(?-u:\x00)'` but get no results at all. It works for searching for `t`, like 
```
rg -cuuu '(?-u:\x73)'
test.txt:1
```
From my understanding the `-uuu` flag along with some UTF escaping should do the trick. It works fine for non-zero bytes. It also works for binary files (tried with a file comprising of a single 00 byte). Am I missing something?

---

_Comment by @lespea on 2019-02-28 19:51_

I think ripgrep is seeing the BOM and properly decoding the test as utf-16 (hence no null bytes).  Try using the `--no-encoding` flag?

```
    -E, --encoding <ENCODING>
            Specify the text encoding that ripgrep will use on all files searched. The
            default value is 'auto', which will cause ripgrep to do a best effort automatic
            detection of encoding on a per-file basis. Automatic detection in this case
            only applies to files that begin with a UTF-8 or UTF-16 byte-order mark (BOM).
            No other automatic detection is performend.

            Other supported values can be found in the list of labels here:
            https://encoding.spec.whatwg.org/#concept-encoding-get

            For more details on encoding and how ripgrep deals with it, see GUIDE.md.

            This flag can be disabled with --no-encoding.
```

---

_Comment by @LesnyRumcajs on 2019-02-28 19:57_

@lespea I'm pretty sure that's the case (removing BOM makes the command find the bytes, it's treating it as a plain binary). I don't know how to get over it though. Using `--no-encoding` as in `rg --no-encoding -cuuu '(?-u:\x00)'` still doesn't detect null bytes in UTF-16 LE file. 

---

_Comment by @BurntSushi on 2019-02-28 21:18_

Interesting issue. The `-u` flags are superfluous in this case, sans the last one, which you can just replace with `-a`. The `(?-u:\x00)` can always just be written as `\x00` since codepoint `0` corresponds to byte `0`. The `--no-encoding` flag is also a red herring here, since all that does is disable the _use_ of an `--encoding` flag, e.g., by resetting it back to `auto`.

The reason why this is happening is because ripgrep is indeed detecting the BOM and transcoding your UTF-16 to UTF-8, which gets rid of all NUL bytes in this case. ripgrep does not expose any options to override this behavior. Even if you set `-E utf8`, the BOM still takes precedence because [ripgrep enables this option](https://docs.rs/encoding_rs_io/0.1.4/encoding_rs_io/struct.DecodeReaderBytesBuilder.html#method.bom_override). This option is enabled because the BOM is a super strong indicator of the encoding of the text file, so even if you specify UTF-8, it's still good behavior to switch to UTF-16 automatically when necessary.

I suspect the way to fix this is to allow one to specify `-E none` such that ripgrep never does any transcoding at all.

The only work-around available to you at the moment, as far as I know, is stripping the BOM:

```
[andrew@Cheetah rg1207]$ xxd foo.utf16le
00000000: 7400 6500 7300 7400 0a00                 t.e.s.t...
[andrew@Cheetah rg1207]$ rg '\x00' foo.utf16le -a
1:test
2:
```

---

_Label `enhancement` added by @BurntSushi on 2019-02-28 21:18_

---

_Comment by @LesnyRumcajs on 2019-02-28 21:42_

@BurntSushi Thanks for explaining. I understand my case is a little bit out of the normal usage of ripgrep - I'm processing text files that may be corrupted (UTF8, UTF16 ... or a mix of them, don't ask). Anyway I think `-E non` or  4th `u` to further reduce ripgrep's smartness would be a nice feature for such corner cases.

---

_Comment by @BurntSushi on 2019-02-28 22:07_

Yes, I agree. ripgrep should be able to handle this use case. There should be a way to override transcoding so that you can treat even completely valid UTF-16 as arbitrary bytes.

---

_Comment by @LesnyRumcajs on 2019-03-01 09:05_

@BurntSushi  Do you consider it a good first task? Or would it require some major rewrite? I'd like to contribute.

---

_Comment by @BurntSushi on 2019-03-01 12:39_

@LesnyRumcajs Ah yes, great idea! This is probably a decent first task, although there is a fair bit of plumbing. The high level idea is to add support for completely disabling transcoding support. This change will require changes to three different crates. (Such is the cost for splitting ripgrep's functionality out so that others can use it.)

* [`encoding_rs_io`'s `DecodeReaderBytesBuilder`](https://docs.rs/encoding_rs_io/0.1.4/encoding_rs_io/struct.DecodeReaderBytesBuilder.html) should get a new option, probably called `bom_sniffing` that is enabled by default but can be disabled. When disabled, BOMs are always ignored, but if an encoding was given, then that encoding is still used. You'll need to add support for this new option in the implementation, and at least one test. This will need to be a separate PR since `encoding_rs_io` lives in its [own repository](https://github.com/BurntSushi/encoding_rs_io). I can cut a new release once this PR is merged. This is the most interesting part of this task; the rest is strictly plumbing.
* Next, [`grep-searcher`'s `SearcherBuilder`](https://docs.rs/grep-searcher/0.1.3/grep_searcher/struct.SearcherBuilder.html) needs to provide its own `bom_sniffing` option that [forwards its value to `DecodeReaderBytesBuilder`](https://github.com/BurntSushi/ripgrep/blob/f19b84fb23e94eb3a738f55d89c3a65225ac4bb3/grep-searcher/src/searcher/mod.rs#L306-L311).
* You'll want to modify ripgrep core itself to make use of this new `bom_sniffing` option. One way of doing this is [modifying the function that interprets which encoding to use from the command line arguments](https://github.com/BurntSushi/ripgrep/blob/f19b84fb23e94eb3a738f55d89c3a65225ac4bb3/src/args.rs#L960-L973). I'd probably do this by replacing the return type, `Result<Option<Encoding>>` with `Result<EncodingMode>`, where `EncodingMode` is a new type defined like so:

```
#[derive(Debug)]
enum EncodingMode {
    // Use an explicit encoding forcefully, but let BOM sniffing override it.
    Some(Encoding),
    // Use only BOM sniffing to auto-detect an encoding.
    Auto,
    // Use no explicit encoding and disable all BOM sniffing. This will
    // always result in searching the raw bytes, regardless of their
    // true encoding.
    Disabled,
}
```

You'll need to add support for a new `none` value for the `encoding` flag. This should be documented in [the encoding flag's docs](https://github.com/BurntSushi/ripgrep/blob/f19b84fb23e94eb3a738f55d89c3a65225ac4bb3/src/app.rs#L983-L994) and to [zsh's auto completion list](https://github.com/BurntSushi/ripgrep/blob/f19b84fb23e94eb3a738f55d89c3a65225ac4bb3/complete/_rg#L381).

At this point, you'll get some compiler errors because the callers of the `encoding` method need to be updated. Specifically, you need to change the [`SearcherBuilder` configuration](https://github.com/BurntSushi/ripgrep/blob/f19b84fb23e94eb3a738f55d89c3a65225ac4bb3/src/args.rs#L759-L770) to use your new type, e.g., setting `bom_sniffing(false)` when you have `EncodingMode::Disabled`. You'll also need to update [PCRE2's use of the `encoding()` method](https://github.com/BurntSushi/ripgrep/blob/f19b84fb23e94eb3a738f55d89c3a65225ac4bb3/src/args.rs#L653-L660). Right now, it disables the UTF-8 check only when an explicit encoding is specified. So you should add a new predictate function to `EncodingMode`, e.g., `has_explicit_encoding`, that can replace the use of `is_some()`. In all other cases, PCRE2 needs to keep its UTF-8 check.

Finally, add a new test covering this feature to [ripgrep's integration tests for new features](https://github.com/BurntSushi/ripgrep/blob/master/tests/feature.rs). If you need help with this let me know, but I think just following some of the pattern of other examples should be good.

Writing all that out makes it seem like a fair bit of work, but I think it's doable!

---

_Comment by @LesnyRumcajs on 2019-03-01 17:04_

@BurntSushi  Woah, thanks a lot for the hints - you saved me several hours of grinding my teeth and potential PR rejects! I'll get to it.

---

_Closed by @BurntSushi on 2019-04-06 14:35_

---
