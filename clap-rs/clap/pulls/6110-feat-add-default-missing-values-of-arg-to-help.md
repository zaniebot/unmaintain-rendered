---
number: 6110
title: "feat: Add default missing values of `Arg` to help message"
type: pull_request
state: open
author: tvercruyssen
labels: []
assignees: []
draft: true
base: master
head: default-missing-vals-help
created_at: 2025-08-23T23:41:50Z
updated_at: 2025-08-25T14:36:48Z
url: https://github.com/clap-rs/clap/pull/6110
synced_at: 2026-01-10T01:28:27Z
---

# feat: Add default missing values of `Arg` to help message

---

_Pull request opened by @tvercruyssen on 2025-08-23 23:41_

Fixes #6109 

This is very much copy-pasted from the implementation of `possible_values` just above it... 

I'm not sure whether there is a need to add another test case. As is quite a lot of them already have a `default_missing_value`. Hence the large list of failing tests (since the expected and found help output no longer match):
```
failures:
    app_settings::arg_required_else_help_error_message
    arg_aliases::invisible_arg_aliases_help_output
    arg_aliases::visible_arg_aliases_help_output
    arg_aliases_short::invisible_short_arg_aliases_help_output
    arg_aliases_short::visible_short_arg_aliases_help_output
    derive_order::derive_order
    derive_order::derive_order_next_order
    derive_order::derive_order_no_next_order
    derive_order::derive_order_subcommand_propagate
    derive_order::derive_order_subcommand_propagate_with_explicit_display_order
    derive_order::no_derive_order
    double_require::help_text
    help::args_negate_sc
    help::args_with_last_usage
    help::complex_help_output
    help::custom_headers_headers
    help::hide_args
    help::issue_1046_hide_scs
    help::issue_1364_no_short_options
    help::issue_1642_long_help_spacing
    help::issue_1794_usage
    help::mixed_argument_types
    help::mixed_argument_types_no_short
    help::mixed_argument_types_short_positional
    help::multi_level_sc_help
    help::multiple_custom_help_headers
    help::old_newline_chars
    help::old_newline_variables
    help::option_usage_order
    hidden_args::hide_args
    hidden_args::hide_long_args
    hidden_args::hide_long_args_short_help
    hidden_args::hide_short_args
    hidden_args::hide_short_args_long_help
```
Potentially if a option is added to enable/disable this feature, it can be disabled for all of those tests to make them pass as is.

Marked as draft since I don't yet know what to do for testing until it is decided what the feature should look like. However, I wanted to see how difficult it would be to implement. Hence, this PR is basically a stepping stone for implementing the final feature.

---
