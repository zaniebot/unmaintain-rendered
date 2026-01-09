---
number: 5853
title: POC of Approach to Supporting Internationalization
type: pull_request
state: open
author: toadslop
labels: []
assignees: []
draft: true
base: master
head: i18n-refactor-2
created_at: 2024-12-22T05:58:34Z
updated_at: 2025-05-31T13:51:33Z
url: https://github.com/clap-rs/clap/pull/5853
synced_at: 2026-01-07T13:12:20-06:00
---

# POC of Approach to Supporting Internationalization

---

_Pull request opened by @toadslop on 2024-12-22 05:58_

# Support for Internationalization

Issue: https://github.com/clap-rs/clap/issues/380

Provides a POC demonstrating a method for adding i18n support to Clap in a minimally invasive way. This POC is not a complete solution and only aims to illustrate the approach for evaluation of the project's maintainers. Additional work will be needed to produce a production-ready solution.

## General Notes

For context, this approach is _not_ an internationlization solution. Rather, it is a solution which extracts the hardcoded English text of the library into external configuration files, and then offers an API for consumers to replace the texts with their own. It does not care about the content of the texts, giving the consumer the freedom to substitute content for any reason and under any conditions they choose; only one such use of this is for internationalization.

## Trying It Out

To get a quick look at the results, you can run the accompanying example. You'll note that the alignment of the of the columns is a bit off -- this is a consequence of the current POC implementation. If the general approach is accepted, then I will consider how to resolve this.

```bash
gh pr checkout <TODO>

cargo run --example i18n --features derive -- --help
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.07s
     Running `target/debug/examples/i18n --help`
A simple to use, efficient, and full-featured Command Line Argument Parser

Usage: i18n [OPTIONS] [NAME] [COMMAND]

Commands:
  test  Does testing things
  help  Print this message or the help of the given subcommand(s)

Arguments:
  [NAME]  Please enter your name

Options:
  -c, --config <FILE>  Enter the path to a configuration file
  -d, --debug...         Execute in debug mode
  -h, --help             Print help
  -V, --version          Print version

LOCALE=jp cargo run --example i18n --features derive -- --help
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.08s
     Running `target/debug/examples/i18n --help`
A simple to use, efficient, and full-featured Command Line Argument Parser

使用方法: i18n [オプション] [名前] [コマンド]

コマンド:
  test  テストを実行します
  help  このメッセージまたは指定されたサブコマンドのヘルプを表示します

引数:
  [名前]  名前を入力してください

オプション:
  -c, --config <ファイル>  設定ファイルのパスを入力してください
  -d, --debug...         デバッグモードで実行します
  -h, --help             ヘルプを表示
  -V, --version          バージョンを表示
```

## Approach

The main design goal of this implementation is to be minimally invasive. By minimally invasive, I mean that I do not want to significantly impact how Clap already works.

### Current Behavior

Clap currently produces output in the following stages:

1. load a layout template
2. interpolate hardcoded text and user provided text into the template while applying styling
3. print output to terminal (or otherwise, make the output available to the consumer)

This implementation changes step 2 and adds a new step between it and step 3.

1. load a layout template
2. interpolate hardcoded keys and user-provided keys into the layout template, producing a new template file which is styled but does not include text -- instead it includes the text-keys surrounded by template syntax (current something lie `{clap.default-help-text}`
3. run a pass over the template from step two to interpolate the actual texts
4. print output to terminal (or otherwise, make the output available to the consumer)

Note that the current implementation tends to break column alignments since the widths of the text keys are often different from the actual text, and it appears that Step 2 aligns the columes based on the input text. Some consideration will be needed to determine how best to resolve this issue but this can be deferred until I get the go ahead from the Clap team.

---

_@toadslop reviewed on 2024-12-22 05:59_

---

_Review comment by @toadslop on `Cargo.toml`:189 on 2024-12-22 05:59_

Just adding a new example to demonstrate, and some dev-deps to support the example.

---

_Review comment by @toadslop on `clap_bench/benches/complex.rs`:2 on 2024-12-22 06:01_

You'll see a lot of cases of `text_provider::DEFAULT_TEXT_PROVIDER` being added throughout this PR. This is just to resolve compile issues. The final implementation would ideally be structured in a way to avoid needing to make these changes; however, to keep the POC simple I opted to not resolve that issue until later.

---

_@toadslop reviewed on 2024-12-22 06:01_

---

_@toadslop reviewed on 2024-12-22 06:02_

---

_Review comment by @toadslop on `clap_builder/Cargo.toml`:69 on 2024-12-22 06:02_

New depts. serde_yml is for deserializing the external config file where the hardcoded texts are extracted to.

---

_@toadslop reviewed on 2024-12-22 06:03_

---

_Review comment by @toadslop on `clap_builder/src/builder/command.rs`:4736 on 2024-12-22 06:03_

This is an example of one case where we have removed hardcoded English text and replaced it with a template string containing a text lookup key.

---

_@toadslop reviewed on 2024-12-22 06:05_

---

_Review comment by @toadslop on `clap_builder/src/builder/styled_str.rs`:33 on 2024-12-22 06:05_

This new interpolate function replaces the text lookup keys with the actual texts. There is probably a better place to put this, but for the purposes of demonstrating the approach, I thought this was good enough.

---

_@toadslop reviewed on 2024-12-22 06:07_

---

_Review comment by @toadslop on `clap_builder/src/derive.rs`:31 on 2024-12-22 06:07_

New parse_with_texts is the API which allows a consumer to substitute the OOTB English texts with their own texts. This could perhaps be adjusted to allow a reference to the texts to be added to the Parser struct itself so that we don't need to pass them down through many layers of function calls as we do here. This change might have required making changes to clap_derive in order to get a minimally working example ready, so I decided against it.

---

_@toadslop reviewed on 2024-12-22 06:08_

---

_Review comment by @toadslop on `clap_builder/src/output/help_template.rs`:200 on 2024-12-22 06:08_

More hardcoded texts replaced with text lookup keys.

---

_@toadslop reviewed on 2024-12-22 06:09_

---

_Review comment by @toadslop on `clap_builder/src/text_provider.rs`:1 on 2024-12-22 06:09_

Simple implementation of what the API for allowing consumers to pass in their own texts might look like.

---

_@toadslop reviewed on 2024-12-22 06:11_

---

_Review comment by @toadslop on `clap_builder/src/util/template.rs`:1 on 2024-12-22 06:11_

Simple template engine for interpolating the actual texts into the templates using the text lookup keys. Hand-rolled this to avoid pulling in a whole templating engine, which would include many more features that what we actually need for this case.

Some improvements to this might be to allow escaping `{`. Also would need some tests since this hastilly written example might have a few bugs lurking in it. But again, this is just a minimal example for the POC.

---

_@toadslop reviewed on 2024-12-22 06:12_

---

_Review comment by @toadslop on `clap_builder/texts/en.yaml`:1 on 2024-12-22 06:12_

These are the hard-coded default texts that I extracted. You'll note that there are many more texts than these -- I just extracted enough to demonstrate using the example that I included in this PR.

---

_@toadslop reviewed on 2024-12-22 06:15_

---

_Review comment by @toadslop on `examples/i18n/main.rs`:10 on 2024-12-22 06:15_

For this example, we use the rust-i18n crate to demonstrate how we can use the TextProvider API to achieve internationalization. I chose this crate because it allows looking translations at runtime, which is probably what most people would want since it avoids packaging languages into the binary that the end user will never use.

---

_Review comment by @toadslop on `examples/i18n/main.rs`:45 on 2024-12-22 06:16_

Notice that we need to provide a template string with the text lookup key inside in order to have Clap interpolate the text. This may not be ideal, but it was the simplest implementation as it didn't require making any changes to clap_derive.

---

_@toadslop reviewed on 2024-12-22 06:16_

---

_@toadslop reviewed on 2024-12-22 06:17_

---

_Review comment by @toadslop on `examples/i18n/main.rs`:80 on 2024-12-22 06:17_

Allow the user to change the locale using an environment variable.

---

_@toadslop reviewed on 2024-12-22 06:18_

---

_Review comment by @toadslop on `examples/i18n/main.rs`:85 on 2024-12-22 06:18_

The user would be free to continue using rust-i18n to internationalize this and the following printlines as well.

---

_@toadslop reviewed on 2024-12-22 06:20_

---

_Review comment by @toadslop on `tests/builder/app_settings.rs`:139 on 2024-12-22 06:20_

This test shows a weakness of the current simple implementation -- the end user is able to extract the text from the error message without calling the print method, which is where I currently carry out interpolation. Will need to find a more ideal place to put that logic so that users can never extract text from Clap that hasn't been interpolated.

---
