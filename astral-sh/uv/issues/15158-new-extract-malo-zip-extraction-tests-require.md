---
number: 15158
title: "New extract::malo_* zip extraction tests require network access"
type: issue
state: closed
author: musicinmybrain
labels:
  - bug
assignees: []
created_at: 2025-08-08T03:59:43Z
updated_at: 2025-08-08T11:26:19Z
url: https://github.com/astral-sh/uv/issues/15158
synced_at: 2026-01-10T01:25:53Z
---

# New extract::malo_* zip extraction tests require network access

---

_Issue opened by @musicinmybrain on 2025-08-08 03:59_

### Summary

The  zip extraction tests that were added in 0.8.6 and match the pattern `extract::malo_*` require network access, but are not controlled by any of the testing-related features:

https://github.com/astral-sh/uv/blob/8968d783de58b0c475f65433f8d8668243811210/crates/uv/Cargo.toml#L171-L190

As much as possible, one should be able to run tests offline (e.g., when building a Linux distribution package without network access) without having to explicitly skip particular tests. I’m not sure what to suggest: yet another feature? Or perhaps there is a way to avoid having to fetch the zip file test cases from the network (Cloudflare R2) at all?

### Platform

Fedora Rawhide x86_64

### Version

uv 0.8.6

### Python version

Python 3.14.0rc1

---

_Label `bug` added by @musicinmybrain on 2025-08-08 03:59_

---

_Comment by @musicinmybrain on 2025-08-08 04:00_

Output from running tests without network access:

```
failures:
---- extract::malo_accept_comment stdout ----
thread 'extract::malo_accept_comment' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/accept/comment.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
---- extract::malo_accept_data_descriptor stdout ----
thread 'extract::malo_accept_data_descriptor' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/accept/data_descriptor.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_accept_deflate stdout ----
thread 'extract::malo_accept_deflate' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/accept/deflate.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_accept_data_descriptor_zip64 stdout ----
thread 'extract::malo_accept_data_descriptor_zip64' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/accept/data_descriptor_zip64.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_accept_normal_deflate stdout ----
thread 'extract::malo_accept_normal_deflate' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/accept/normal_deflate.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_accept_store stdout ----
thread 'extract::malo_accept_store' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/accept/store.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_accept_normal_deflate_zip64_extra stdout ----
thread 'extract::malo_accept_normal_deflate_zip64_extra' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/accept/normal_deflate_zip64_extra.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_accept_subdir stdout ----
thread 'extract::malo_accept_subdir' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/accept/subdir.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_accept_zip64_eocd stdout ----
thread 'extract::malo_accept_zip64_eocd' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/accept/zip64_eocd.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_iffy_8bitcomment stdout ----
thread 'extract::malo_iffy_8bitcomment' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/iffy/8bitcomment.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_iffy_extra3byte stdout ----
thread 'extract::malo_iffy_extra3byte' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/iffy/extra3byte.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_iffy_nosubdir stdout ----
thread 'extract::malo_iffy_nosubdir' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/iffy/nosubdir.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_iffy_non_ascii_original_name stdout ----
thread 'extract::malo_iffy_non_ascii_original_name' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/iffy/non_ascii_original_name.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_iffy_prefix stdout ----
thread 'extract::malo_iffy_prefix' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/iffy/prefix.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_iffy_suffix_not_comment stdout ----
thread 'extract::malo_iffy_suffix_not_comment' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/iffy/suffix_not_comment.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_iffy_zip64_eocd_extensible_data stdout ----
thread 'extract::malo_iffy_zip64_eocd_extensible_data' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/iffy/zip64_eocd_extensible_data.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_iffy_zip64_extra_too_long stdout ----
thread 'extract::malo_iffy_zip64_extra_too_long' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/iffy/zip64_extra_too_long.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_iffy_zip64_extra_too_short stdout ----
thread 'extract::malo_iffy_zip64_extra_too_short' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/iffy/zip64_extra_too_short.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_malicious_second_unicode_extra stdout ----
thread 'extract::malo_malicious_second_unicode_extra' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/malicious/second_unicode_extra.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_malicious_short_usize stdout ----
thread 'extract::malo_malicious_short_usize' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/malicious/short_usize.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_malicious_short_usize_zip64 stdout ----
thread 'extract::malo_malicious_short_usize_zip64' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/malicious/short_usize_zip64.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_malicious_zip64_eocd_confusion stdout ----
thread 'extract::malo_malicious_zip64_eocd_confusion' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/malicious/zip64_eocd_confusion.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_malicious_unicode_extra_chain stdout ----
thread 'extract::malo_malicious_unicode_extra_chain' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/malicious/unicode_extra_chain.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_malicious_zipinzip stdout ----
thread 'extract::malo_malicious_zipinzip' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/malicious/zipinzip.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_cd_extra_entry stdout ----
thread 'extract::malo_reject_cd_extra_entry' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/cd_extra_entry.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_cd_missing_entry stdout ----
thread 'extract::malo_reject_cd_missing_entry' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/cd_missing_entry.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_data_descriptor_bad_crc stdout ----
thread 'extract::malo_reject_data_descriptor_bad_crc' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/data_descriptor_bad_crc.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_data_descriptor_bad_crc_0 stdout ----
thread 'extract::malo_reject_data_descriptor_bad_crc_0' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/data_descriptor_bad_crc_0.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_data_descriptor_bad_usize stdout ----
thread 'extract::malo_reject_data_descriptor_bad_usize' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/data_descriptor_bad_usize.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_data_descriptor_bad_usize_no_sig stdout ----
thread 'extract::malo_reject_data_descriptor_bad_usize_no_sig' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/data_descriptor_bad_usize_no_sig.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_data_descriptor_bad_csize stdout ----
thread 'extract::malo_reject_data_descriptor_bad_csize' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/data_descriptor_bad_csize.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_data_descriptor_zip64_csize stdout ----
thread 'extract::malo_reject_data_descriptor_zip64_csize' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/data_descriptor_zip64_csize.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_data_descriptor_zip64_usize stdout ----
thread 'extract::malo_reject_data_descriptor_zip64_usize' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/data_descriptor_zip64_usize.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_shortextra stdout ----
thread 'extract::malo_reject_shortextra' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/shortextra.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_dupe_eocd stdout ----
thread 'extract::malo_reject_dupe_eocd' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/dupe_eocd.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_zip64_extra_csize stdout ----
thread 'extract::malo_reject_zip64_extra_csize' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/zip64_extra_csize.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
---- extract::malo_reject_zip64_extra_usize stdout ----
thread 'extract::malo_reject_zip64_extra_usize' panicked at crates/uv/tests/it/extract.rs:5:44:
called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/reject/zip64_extra_usize.zip", source: hyper_util::client::legacy::Error(Connect, ConnectError("dns error", Custom { kind: Uncategorized, error: "failed to lookup address information: Temporary failure in name resolution" })) }
failures:
    extract::malo_accept_comment
    extract::malo_accept_data_descriptor
    extract::malo_accept_data_descriptor_zip64
    extract::malo_accept_deflate
    extract::malo_accept_normal_deflate
    extract::malo_accept_normal_deflate_zip64_extra
    extract::malo_accept_store
    extract::malo_accept_subdir
    extract::malo_accept_zip64_eocd
    extract::malo_iffy_8bitcomment
    extract::malo_iffy_extra3byte
    extract::malo_iffy_non_ascii_original_name
    extract::malo_iffy_nosubdir
    extract::malo_iffy_prefix
    extract::malo_iffy_suffix_not_comment
    extract::malo_iffy_zip64_eocd_extensible_data
    extract::malo_iffy_zip64_extra_too_long
    extract::malo_iffy_zip64_extra_too_short
    extract::malo_malicious_second_unicode_extra
    extract::malo_malicious_short_usize
    extract::malo_malicious_short_usize_zip64
    extract::malo_malicious_unicode_extra_chain
    extract::malo_malicious_zip64_eocd_confusion
    extract::malo_malicious_zipinzip
    extract::malo_reject_cd_extra_entry
    extract::malo_reject_cd_missing_entry
    extract::malo_reject_data_descriptor_bad_crc
    extract::malo_reject_data_descriptor_bad_crc_0
    extract::malo_reject_data_descriptor_bad_csize
    extract::malo_reject_data_descriptor_bad_usize
    extract::malo_reject_data_descriptor_bad_usize_no_sig
    extract::malo_reject_data_descriptor_zip64_csize
    extract::malo_reject_data_descriptor_zip64_usize
    extract::malo_reject_dupe_eocd
    extract::malo_reject_shortextra
    extract::malo_reject_zip64_extra_csize
    extract::malo_reject_zip64_extra_usize
test result: FAILED. 160 passed; 37 failed; 0 ignored; 0 measured; 0 filtered out; finished in 3.42s
error: test failed, to rerun pass `-p uv --test it`
```

---

_Comment by @charliermarsh on 2025-08-08 09:18_

We could add another feature for it. I didn't really want to check these into the repo (and they also _must_ be installed over HTTPS rather than via local path, though that's solvable).

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-08 09:18_

---

_Referenced in [astral-sh/uv#15160](../../astral-sh/uv/pulls/15160.md) on 2025-08-08 09:30_

---

_Comment by @musicinmybrain on 2025-08-08 10:51_

Seems reasonable enough. Thanks!

---

_Closed by @zanieb on 2025-08-08 11:26_

---
