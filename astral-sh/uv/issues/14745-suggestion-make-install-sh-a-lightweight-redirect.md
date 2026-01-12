```yaml
number: 14745
title: "Suggestion: Make `install.sh` a lightweight redirect to GitHub-hosted installer script (like `pyenv`)"
type: issue
state: closed
author: tataouinea
labels:
  - enhancement
assignees: []
created_at: 2025-07-19T12:43:42Z
updated_at: 2025-07-20T17:13:13Z
url: https://github.com/astral-sh/uv/issues/14745
synced_at: 2026-01-12T16:01:55Z
```

# Suggestion: Make `install.sh` a lightweight redirect to GitHub-hosted installer script (like `pyenv`)

---

_@tataouinea_

### Summary

The current `install.sh` script available at:

```
curl -LsSf https://astral.sh/uv/install.sh | sh
```

is long and complex, making it difficult to quickly audit for safety. This is important because users are encouraged to pipe it directly into `sh`.

I'd like to propose changing this script to follow a model like [pyenv's installer](https://github.com/pyenv/pyenv-installer), where the public-facing script is short, readable, and simply redirects to a GitHub-hosted file that contains the real logic.

### Motivation

Piping shell scripts from external domains is risky. While it's always best to manually review scripts, users rarely do this. With the current approach, https://astral.sh/uv/install.sh is quite lengthy and not easily auditable at a glance.

By contrast, tools like [pyenv](https://github.com/pyenv/pyenv-installer) use an installation entrypoint like:

```
#!/bin/bash
#
# Usage: curl https://pyenv.run | bash
#
# For more info, visit: https://github.com/pyenv/pyenv-installer
#
index_main() {
    set -e
    curl -s -S -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash
}

index_main
```

This has two key benefits:

1. **Transparency:** Users can immediately understand what the script is doing.
2. **Trust:** The actual installer lives on GitHub, where users can:
    - Verify the script's contents easily.
    - Inspect the Git log and blame history.
    - Trust GitHub as a more secure host than a custom domain.

This minimizes the blast radius in the unlikely event that astral.sh is compromised, while still preserving the convenience of a memorable and friendly installation URL.

### Proposal

- Keep https://astral.sh/uv/install.sh for convenience.
- Replace its contents with a short shell script that transparently downloads and executes a GitHub-hosted script, e.g., from the main branch or a tag on this repository.

Example sketch:

```
#!/bin/bash
#
# Usage: curl -LsSf https://astral.sh/uv/install.sh | sh
#
# For more info, visit: https://github.com/astral-sh/uv
#
curl -sSfL https://raw.githubusercontent.com/astral-sh/uv/main/install/install.sh | sh
```

This would keep the current UX intact, while improving user trust and security.

### Alternatives

- Keep the current setup but add a clear comment block at the top explaining where the source of truth is hosted and how to audit it.
- Host `install.sh` directly on GitHub Pages (e.g., astral-sh.github.io) to avoid the risks of self-managed domains.

### Additional Context

This change is based on good practices around supply chain trust, especially when it comes to "curl | sh" style install flows. By pointing to a trusted GitHub file, you're helping users more safely bootstrap `uv` into their systems.

Let me know if you'd like help drafting the new short redirect script.

Thanks for all the work on `uv` — it’s been a joy to use!

---

_Label `enhancement` added by @tataouinea on 2025-07-19 12:43_

---

_Comment by @FishAlchemist on 2025-07-19 16:23_

Actually, the script is on GitHub; it just lacks Git history, but the hash is still there.

<img width="1185" height="90" alt="Image" src="https://github.com/user-attachments/assets/4d868981-3c32-41dd-969b-976f8a3ff0c5" />

<img width="899" height="285" alt="Image" src="https://github.com/user-attachments/assets/ff6a8b12-8b23-4d20-917b-00a129d9277f" />

<img width="1250" height="411" alt="Image" src="https://github.com/user-attachments/assets/29fb0a1f-985d-413f-acae-80644952a4a8" />

Or are you asking for the change history?

---

_Comment by @zanieb on 2025-07-20 16:56_

The URL at `astral.sh` is just a redirect to `https://github.com/astral-sh/uv/releases/latest/download/uv-installer.sh`, I'm not sure how making it a shim makes it easier to audit? If anything, that means it requires a separate invocation to inspect the relevant code?

---

_Comment by @tataouinea on 2025-07-20 17:08_

> The URL at `astral.sh` is just a redirect to `https://github.com/astral-sh/uv/releases/latest/download/uv-installer.sh`, I'm not sure how making it a shim makes it easier to audit? If anything, that means it requires a separate invocation to inspect the relevant code?

Ah, you're absolutely right — I hadn’t noticed that https://astral.sh/uv/install.sh is just a redirect to GitHub. A quick `curl -I https://astral.sh/uv/install.sh` confirms it's a 301 to https://github.com/astral-sh/uv/releases/latest/download/uv-installer.sh.

That completely addresses my original concern — since the actual script is served from GitHub, it’s already benefiting from the trust and transparency I was advocating for. No need for a shim or change in structure.

Thanks for the clarification, and sorry for the noise!

---

_Closed by @tataouinea on 2025-07-20 17:08_

---

_Comment by @zanieb on 2025-07-20 17:13_

No problem!

---
