```yaml
number: 7224
title: Deduplicate source dist unpack code
type: pull_request
state: closed
author: konstin
labels: []
assignees: []
base: main
head: konsti/remove-some-duplication-untar
created_at: 2024-09-09T16:33:33Z
updated_at: 2024-09-24T10:33:40Z
url: https://github.com/astral-sh/uv/pull/7224
synced_at: 2026-01-12T16:07:44Z
```

# Deduplicate source dist unpack code

---

_@konstin_

Use a generic instead of having the same code again.

---

_Comment by @charliermarsh on 2024-09-09 16:34_

Can you check the llvm-lines before and after just to be safe?

---

_Comment by @konstin on 2024-09-09 16:44_

Before: 1569748
After: 1570933

---

_@charliermarsh reviewed on 2024-09-09 16:46_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:165 on 2024-09-09 16:46_

Can you just make this fn take the `BufReader`? And move the first few lines out to the caller?

---

_Comment by @konstin on 2024-09-09 18:38_

With main, this branch and the branch plus the batch below i get the following numbers for release. @BurntSushi do you know why we get more llvm lines by removing duplicates for a generic?

patch:   5905096                117569                (TOTAL)
branch:  5904673                117543                (TOTAL)
main:    5904053                117523                (TOTAL)


```diff
diff --git a/crates/uv-extract/src/stream.rs b/crates/uv-extract/src/stream.rs
index c23f20755..79f3ac26d 100644
--- a/crates/uv-extract/src/stream.rs
+++ b/crates/uv-extract/src/stream.rs
@@ -104,8 +104,8 @@ pub async fn unzip<R: tokio::io::AsyncRead + Unpin>(
 /// Unpack the given tar archive into the destination directory.
 ///
 /// This is equivalent to `archive.unpack_in(dst)`, but it also preserves the executable bit.
-async fn untar_in<'a>(
-    mut archive: tokio_tar::Archive<&'a mut (dyn tokio::io::AsyncRead + Unpin)>,
+async fn untar_in<'a, T: tokio::io::AsyncRead + Unpin>(
+    mut archive: tokio_tar::Archive<&'a mut T>,
     dst: &Path,
 ) -> std::io::Result<()> {
     let mut entries = archive.entries()?;
@@ -157,22 +157,17 @@ async fn untar_in<'a>(
 /// Unzip a `.tar.*` archive into the target directory, without requiring `Seek`.
 ///
 /// This is useful for unpacking files as they're being downloaded.
-pub(crate) async fn untar_compression<
-    R: tokio::io::AsyncRead + Unpin,
-    Decoder: tokio::io::AsyncRead + Unpin,
->(
+async fn untar_compression<'a, R: tokio::io::AsyncRead + Unpin + 'a>(
     reader: R,
-    decoder: impl Fn(tokio::io::BufReader<R>) -> Decoder,
+    decoder: impl Fn(tokio::io::BufReader<R>) -> Box<dyn tokio::io::AsyncRead + Unpin + 'a>,
     target: impl AsRef<Path>,
 ) -> Result<(), Error> {
     let reader = tokio::io::BufReader::with_capacity(DEFAULT_BUF_SIZE, reader);
     let mut decompressed_bytes = decoder(reader);

-    let archive = tokio_tar::ArchiveBuilder::new(
-        &mut decompressed_bytes as &mut (dyn tokio::io::AsyncRead + Unpin),
-    )
-    .set_preserve_mtime(false)
-    .build();
+    let archive = tokio_tar::ArchiveBuilder::new(&mut decompressed_bytes)
+        .set_preserve_mtime(false)
+        .build();
     Ok(untar_in(archive, target.as_ref()).await?)
 }

@@ -190,7 +185,7 @@ pub async fn archive<R: tokio::io::AsyncRead + Unpin>(
         SourceDistExtension::TarGz => {
             untar_compression(
                 reader,
-                async_compression::tokio::bufread::GzipDecoder::new,
+                |x| Box::new(async_compression::tokio::bufread::GzipDecoder::new(x)),
                 target,
             )
             .await?;
@@ -198,7 +193,7 @@ pub async fn archive<R: tokio::io::AsyncRead + Unpin>(
         SourceDistExtension::TarBz2 => {
             untar_compression(
                 reader,
-                async_compression::tokio::bufread::BzDecoder::new,
+                |x| Box::new(async_compression::tokio::bufread::BzDecoder::new(x)),
                 target,
             )
             .await?;
@@ -206,7 +201,7 @@ pub async fn archive<R: tokio::io::AsyncRead + Unpin>(
         SourceDistExtension::TarXz => {
             untar_compression(
                 reader,
-                async_compression::tokio::bufread::XzDecoder::new,
+                |x| Box::new(async_compression::tokio::bufread::XzDecoder::new(x)),
                 target,
             )
             .await?;
@@ -214,7 +209,7 @@ pub async fn archive<R: tokio::io::AsyncRead + Unpin>(
         SourceDistExtension::TarZst => {
             untar_compression(
                 reader,
-                async_compression::tokio::bufread::ZstdDecoder::new,
+                |x| Box::new(async_compression::tokio::bufread::ZstdDecoder::new(x)),
                 target,
             )
             .await?;
```

---

_Closed by @konstin on 2024-09-24 10:33_

---
