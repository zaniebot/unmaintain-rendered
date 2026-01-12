```yaml
number: 3300
title: "-h short help switches from single to multiple lines when adding a value_name entry"
type: issue
state: open
author: dandavison
labels:
  - C-bug
  - A-help
  - S-waiting-on-design
  - M-minor-incompat
assignees: []
created_at: 2022-01-16T22:19:57Z
updated_at: 2023-07-17T13:14:08Z
url: https://github.com/clap-rs/clap/issues/3300
synced_at: 2026-01-12T16:14:14Z
```

# -h short help switches from single to multiple lines when adding a value_name entry

---

_@dandavison_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.58.0 (02072b482 2022-01-11)

### Clap Version

`clap = { version = "3.0.5", features = ["derive"] }`

### Minimal reproducible code

I'm sorry, I have not been able to identify a small example yet. But I do have a reproducible example:

```sh
mkdir /tmp/clap-bug-in-delta
cd /tmp/clap-bug-in-delta
git clone git@github.com:dandavison/delta.git
cd delta
git fetch origin clap-bug-maybe
git checkout clap-bug-maybe
git checkout clap-bug-maybe~1
cargo build --release
echo "==========================================================================================="
echo "=> The following output will be formatted with one line per option. THIS IS WHAT I EXPECT."
echo "==========================================================================================="
echo
./target/release/delta -h | head -n 20 | tail -n 10
git checkout clap-bug-maybe
cargo build --release
echo "======================================================================================================"
echo "=> The following command will be formatted with multiple lines per option. THIS IS NOT WHAT I EXPECT."
echo "======================================================================================================"
echo
./target/release/delta -h | head -n 20 | tail -n 10
echo
echo "==========================================="
echo "=> But why did it change? This is the diff:"
echo "==========================================="
git -P diff clap-bug-maybe~1 clap-bug-maybe
```

### Steps to reproduce the bug with the above code

Run the shell script provided above.

### Actual Behaviour

When I run the above shell script, I get the following output:

![image](https://user-images.githubusercontent.com/52205/149682334-d06a1b0b-f94c-4d29-a031-27d892c652bd.png)

### Expected Behaviour

I expect `delta -h` to always produce help text formatted as one-line-per-option.

I think that the code change made in the shell script should not cause clap to change its behavior from one-line-per-option formatting to multple-lines-per-option.

### Additional Context

_No response_

### Debug Output

<details>

```
[            clap::build::app] 	App::_do_parse
[            clap::build::app] 	App::_build
[            clap::build::app] 	App::_propagate:delta
[            clap::build::app] 	App::_check_help_and_version
[            clap::build::app] 	App::_propagate_global_args:delta
[            clap::build::app] 	App::_derive_display_order:delta
[         clap::parse::parser] 	Parser::get_matches_with
[         clap::parse::parser] 	Parser::_build
[      clap::parse::validator] 	Validator::validate
[         clap::parse::parser] 	Parser::add_defaults
[         clap::parse::parser] 	Parser::add_defaults:iter:blame-code-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:blame-code-style: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:blame-code-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:blame-format:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:blame-format: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:blame-format: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 56CDDC4DCF7620D1]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."{timestamp:<15} {author:<15.14} {commit:<8}"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=1
[            clap::build::app] 	App::groups_for_arg: id=[hash: 56CDDC4DCF7620D1]
[         clap::parse::parser] 	Parser::add_value:iter:blame-format: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:blame-palette:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:blame-palette: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:blame-palette: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:blame-separator:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:blame-separator: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:blame-separator: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: FA4C0A49E4E824EB]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."│"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=2
[            clap::build::app] 	App::groups_for_arg: id=[hash: FA4C0A49E4E824EB]
[         clap::parse::parser] 	Parser::add_value:iter:blame-separator: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:blame-separator-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:blame-separator-style: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:blame-separator-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:blame-timestamp-format:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:blame-timestamp-format: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:blame-timestamp-format: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 384A5AE250BC4560]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."%Y-%m-%d %H:%M:%S %z"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=3
[            clap::build::app] 	App::groups_for_arg: id=[hash: 384A5AE250BC4560]
[         clap::parse::parser] 	Parser::add_value:iter:blame-timestamp-format: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:commit-decoration-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:commit-decoration-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:commit-decoration-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5DD1AA0AA564F4FE]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val...""
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=4
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5DD1AA0AA564F4FE]
[         clap::parse::parser] 	Parser::add_value:iter:commit-decoration-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:commit-regex:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:commit-regex: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:commit-regex: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 9B3E8EF39C82E397]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."^commit "
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=5
[            clap::build::app] 	App::groups_for_arg: id=[hash: 9B3E8EF39C82E397]
[         clap::parse::parser] 	Parser::add_value:iter:commit-regex: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:commit-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:commit-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:commit-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5599B14D59DA9C11]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."raw"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=6
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5599B14D59DA9C11]
[         clap::parse::parser] 	Parser::add_value:iter:commit-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:default-language:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:default-language: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:default-language: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:diff-stat-align-width:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:diff-stat-align-width: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:diff-stat-align-width: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 1871FF364E95D4A7]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."48"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=7
[            clap::build::app] 	App::groups_for_arg: id=[hash: 1871FF364E95D4A7]
[         clap::parse::parser] 	Parser::add_value:iter:diff-stat-align-width: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:features:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:features: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:features: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:file-added-label:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:file-added-label: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:file-added-label: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5E0D24BBCDB34988]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."added:"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=8
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5E0D24BBCDB34988]
[         clap::parse::parser] 	Parser::add_value:iter:file-added-label: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:file-copied-label:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:file-copied-label: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:file-copied-label: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: A70BBAD462A5D1B2]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."copied:"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=9
[            clap::build::app] 	App::groups_for_arg: id=[hash: A70BBAD462A5D1B2]
[         clap::parse::parser] 	Parser::add_value:iter:file-copied-label: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:file-decoration-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:file-decoration-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:file-decoration-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 137B4D7D7D1881A1]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."blue ul"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=10
[            clap::build::app] 	App::groups_for_arg: id=[hash: 137B4D7D7D1881A1]
[         clap::parse::parser] 	Parser::add_value:iter:file-decoration-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:file-modified-label:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:file-modified-label: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:file-modified-label: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: DBA64F5F3C7FBF57]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val...""
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=11
[            clap::build::app] 	App::groups_for_arg: id=[hash: DBA64F5F3C7FBF57]
[         clap::parse::parser] 	Parser::add_value:iter:file-modified-label: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:file-removed-label:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:file-removed-label: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:file-removed-label: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: E77494CD87F1D2DA]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."removed:"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=12
[            clap::build::app] 	App::groups_for_arg: id=[hash: E77494CD87F1D2DA]
[         clap::parse::parser] 	Parser::add_value:iter:file-removed-label: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:file-renamed-label:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:file-renamed-label: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:file-renamed-label: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 14DA47432C8CFA60]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."renamed:"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=13
[            clap::build::app] 	App::groups_for_arg: id=[hash: 14DA47432C8CFA60]
[         clap::parse::parser] 	Parser::add_value:iter:file-renamed-label: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:file-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:file-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:file-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 58E5E5E2FBEF5F00]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."blue"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=14
[            clap::build::app] 	App::groups_for_arg: id=[hash: 58E5E5E2FBEF5F00]
[         clap::parse::parser] 	Parser::add_value:iter:file-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:file-regex-replacement:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:file-regex-replacement: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:file-regex-replacement: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:grep-context-line-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:grep-context-line-style: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:grep-context-line-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:grep-file-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:grep-file-style: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:grep-file-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:grep-line-number-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:grep-line-number-style: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:grep-line-number-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:grep-match-line-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:grep-match-line-style: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:grep-match-line-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:grep-match-word-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:grep-match-word-style: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:grep-match-word-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:grep-separator-symbol:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:grep-separator-symbol: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:grep-separator-symbol: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 13CED1EB3B3A0007]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val...":"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=15
[            clap::build::app] 	App::groups_for_arg: id=[hash: 13CED1EB3B3A0007]
[         clap::parse::parser] 	Parser::add_value:iter:grep-separator-symbol: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:hunk-header-decoration-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-decoration-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-decoration-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 82F533ECABFA714B]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."blue box"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=16
[            clap::build::app] 	App::groups_for_arg: id=[hash: 82F533ECABFA714B]
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-decoration-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:hunk-header-file-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-file-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-file-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 9D90C893B76CB18B]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."blue"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=17
[            clap::build::app] 	App::groups_for_arg: id=[hash: 9D90C893B76CB18B]
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-file-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:hunk-header-line-number-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-line-number-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-line-number-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 9FC6FE4D36EF8BF9]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."blue"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=18
[            clap::build::app] 	App::groups_for_arg: id=[hash: 9FC6FE4D36EF8BF9]
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-line-number-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:hunk-header-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: B38D31D507F3E90A]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."line-number syntax"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=19
[            clap::build::app] 	App::groups_for_arg: id=[hash: B38D31D507F3E90A]
[         clap::parse::parser] 	Parser::add_value:iter:hunk-header-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:hunk-label:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:hunk-label: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:hunk-label: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 18DD752B05658C3F]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val...""
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=20
[            clap::build::app] 	App::groups_for_arg: id=[hash: 18DD752B05658C3F]
[         clap::parse::parser] 	Parser::add_value:iter:hunk-label: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:hyperlinks-commit-link-format:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:hyperlinks-commit-link-format: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:hyperlinks-commit-link-format: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:hyperlinks-file-link-format:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:hyperlinks-file-link-format: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:hyperlinks-file-link-format: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: DFA80C8E158D7D7F]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."file://{path}"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=21
[            clap::build::app] 	App::groups_for_arg: id=[hash: DFA80C8E158D7D7F]
[         clap::parse::parser] 	Parser::add_value:iter:hyperlinks-file-link-format: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:inline-hint-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:inline-hint-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:inline-hint-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 546A0AABF585531B]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."blue"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=22
[            clap::build::app] 	App::groups_for_arg: id=[hash: 546A0AABF585531B]
[         clap::parse::parser] 	Parser::add_value:iter:inline-hint-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:inspect-raw-lines:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:inspect-raw-lines: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:inspect-raw-lines: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 41D2EAB3710EDBCF]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."true"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=23
[            clap::build::app] 	App::groups_for_arg: id=[hash: 41D2EAB3710EDBCF]
[         clap::parse::parser] 	Parser::add_value:iter:inspect-raw-lines: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:line-buffer-size:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:line-buffer-size: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:line-buffer-size: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 15CB59E2C5B60FAB]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."32"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=24
[            clap::build::app] 	App::groups_for_arg: id=[hash: 15CB59E2C5B60FAB]
[         clap::parse::parser] 	Parser::add_value:iter:line-buffer-size: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:line-fill-method:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:line-fill-method: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:line-fill-method: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:line-numbers-left-format:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-left-format: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-left-format: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: BD41D6EAA992C433]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."{nm:^4}⋮"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=25
[            clap::build::app] 	App::groups_for_arg: id=[hash: BD41D6EAA992C433]
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-left-format: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:line-numbers-left-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-left-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-left-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 8FB24FA5E22D8B07]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=26
[            clap::build::app] 	App::groups_for_arg: id=[hash: 8FB24FA5E22D8B07]
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-left-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:line-numbers-minus-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-minus-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-minus-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5624D3D8642F6C26]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=27
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5624D3D8642F6C26]
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-minus-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:line-numbers-plus-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-plus-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-plus-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 7D9D82C06A7A0180]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=28
[            clap::build::app] 	App::groups_for_arg: id=[hash: 7D9D82C06A7A0180]
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-plus-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:line-numbers-right-format:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-right-format: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-right-format: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 43EF7A488FEFC82A]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."{np:^4}│"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=29
[            clap::build::app] 	App::groups_for_arg: id=[hash: 43EF7A488FEFC82A]
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-right-format: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:line-numbers-right-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-right-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-right-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 181D448BDFC5D338]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=30
[            clap::build::app] 	App::groups_for_arg: id=[hash: 181D448BDFC5D338]
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-right-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:line-numbers-zero-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-zero-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-zero-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 177F1774C56C3442]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=31
[            clap::build::app] 	App::groups_for_arg: id=[hash: 177F1774C56C3442]
[         clap::parse::parser] 	Parser::add_value:iter:line-numbers-zero-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:map-styles:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:map-styles: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:map-styles: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:max-line-distance:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:max-line-distance: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:max-line-distance: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 73D3CD19BFF4DC1B]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."0.6"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=32
[            clap::build::app] 	App::groups_for_arg: id=[hash: 73D3CD19BFF4DC1B]
[         clap::parse::parser] 	Parser::add_value:iter:max-line-distance: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:max-line-length:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:max-line-length: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:max-line-length: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 12E83F165DADB6A]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."512"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=33
[            clap::build::app] 	App::groups_for_arg: id=[hash: 12E83F165DADB6A]
[         clap::parse::parser] 	Parser::add_value:iter:max-line-length: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:merge-conflict-begin-symbol:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-begin-symbol: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-begin-symbol: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: F4ABA8A495C126A4]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."▼"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=34
[            clap::build::app] 	App::groups_for_arg: id=[hash: F4ABA8A495C126A4]
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-begin-symbol: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:merge-conflict-end-symbol:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-end-symbol: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-end-symbol: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 1178E72A4D87EF48]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."▲"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=35
[            clap::build::app] 	App::groups_for_arg: id=[hash: 1178E72A4D87EF48]
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-end-symbol: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:merge-conflict-ours-diff-header-decoration-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-ours-diff-header-decoration-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-ours-diff-header-decoration-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5BE7A142A6D66B90]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."box"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=36
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5BE7A142A6D66B90]
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-ours-diff-header-decoration-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:merge-conflict-ours-diff-header-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-ours-diff-header-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-ours-diff-header-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 4D2E6278BEC5F49B]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."normal"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=37
[            clap::build::app] 	App::groups_for_arg: id=[hash: 4D2E6278BEC5F49B]
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-ours-diff-header-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:merge-conflict-theirs-diff-header-decoration-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-theirs-diff-header-decoration-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-theirs-diff-header-decoration-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 49E296EF7CB9631A]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."box"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=38
[            clap::build::app] 	App::groups_for_arg: id=[hash: 49E296EF7CB9631A]
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-theirs-diff-header-decoration-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:merge-conflict-theirs-diff-header-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-theirs-diff-header-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-theirs-diff-header-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: B78D2CADB774D71D]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."normal"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=39
[            clap::build::app] 	App::groups_for_arg: id=[hash: B78D2CADB774D71D]
[         clap::parse::parser] 	Parser::add_value:iter:merge-conflict-theirs-diff-header-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:minus-empty-line-marker-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:minus-empty-line-marker-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:minus-empty-line-marker-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 1A356B5CE9B72F46]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."normal auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=40
[            clap::build::app] 	App::groups_for_arg: id=[hash: 1A356B5CE9B72F46]
[         clap::parse::parser] 	Parser::add_value:iter:minus-empty-line-marker-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:minus-emph-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:minus-emph-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:minus-emph-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 80925DC4BC9352A9]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."normal auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=41
[            clap::build::app] 	App::groups_for_arg: id=[hash: 80925DC4BC9352A9]
[         clap::parse::parser] 	Parser::add_value:iter:minus-emph-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:minus-non-emph-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:minus-non-emph-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:minus-non-emph-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5A69F0A80EA7DB13]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."minus-style"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=42
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5A69F0A80EA7DB13]
[         clap::parse::parser] 	Parser::add_value:iter:minus-non-emph-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:minus-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:minus-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:minus-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: ECC7DC1B3D93347C]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."normal auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=43
[            clap::build::app] 	App::groups_for_arg: id=[hash: ECC7DC1B3D93347C]
[         clap::parse::parser] 	Parser::add_value:iter:minus-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:navigate-regex:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:navigate-regex: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:navigate-regex: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:pager:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:pager: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:pager: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:paging-mode:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:paging-mode: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:paging-mode: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5D0570B9ADC7168]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=44
[            clap::build::app] 	App::groups_for_arg: id=[hash: 5D0570B9ADC7168]
[         clap::parse::parser] 	Parser::add_value:iter:paging-mode: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:plus-emph-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:plus-emph-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:plus-emph-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: C585956EEB195C27]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."syntax auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=45
[            clap::build::app] 	App::groups_for_arg: id=[hash: C585956EEB195C27]
[         clap::parse::parser] 	Parser::add_value:iter:plus-emph-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:plus-empty-line-marker-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:plus-empty-line-marker-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:plus-empty-line-marker-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 98090FB09CDF4FE8]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."normal auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=46
[            clap::build::app] 	App::groups_for_arg: id=[hash: 98090FB09CDF4FE8]
[         clap::parse::parser] 	Parser::add_value:iter:plus-empty-line-marker-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:plus-non-emph-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:plus-non-emph-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:plus-non-emph-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 9B3DF14708B1895]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."plus-style"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=47
[            clap::build::app] 	App::groups_for_arg: id=[hash: 9B3DF14708B1895]
[         clap::parse::parser] 	Parser::add_value:iter:plus-non-emph-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:plus-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:plus-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:plus-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: D1D1DAD3D2E91286]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."syntax auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=48
[            clap::build::app] 	App::groups_for_arg: id=[hash: D1D1DAD3D2E91286]
[         clap::parse::parser] 	Parser::add_value:iter:plus-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:right-arrow:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:right-arrow: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:right-arrow: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 7738710EF51350A]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."⟶  "
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=49
[            clap::build::app] 	App::groups_for_arg: id=[hash: 7738710EF51350A]
[         clap::parse::parser] 	Parser::add_value:iter:right-arrow: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:syntax-theme:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:syntax-theme: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:syntax-theme: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:tab-width:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:tab-width: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:tab-width: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: E73E3DA9AFBB160E]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."4"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=50
[            clap::build::app] 	App::groups_for_arg: id=[hash: E73E3DA9AFBB160E]
[         clap::parse::parser] 	Parser::add_value:iter:tab-width: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:true-color:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:true-color: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:true-color: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: CB05E52F01F539B0]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=51
[            clap::build::app] 	App::groups_for_arg: id=[hash: CB05E52F01F539B0]
[         clap::parse::parser] 	Parser::add_value:iter:true-color: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:whitespace-error-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:whitespace-error-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:whitespace-error-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: D4059E3D0A877EA4]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."auto auto"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=52
[            clap::build::app] 	App::groups_for_arg: id=[hash: D4059E3D0A877EA4]
[         clap::parse::parser] 	Parser::add_value:iter:whitespace-error-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:width:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:width: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:width: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:tokenization-regex:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:tokenization-regex: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:tokenization-regex: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 3C8F7BB37CE99C65]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."\\w+"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=53
[            clap::build::app] 	App::groups_for_arg: id=[hash: 3C8F7BB37CE99C65]
[         clap::parse::parser] 	Parser::add_value:iter:tokenization-regex: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:wrap-left-symbol:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:wrap-left-symbol: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:wrap-left-symbol: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 43DF3645718DA9D5]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."↵"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=54
[            clap::build::app] 	App::groups_for_arg: id=[hash: 43DF3645718DA9D5]
[         clap::parse::parser] 	Parser::add_value:iter:wrap-left-symbol: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:wrap-max-lines:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:wrap-max-lines: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:wrap-max-lines: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: D9D802A90920E805]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."2"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=55
[            clap::build::app] 	App::groups_for_arg: id=[hash: D9D802A90920E805]
[         clap::parse::parser] 	Parser::add_value:iter:wrap-max-lines: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:wrap-right-percent:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:wrap-right-percent: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:wrap-right-percent: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 3ED2737676D6557F]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."37.0"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=56
[            clap::build::app] 	App::groups_for_arg: id=[hash: 3ED2737676D6557F]
[         clap::parse::parser] 	Parser::add_value:iter:wrap-right-percent: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:wrap-right-prefix-symbol:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:wrap-right-prefix-symbol: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:wrap-right-prefix-symbol: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: DEEB553C2C961575]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."…"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=57
[            clap::build::app] 	App::groups_for_arg: id=[hash: DEEB553C2C961575]
[         clap::parse::parser] 	Parser::add_value:iter:wrap-right-prefix-symbol: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:wrap-right-symbol:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:wrap-right-symbol: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:wrap-right-symbol: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: DAE0BAD1EA22B0EA]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."↴"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=58
[            clap::build::app] 	App::groups_for_arg: id=[hash: DAE0BAD1EA22B0EA]
[         clap::parse::parser] 	Parser::add_value:iter:wrap-right-symbol: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:zero-style:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:zero-style: has default vals
[         clap::parse::parser] 	Parser::add_value:iter:zero-style: wasn't used
[            clap::build::app] 	App::groups_for_arg: id=[hash: 4249AAFB8024F360]
[         clap::parse::parser] 	Parser::add_single_val_to_arg: adding val..."syntax normal"
[         clap::parse::parser] 	Parser::add_single_val_to_arg: cur_idx:=59
[            clap::build::app] 	App::groups_for_arg: id=[hash: 4249AAFB8024F360]
[         clap::parse::parser] 	Parser::add_value:iter:zero-style: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:24-bit-color:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:24-bit-color: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:24-bit-color: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:minus-file:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:minus-file: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:minus-file: doesn't have default missing vals
[         clap::parse::parser] 	Parser::add_defaults:iter:plus-file:
[         clap::parse::parser] 	Parser::add_value: doesn't have conditional defaults
[         clap::parse::parser] 	Parser::add_value:iter:plus-file: doesn't have default vals
[         clap::parse::parser] 	Parser::add_value:iter:plus-file: doesn't have default missing vals
[      clap::parse::validator] 	Validator::validate_conflicts
[      clap::parse::validator] 	Validator::validate_exclusive
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 56CDDC4DCF7620D1]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: FA4C0A49E4E824EB]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 384A5AE250BC4560]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 5DD1AA0AA564F4FE]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 9B3E8EF39C82E397]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 5599B14D59DA9C11]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 1871FF364E95D4A7]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 5E0D24BBCDB34988]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: A70BBAD462A5D1B2]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 137B4D7D7D1881A1]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: DBA64F5F3C7FBF57]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: E77494CD87F1D2DA]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 14DA47432C8CFA60]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 58E5E5E2FBEF5F00]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 13CED1EB3B3A0007]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 82F533ECABFA714B]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 9D90C893B76CB18B]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 9FC6FE4D36EF8BF9]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: B38D31D507F3E90A]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 18DD752B05658C3F]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: DFA80C8E158D7D7F]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 546A0AABF585531B]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 41D2EAB3710EDBCF]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 15CB59E2C5B60FAB]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: BD41D6EAA992C433]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 8FB24FA5E22D8B07]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 5624D3D8642F6C26]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 7D9D82C06A7A0180]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 43EF7A488FEFC82A]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 181D448BDFC5D338]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 177F1774C56C3442]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 73D3CD19BFF4DC1B]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 12E83F165DADB6A]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: F4ABA8A495C126A4]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 1178E72A4D87EF48]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 5BE7A142A6D66B90]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 4D2E6278BEC5F49B]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 49E296EF7CB9631A]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: B78D2CADB774D71D]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 1A356B5CE9B72F46]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 80925DC4BC9352A9]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 5A69F0A80EA7DB13]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: ECC7DC1B3D93347C]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 5D0570B9ADC7168]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: C585956EEB195C27]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 98090FB09CDF4FE8]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 9B3DF14708B1895]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: D1D1DAD3D2E91286]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 7738710EF51350A]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: E73E3DA9AFBB160E]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: CB05E52F01F539B0]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: D4059E3D0A877EA4]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 3C8F7BB37CE99C65]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 43DF3645718DA9D5]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: D9D802A90920E805]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 3ED2737676D6557F]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: DEEB553C2C961575]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: DAE0BAD1EA22B0EA]
[      clap::parse::validator] 	Validator::validate_exclusive:iter:[hash: 4249AAFB8024F360]
[      clap::parse::validator] 	Validator::validate_required: required=ChildGraph([])
[      clap::parse::validator] 	Validator::gather_requirements
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 56CDDC4DCF7620D1]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: FA4C0A49E4E824EB]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 384A5AE250BC4560]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 5DD1AA0AA564F4FE]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 9B3E8EF39C82E397]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 5599B14D59DA9C11]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 1871FF364E95D4A7]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 5E0D24BBCDB34988]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: A70BBAD462A5D1B2]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 137B4D7D7D1881A1]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: DBA64F5F3C7FBF57]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: E77494CD87F1D2DA]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 14DA47432C8CFA60]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 58E5E5E2FBEF5F00]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 13CED1EB3B3A0007]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 82F533ECABFA714B]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 9D90C893B76CB18B]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 9FC6FE4D36EF8BF9]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: B38D31D507F3E90A]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 18DD752B05658C3F]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: DFA80C8E158D7D7F]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 546A0AABF585531B]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 41D2EAB3710EDBCF]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 15CB59E2C5B60FAB]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: BD41D6EAA992C433]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 8FB24FA5E22D8B07]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 5624D3D8642F6C26]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 7D9D82C06A7A0180]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 43EF7A488FEFC82A]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 181D448BDFC5D338]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 177F1774C56C3442]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 73D3CD19BFF4DC1B]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 12E83F165DADB6A]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: F4ABA8A495C126A4]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 1178E72A4D87EF48]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 5BE7A142A6D66B90]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 4D2E6278BEC5F49B]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 49E296EF7CB9631A]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: B78D2CADB774D71D]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 1A356B5CE9B72F46]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 80925DC4BC9352A9]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 5A69F0A80EA7DB13]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: ECC7DC1B3D93347C]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 5D0570B9ADC7168]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: C585956EEB195C27]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 98090FB09CDF4FE8]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 9B3DF14708B1895]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: D1D1DAD3D2E91286]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 7738710EF51350A]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: E73E3DA9AFBB160E]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: CB05E52F01F539B0]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: D4059E3D0A877EA4]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 3C8F7BB37CE99C65]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 43DF3645718DA9D5]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: D9D802A90920E805]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 3ED2737676D6557F]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: DEEB553C2C961575]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: DAE0BAD1EA22B0EA]
[      clap::parse::validator] 	Validator::gather_requirements:iter:[hash: 4249AAFB8024F360]
[      clap::parse::validator] 	Validator::validate_required_unless
[      clap::parse::validator] 	Validator::validate_matched_args
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 56CDDC4DCF7620D1]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "{timestamp:<15} {author:<15.14} {commit:<8}",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="blame-format"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "blame-format"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: FA4C0A49E4E824EB]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "│",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="blame-separator"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "blame-separator"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 384A5AE250BC4560]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "%Y-%m-%d %H:%M:%S %z",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="blame-timestamp-format"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "blame-timestamp-format"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 5DD1AA0AA564F4FE]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="commit-decoration-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "commit-decoration-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 9B3E8EF39C82E397]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "^commit ",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="commit-regex"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "commit-regex"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 5599B14D59DA9C11]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "raw",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="commit-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "commit-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 1871FF364E95D4A7]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "48",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="diff-stat-align-width"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "diff-stat-align-width"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 5E0D24BBCDB34988]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "added:",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="file-added-label"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "file-added-label"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: A70BBAD462A5D1B2]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "copied:",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="file-copied-label"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "file-copied-label"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 137B4D7D7D1881A1]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "blue ul",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="file-decoration-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "file-decoration-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: DBA64F5F3C7FBF57]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="file-modified-label"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "file-modified-label"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: E77494CD87F1D2DA]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "removed:",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="file-removed-label"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "file-removed-label"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 14DA47432C8CFA60]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "renamed:",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="file-renamed-label"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "file-renamed-label"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 58E5E5E2FBEF5F00]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "blue",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="file-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "file-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 13CED1EB3B3A0007]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            ":",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="grep-separator-symbol"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "grep-separator-symbol"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 82F533ECABFA714B]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "blue box",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="hunk-header-decoration-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "hunk-header-decoration-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 9D90C893B76CB18B]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "blue",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="hunk-header-file-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "hunk-header-file-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 9FC6FE4D36EF8BF9]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "blue",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="hunk-header-line-number-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "hunk-header-line-number-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: B38D31D507F3E90A]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "line-number syntax",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="hunk-header-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "hunk-header-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 18DD752B05658C3F]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="hunk-label"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "hunk-label"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: DFA80C8E158D7D7F]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "file://{path}",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="hyperlinks-file-link-format"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "hyperlinks-file-link-format"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 546A0AABF585531B]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "blue",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="inline-hint-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "inline-hint-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 41D2EAB3710EDBCF]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "true",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="inspect-raw-lines"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "inspect-raw-lines"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 15CB59E2C5B60FAB]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "32",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="line-buffer-size"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "line-buffer-size"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: BD41D6EAA992C433]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "{nm:^4}⋮",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="line-numbers-left-format"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "line-numbers-left-format"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 8FB24FA5E22D8B07]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="line-numbers-left-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "line-numbers-left-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 5624D3D8642F6C26]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="line-numbers-minus-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "line-numbers-minus-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 7D9D82C06A7A0180]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="line-numbers-plus-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "line-numbers-plus-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 43EF7A488FEFC82A]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "{np:^4}│",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="line-numbers-right-format"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "line-numbers-right-format"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 181D448BDFC5D338]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="line-numbers-right-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "line-numbers-right-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 177F1774C56C3442]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="line-numbers-zero-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "line-numbers-zero-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 73D3CD19BFF4DC1B]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "0.6",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="max-line-distance"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "max-line-distance"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 12E83F165DADB6A]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "512",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="max-line-length"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "max-line-length"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: F4ABA8A495C126A4]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "▼",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="merge-conflict-begin-symbol"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "merge-conflict-begin-symbol"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 1178E72A4D87EF48]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "▲",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="merge-conflict-end-symbol"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "merge-conflict-end-symbol"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 5BE7A142A6D66B90]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "box",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="merge-conflict-ours-diff-header-decoration-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "merge-conflict-ours-diff-header-decoration-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 4D2E6278BEC5F49B]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "normal",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="merge-conflict-ours-diff-header-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "merge-conflict-ours-diff-header-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 49E296EF7CB9631A]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "box",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="merge-conflict-theirs-diff-header-decoration-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "merge-conflict-theirs-diff-header-decoration-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: B78D2CADB774D71D]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "normal",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="merge-conflict-theirs-diff-header-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "merge-conflict-theirs-diff-header-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 1A356B5CE9B72F46]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "normal auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="minus-empty-line-marker-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "minus-empty-line-marker-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 80925DC4BC9352A9]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "normal auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="minus-emph-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "minus-emph-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 5A69F0A80EA7DB13]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "minus-style",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="minus-non-emph-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "minus-non-emph-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: ECC7DC1B3D93347C]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "normal auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="minus-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "minus-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 5D0570B9ADC7168]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="paging-mode"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "paging-mode"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: C585956EEB195C27]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "syntax auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="plus-emph-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "plus-emph-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 98090FB09CDF4FE8]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "normal auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="plus-empty-line-marker-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "plus-empty-line-marker-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 9B3DF14708B1895]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "plus-style",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="plus-non-emph-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "plus-non-emph-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: D1D1DAD3D2E91286]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "syntax auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="plus-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "plus-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 7738710EF51350A]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "⟶  ",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="right-arrow"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "right-arrow"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: E73E3DA9AFBB160E]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "4",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="tab-width"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "tab-width"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: CB05E52F01F539B0]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="true-color"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "true-color"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: D4059E3D0A877EA4]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "auto auto",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="whitespace-error-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "whitespace-error-style"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 3C8F7BB37CE99C65]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "\\w+",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="tokenization-regex"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "tokenization-regex"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 43DF3645718DA9D5]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "↵",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="wrap-left-symbol"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "wrap-left-symbol"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: D9D802A90920E805]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "2",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="wrap-max-lines"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "wrap-max-lines"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 3ED2737676D6557F]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "37.0",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="wrap-right-percent"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "wrap-right-percent"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: DEEB553C2C961575]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "…",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="wrap-right-prefix-symbol"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "wrap-right-prefix-symbol"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: DAE0BAD1EA22B0EA]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "↴",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="wrap-right-symbol"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "wrap-right-symbol"=0
[      clap::parse::validator] 	Validator::validate_matched_args:iter:[hash: 4249AAFB8024F360]: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "syntax normal",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="zero-style"
[      clap::parse::validator] 	Validator::validate_arg_values: checking validator...
[      clap::parse::validator] 	good
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "zero-style"=0
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[[hash: 59636393CFFBFE5F], [hash: 30FF0B7C4D079478]]
```

---

_Label `C-bug` added by @dandavison on 2022-01-16 22:19_

---

_Comment by @dandavison on 2022-01-16 22:30_

Please note that I have updated my original submission above so that it now contains the clap debug output.

---

_Referenced in [dandavison/delta#915](../../dandavison/delta/pulls/915.md) on 2022-01-17 22:07_

---

_Comment by @epage on 2022-01-18 03:07_

Which behavior did you have with clap2?

What is happening is we have logic to guess when we overflow and should switch to next-line-help for more space.  It seems to bail out if the argument column (`--merge-conflict-theirs-diff-header-style <STYLE_STRING>`) is larger than the terminal (what is happening before your change).  With your change, we are no longer bailing out and clap is thinking it will fit best if we move everything to the following line.

Various forms of this logic have dated back to at least 2017 (so before clap3 though clap3 made slight tweaks around it) and I've still not seen any code reference why we have that bail out or what all use cases we are concerned about for why we are not exclusively staying to one line (ie why we are out guessing the developer).

---

_Label `A-help` added by @epage on 2022-01-18 03:07_

---

_Label `S-triage` added by @epage on 2022-01-18 03:07_

---

_Comment by @dandavison on 2022-01-18 14:33_

Hi @epage, thanks for the reply!

> Which behavior did you have with clap2?

Delta has just [migrated](https://github.com/dandavison/delta/pull/889) from StructOpt to clap v3, retaining the derive macro / StructOpt style.

I think my core request is this: is it possible to make it so that the `-h` output will always be formatted with one line per option, regardless of terminal width etc?

delta has 100 command line options. With one-line-per-option the `-h` output is 111 lines, but with multi-line it is 321. So I'm really hoping to be able to give users the "concise" (!) 111 line version with `-h`. I'm aware that other clap apps such as bat and ripgrep achieve this (but with clap2, and not using the derive macro StructOpt style).

> bail out if the argument column (`--merge-conflict-theirs-diff-header-style <STYLE_STRING>`) is larger than the terminal 

I don't think I yet understand the dependence on terminal size that you're referring to -- I have not yet been able to make clap switch between the two formats by changing my terminal width or font size (i.e. causing the output of `echo $COLUMNS` and `tput cols` to vary). But I might be being slow here! So this is tangential -- my main question is the one above.

---

_Comment by @dandavison on 2022-01-18 14:37_

Incidentally, the migration to clap has definitely been an improvement to delta, for example because it allowed us to use `after_long_help` thus keeping the `-h` output minimal. And it was very easy to switch, so thanks to StructOpt and Clap developers for combining them so well -- this is the only issue I've encountered.

---

_Comment by @epage on 2022-01-18 16:23_

> I think my core request is this: is it possible to make it so that the -h output will always be formatted with one line per option, regardless of terminal width etc?
>
> delta has 100 command line options. With one-line-per-option the -h output is 111 lines, but with multi-line it is 321. So I'm really hoping to be able to give users the "concise" (!) 111 line version with -h. I'm aware that other clap apps such as bat and ripgrep achieve this (but with clap2, and not using the derive macro StructOpt style).

We only auto-detect terminal size when the `wrap_help` feature flag is enabled (seems like an odd name).  Otherwise, we rely on the configured [App::terminal_width](https://docs.rs/clap/latest/clap/struct.App.html#method.term_width), defaulting to 100 if unset.

You could set `App::terminal._width(0)` which means "no wrap" though that will also impact `--help`.

In the future, we are looking at opening up our help generation so users can get greater control (https://github.com/clap-rs/clap/issues/2914).

We can leave this issue open though for brainstorming how to improve things in this specific case.  I am hesitant to add yet another setting to toggle behavior as API bloat is a current problem with clap.  I am more open to ideas that would generally improve the existing behavior or if there are solid enough justifications for dramatic changes.  With that said, one thing we can explore is changing from a bool (next line or auto) to a enum (next line, no next line, auto).

---

_Label `S-triage` removed by @epage on 2022-01-18 16:23_

---

_Label `S-waiting-on-design` added by @epage on 2022-01-18 16:23_

---

_Referenced in [clap-rs/clap#4190](../../clap-rs/clap/pulls/4190.md) on 2022-09-07 19:03_

---

_Referenced in [clap-rs/clap#4409](../../clap-rs/clap/issues/4409.md) on 2022-10-20 16:32_

---

_Comment by @bgilbert on 2022-11-05 07:42_

I was pointed to this issue from #4409.  I have an application where we want help to wrap at 80 characters (since that's the width of a terminal :stuck_out_tongue:) but would like to keep the compact single-line format for short help.  This has required us to aggressively limit the lengths of short help strings (we have a CI check for it) because if any one option overruns its line, the entire section gets converted to multi-line format.  For some options it'd be nice to have room for a few more words without bloating the rest of the help.

I'd love for clap to be able to wrap individual help texts to a second line _within their column_.  This would allow longer texts to degrade gracefully.  So, instead of this:

```
Options:
      --process-integer <INTEGER>
          Process an integer with really complex semantics
      --process-string <STRING>
          Process a string
  -h, --help
          Print help information
```

we'd get this:

```
Options:
      --process-integer <INTEGER>  Process an integer with really complex
                                   semantics
      --process-string <STRING>    Process a string
  -h, --help                       Print help information
```

Presumably that'd need to be configurable, and I understand the hesitation to add more options.  I think it'd really improve readability, though.

---

_Comment by @epage on 2022-11-07 16:14_

> I'd love for clap to be able to wrap individual help texts to a second line within their column. This would allow longer texts to degrade gracefully. So, instead of this:

clap does that today.

From `cargo check` on my machine:
```
    -Z <FLAG>                       Unstable (nightly-only) flags to Cargo, see 'cargo -Z help' for
                                    details
```

The problem, and what this issue is about, is our heuristic for switching to next line help.
```rust
            // force_next_line
            let h = arg.get_help().unwrap_or_default();
            let h_w = h.display_width() + display_width(spec_vals);
            let taken = longest + 12;
            self.term_w >= taken
                && (taken as f32 / self.term_w as f32) > 0.40
                && h_w > (self.term_w - taken)
```
- Don't bother switching to next-line if the longest argument (without help) takes up more than the term width
- **If the longest argument (without help) takes up more than 40% of the terminal width, switch to next line**
- And if we can't fit on one line

The 40% check is the one that prevents us from always switching to next-line help.    I assume the intent is to ensure that the second column is not too small to be very readable.  @bgilbert any thoughts on how to balance avoiding next_line_help with the second column being too narrow?

I am noticing a magic number in here.  That was most likely missed when we changed the tab width and is likely too large.

---

_Referenced in [clap-rs/clap#4463](../../clap-rs/clap/pulls/4463.md) on 2022-11-07 16:37_

---

_Comment by @bgilbert on 2022-11-12 22:26_

Oof, okay.  To my mind, next-line help is a last resort, so I'd probably set the limit around 60%.  On an 80-column screen that leaves 32 columns, which is a bit narrow but still readable.  But, 55% or even 50% would still be an improvement.

I'd also be completely happy with an option to disable (or, I suppose, force) next-line help.

---

_Comment by @epage on 2022-11-13 04:26_

This got me curious about the origin of auto-next-line-help
- #637 originally added it to resolve #597
  - The main concern was one or two excessively long arguments would cause a lot of whitespace that could make things hard to read.
- #648 changed it from 25% to 40% (but discussed in #597?)
- #2174 changed it to apply to all args to resolve #2170
  - I'm not fully convinced it should be consistent.  I do think the inconsistent blank lines is a problem but we no longer show blank lines after `next_line_help` for short-help
  - Also, `Arg::next_line_help(true)` will force the entire section to use `next_line_help` (without `help_heading`, this basically means all args now that unified help is used).  This seems like a bug and it goes against the  [documentation](https://docs.rs/clap/latest/clap/builder/struct.Arg.html#method.next_line_help)

In general, I've been wondering if absolute weighting might be better than relative (if the arg is too many characters or the about has too few characters).  This then becomes a fairly easy calculation that callers can do to pass in `Arg::next_line_help` or `Command::next_line_help` and get the behavior they want.  Moving this to callers would allow clap to shrink a little bit.  The question is how much and if this automation pulls its weight enough to be worth it.

So a part of me leans towards removing all of these calculations (and making `Arg::next_line_help` only apply to the one arg) but I would be interested in hearing your thoughts @joshtriplett as the original filer.

---

_Referenced in [clap-rs/clap#4550](../../clap-rs/clap/issues/4550.md) on 2022-12-13 06:33_

---

_Referenced in [clap-rs/clap#4631](../../clap-rs/clap/issues/4631.md) on 2023-01-12 14:50_

---

_Label `M-minor-incompat` added by @epage on 2023-07-17 13:14_

---
