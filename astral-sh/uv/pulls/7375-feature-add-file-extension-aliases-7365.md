```yaml
number: 7375
title: Feature/add file extension aliases 7365
type: pull_request
state: closed
author: Aditya-PS-05
labels: []
assignees: []
base: main
head: feature/add-file-extension-aliases-7365
created_at: 2024-09-13T19:31:47Z
updated_at: 2024-09-14T07:50:29Z
url: https://github.com/astral-sh/uv/pull/7375
synced_at: 2026-01-12T16:07:48Z
```

# Feature/add file extension aliases 7365

---

_@Aditya-PS-05_

## Summary
This pull request adds support for additional file extension aliases in the SourceDistExtension and ExtensionError enums. The newly supported file extensions include .tbz, .tgz, .txz, .tar.lz, .tar.lzma. These changes align the extensions supported by the SourceDistExtension with those used in Python packaging tools, enhancing compatibility with a broader range of source distribution formats.

closes #7201 

## Test Plan
should be added or updated to verify that the new extensions are correctly recognized as valid source distributions and that errors are correctly raised when unsupported extensions are provided.


---

_@charliermarsh reviewed on 2024-09-13 19:36_

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/extension.rs`:73 on 2024-09-13 19:36_

It might be that `.tar` is just uncompressed tar without gzip?

---

_@Aditya-PS-05 reviewed on 2024-09-13 19:43_

---

_Review comment by @Aditya-PS-05 on `crates/distribution-filename/src/extension.rs`:73 on 2024-09-13 19:43_

Thanks for suggestion.

---

_Closed by @Aditya-PS-05 on 2024-09-13 19:43_

---

_@Aditya-PS-05 reviewed on 2024-09-13 19:59_

---

_Review comment by @Aditya-PS-05 on `crates/distribution-filename/src/extension.rs`:73 on 2024-09-13 19:59_

Should I need to update the SourceDistExtension enum?

---

_Reopened by @Aditya-PS-05 on 2024-09-13 20:00_

---

_Closed by @Aditya-PS-05 on 2024-09-13 20:05_

---

_@charliermarsh reviewed on 2024-09-13 20:07_

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/extension.rs`:73 on 2024-09-13 20:07_

I believe so, yeah.

---

_@charliermarsh reviewed on 2024-09-14 00:33_

---

_Review comment by @charliermarsh on `crates/distribution-filename/src/extension.rs`:73 on 2024-09-14 00:33_

Do you want some help finishing this off?

---

_@Aditya-PS-05 reviewed on 2024-09-14 07:38_

---

_Review comment by @Aditya-PS-05 on `crates/distribution-filename/src/extension.rs`:73 on 2024-09-14 07:38_

Yeah I need some help, 

in extension.rs file: 
```
pub enum SourceDistExtension {
    Zip,
    TarGz,
    TarBz2,
    TarXz,
    Tar,
    TarZst,
    TarLz,
    TarLzma,
}
```

```
match extension {
            "zip" => Ok(Self::Zip),
            "gz" if is_tar(path.as_ref()) => Ok(Self::TarGz),
            "tgz" => Ok(Self::TarGz),
            "tar" => Ok(Self::Tar),
            "bz2" | "tbz" if is_tar(path.as_ref()) => Ok(Self::TarBz2),
            "xz" | "txz" | "tlz" | "tar.lz" | "tar.lzma" if is_tar(path.as_ref()) => {
                Ok(Self::TarXz)
            }
            "zst" if is_tar(path.as_ref()) => Ok(Self::TarZst),
            _ => Err(ExtensionError::SourceDist),
        }
```
```
impl Display for SourceDistExtension {
    fn fmt(&self, f: &mut Formatter<'_>) -> std::fmt::Result {
        match self {
            Self::Zip => f.write_str("zip"),
            Self::TarGz => f.write_str("tar.gz"),
            Self::TarBz2 => f.write_str("tar.bz2"),
            Self::TarXz => f.write_str("tar.xz"),
            Self::Tar => f.write_str("tar"),
            Self::TarZst => f.write_str("tar.zst"),
            Self::TarLz => f.write_str("tar.lz"),
            Self::TarLzma => f.write_str("tar.lzma"),
        }
    }
}

// #[derive(Error, Debug)]
pub enum ExtensionError {
    #[error("`.whl`, `.zip`, `.tar.gz`, `.tgz`, `.tar.bz2`, `.tbz`, `.tar.xz`, `.txz`, `.tar.lz`, `.tar.lzma`, `.tar.zst`")]
    Dist,
    #[error("`.zip`, `.tar.gz`, `.tgz`, `.tar.bz2`, `.tbz`, `.tar.xz`, `.txz`, `.tar.lz`, `.tar.lzma`, `.tar.zst`")]
    SourceDist,
}

```

Where am I going wrong, as some tests are being failed. I tried to took inspiration from the pr #7201 as he done the same thing. 

---

_Reopened by @Aditya-PS-05 on 2024-09-14 07:49_

---

_Closed by @Aditya-PS-05 on 2024-09-14 07:50_

---
