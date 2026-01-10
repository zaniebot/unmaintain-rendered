---
number: 6030
title: one args conflicts with some others only if condition met
type: issue
state: open
author: jnmdf
labels:
  - C-enhancement
  - A-validators
  - S-waiting-on-design
assignees: []
created_at: 2025-06-10T10:14:06Z
updated_at: 2025-06-10T15:44:17Z
url: https://github.com/clap-rs/clap/issues/6030
synced_at: 2026-01-10T01:28:21Z
---

# one args conflicts with some others only if condition met

---

_Issue opened by @jnmdf on 2025-06-10 10:14_

```rust
/// 支持的哈希算法枚举
#[derive(Debug, Copy, Clone, PartialEq, Eq, PartialOrd, Ord, ValueEnum)]
enum HashAlgorithm {
    Sha1,
    Sha256,
    Sha384,
    Sha512,
}

/// OAEP 填充选项
#[derive(Debug, Parser)]
#[group(required = false, multiple = false)]
struct OaepOptions {
    /// MGF (Mask Generation Function) 哈希算法
    #[clap(long, value_enum, default_value = "sha256")]
    mgf: HashAlgorithm,
    
    /// OAEP 哈希算法
    #[clap(long, value_enum, default_value = "sha256")]
    hash: HashAlgorithm,
    
    /// OAEP 标签 (可选)
    #[clap(long)]
    label: Option<String>,
}

/// 加密参数
#[derive(Debug, Parser)]
#[clap(group(
    ArgGroup::new("padding_options")
        .required(false)
        .multiple(false)
        .args(&["oaep_options"]),
))]
struct EncryptArgs {
    /// 输入文件路径
    input: String,
    
    /// 输出文件路径
    output: String,
    
    /// 公钥文件路径 (PEM 格式)
    #[clap(short, long)]
    public_key: String,
    
    /// 填充方案
    #[clap(short, long, value_enum, default_value = "oaep")]
    padding: PaddingSchemeChoice,
    
    /// OAEP 特定选项 (仅当 padding=oaep 时可用)
    #[clap(flatten)]
    oaep_options: Option<OaepOptions>,
}

/// 填充方案选择
#[derive(Debug, Copy, Clone, PartialEq, Eq, PartialOrd, Ord, ValueEnum)]
enum PaddingSchemeChoice {
    Pkcs1,
    Oaep,
}

```


clap version: 4.5.38
rust version: rustc 1.86.0 (05f9846f8 2025-03-31)
As the code shown above: what i want is making oaep_options conflict with padding if padding value is not oaep (pkcs1 here), in other words, oaep_options should be only allowed if padding=oaep.
After a lot of searching, one method is just ignoring oaep_options if padding != oaep, but in my opinion, raise some error message is more friendly for end users; another idea is using subcommand (make pkcs1 and oaep two subcommands), but i don't like it, because padding is just an option in context of rsa. Anyone has any idea about it?

---

_Label `A-validators` added by @epage on 2025-06-10 15:28_

---

_Label `C-enhancement` added by @epage on 2025-06-10 15:29_

---

_Label `S-waiting-on-design` added by @epage on 2025-06-10 15:29_

---

_Comment by @epage on 2025-06-10 15:44_

As a heads up, there are a lot of different validation needs people have, including
- #4682
- #1801

We're being a bit cautious in what all validators we add because every new feature has an impact on
- Build times
- Binary size
- People's ability to discover the API they need

We would be very interested in a exploring ways to generalize the validation so its more pluggable (#3476).

In the mean time, it seems like a case like this could be handled by validating within your application code.

---
