```yaml
number: 746
title: semi-automatic encoding detection
type: issue
state: closed
author: tbsmark86
labels:
  - enhancement
  - wontfix
assignees: []
created_at: 2018-01-12T21:32:06Z
updated_at: 2018-07-22T16:23:58Z
url: https://github.com/BurntSushi/ripgrep/issues/746
synced_at: 2026-01-12T16:13:22Z
```

# semi-automatic encoding detection

---

_@tbsmark86_

When dealing with directory's with mixed encoding (legacy and utf-8) its impossible to search for certain chars on all files.  (Like German umlaute).

I'd like to see an option that something like "-E cp1252" is only used if the file is invalid as utf-8. There are of course some corner case where a cp1252 file can be valid utf-8 but those should be rare.

I'am completely new to rust - i've hacked something that works for me:


```
--- a/src/decoder.rs
+++ b/src/decoder.rs
@@ -1,7 +1,7 @@
 use std::cmp;
 use std::io::{self, Read};
 
-use encoding_rs::{Decoder, Encoding, UTF_8};
+use encoding_rs::{Decoder, Encoding, UTF_8, DecoderResult};
 
 /// A BOM is at least 2 bytes and at most 3 bytes.
 ///
@@ -139,8 +139,11 @@ pub struct DecodeReader<R, B> {
     /// Whether a "last" read has occurred. After this point, EOF will always
     /// be returned.
     last: bool,
+    /// Only use decode if any invalid utf8 sequence is encounterd
+    try_utf8_first: bool,
     /// The underlying text decoder derived from the BOM, if one exists.
     decoder: Option<Decoder>,
+    utf8_decoder: Decoder,
 }
 
 impl<R: io::Read, B: AsMut<[u8]>> DecodeReader<R, B> {
@@ -167,7 +170,9 @@ impl<R: io::Read, B: AsMut<[u8]>> DecodeReader<R, B> {
             pos: 0,
             first: enc.is_none(),
             last: false,
+            try_utf8_first: true,
             decoder: enc.map(|enc| enc.new_decoder_with_bom_removal()),
+            utf8_decoder: UTF_8.new_decoder_with_bom_removal()
         }
     }
 @@ -255,6 +260,52 @@ impl<R: io::Read, B: AsMut<[u8]>> DecodeReader<R, B> {
         self.decoder = bom.decoder();
         Ok(())
     }
+    
+    fn detect_streaming(&mut self, buf: &mut [u8]) -> io::Result<usize> {
+        assert!(buf.len() >= 4);
+        if self.last {
+            return Ok(0);
+        }
+        if self.pos >= self.buflen {
+            self.fill()?;
+        }
+        let mut nwrite = 0;
+        loop {
+            let (error, nin, nout) =
+                self.utf8_decoder.decode_to_utf8_without_replacement(
+                    &self.buf.as_mut()[self.pos..self.buflen], buf, false);
+            match error {
+                DecoderResult::Malformed(_, _) => {
+                    if nwrite > 0 {
+                        return Ok(nwrite);
+                    }
+                    return self.transcode(buf);
+                }
+                _ => {}
+            }
+            self.pos += nin;
+            nwrite += nout;
+            // If we've written at least one byte to the caller-provided
+            // buffer, then our mission is complete.
+            if nwrite > 0 {
+                break;
+            }
+            // Otherwise, we know that our internal buffer has insufficient
+            // data to transcode at least one char, so we attempt to refill it.
+            self.fill()?;
+            // Quit on EOF.
+            if self.buflen == 0 {
+                self.pos = 0;
+                self.last = true;
+                let (_, _, nout, _) =
+                    self.utf8_decoder.decode_to_utf8(
+                        &[], buf, true);
+                return Ok(nout);
+            }
+        }
+        Ok(nwrite)
+
+    }
 }
 
 impl<R: io::Read, B: AsMut<[u8]>> io::Read for DecodeReader<R, B> {
@@ -276,6 +327,9 @@ impl<R: io::Read, B: AsMut<[u8]>> io::Read for DecodeReader<R, B> {
                 io::ErrorKind::Other,
                 "DecodeReader: byte buffer must have length at least 4"));
         }
+        if self.try_utf8_first {
+            return self.detect_streaming(buf);
+        }
         self.transcode(buf)
     }
 }
```


Obviously this is not really optional, also it may be better if this a kind of child-class that's only active while this option is used.

---

_Renamed from "Semi-automatic encoding detection" to "semi-automatic encoding detection" by @BurntSushi on 2018-01-31 02:07_

---

_Label `enhancement` added by @BurntSushi on 2018-01-31 02:07_

---

_Label `help wanted` added by @BurntSushi on 2018-01-31 02:07_

---

_Comment by @BurntSushi on 2018-01-31 02:08_

I agree this would be a nice feature. Thanks for reporting it. Thanks for the patch as well.

I think before we work on any code for this, we need a strong specification that outlines ripgrep's automatic encoding detection behavior. We need to be careful going down this road because encoding detection can never be bullet proof, which exposes us to bug reports that we cannot fix. For that reason, while I do think this would be cool, I'm not actually quite convinced that we should definitely do this. I'll leave this issue open for now.

---

_Comment by @tbsmark86 on 2018-02-26 19:12_

Yes automatic detection can't be bullet proof.

But if it is never documented as 'automatic' won't that solve the bug report problem?

Something like that:

-E --encoding 
          Specify the text ...
     --fallback-encoding <ENCODING>
           Specify the text encoding to use if a file can not be read as UTF-8.
          Mutally exclusive to --encoding.



---

_Comment by @BurntSushi on 2018-07-22 16:23_

Here are my thoughts:

1. While I in principle am not opposed to some sort of "smart" encoding detection, it is not something that I want to maintain. It seems like something that will forever be buggy. If someone else wants to maintain a high quality crate for doing automatic encoding detection and can slide in as a `Read` adapter easily, then I might consider using it.
2. With the new `--pre` flag, it is actually now possible to do this via a user provided program.

In light of that, I'm going to close this. Thanks for the thoughts though!

---

_Closed by @BurntSushi on 2018-07-22 16:23_

---

_Label `wontfix` added by @BurntSushi on 2018-07-22 16:23_

---

_Label `help wanted` removed by @BurntSushi on 2018-07-22 16:23_

---
