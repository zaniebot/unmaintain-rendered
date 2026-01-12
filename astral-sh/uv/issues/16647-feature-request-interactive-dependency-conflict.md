```yaml
number: 16647
title: "Feature Request: Interactive Dependency Conflict Resolution Assistant"
type: issue
state: open
author: cherchyk
labels:
  - enhancement
assignees: []
created_at: 2025-11-09T02:52:44Z
updated_at: 2025-11-10T14:38:17Z
url: https://github.com/astral-sh/uv/issues/16647
synced_at: 2026-01-12T16:02:35Z
```

# Feature Request: Interactive Dependency Conflict Resolution Assistant

---

_@cherchyk_

# Feature Request: Interactive Dependency Conflict Resolution Assistant

## Summary

Add an interactive conflict resolution assistant that helps developers understand and resolve dependency conflicts when `uv lock` or `uv add` fails, leveraging uv's speed to explore the solution space and suggest concrete fixes.

## Motivation

### The Problem

Dependency resolution failures are one of the most frustrating experiences in Python development:

```bash
$ uv add django==5.0
  × No solution found when resolving dependencies:
  ╰─▶ Because myproject depends on django==5.0 and django==5.0 depends on sqlparse>=0.3.1,
      we can conclude that myproject depends on sqlparse>=0.3.1.
      And because myproject depends on some-legacy-package==1.0 and some-legacy-package==1.0
      depends on sqlparse<0.3, we can conclude that myproject's requirements are unsatisfiable.
```

**Current developer experience:**
1. Read cryptic error messages
2. Manually investigate which package versions are compatible
3. Trial-and-error: try different versions, run `uv lock` again, repeat
4. Give up and either pin old versions or abandon the upgrade
5. Time wasted: 15 minutes to 2+ hours

**Why this is a critical pain point:**
- **Common**: Happens during every major dependency upgrade
- **Frustrating**: Requires deep understanding of dependency trees
- **Time-consuming**: Manual exploration of version compatibility
- **Error-prone**: Easy to make suboptimal choices
- **Blocks innovation**: Developers avoid upgrades to prevent conflicts

### Why Current Solutions Fall Short

1. **pip**: Gives up immediately with unclear error messages
2. **poetry**: Shows conflict but no suggestions for resolution
3. **Manual tools** (pipdeptree, etc.): Require separate analysis
4. **Stack Overflow**: Generic advice, not specific to your dependency tree

## Proposed Solution

### Core Feature: Interactive Conflict Resolution

When dependency resolution fails, automatically analyze the conflict and present actionable solutions:

```bash
$ uv add django==5.0

  × Dependency conflict detected

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📦 Conflict Analysis
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Root cause: Incompatible sqlparse version requirements

  django 5.0 requires sqlparse >=0.3.1
       ↓
  ⚠️  CONFLICT
       ↓
  legacy-parser 1.0 requires sqlparse <0.3

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💡 3 Solutions Found (analyzed in 47ms)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. Downgrade Django (Recommended)
   uv add django>=4.2,<5.0

   ✓ Keeps all existing dependencies
   ✓ django 4.2 is LTS (supported until April 2026)
   ✓ Compatible with sqlparse 0.2.4

2. Upgrade legacy-parser
   uv add legacy-parser>=2.0

   ⚠ legacy-parser 2.0 released 6 months ago
   ⚠ Breaking changes in API (see changelog)
   ✓ Compatible with sqlparse 0.3.1+

3. Remove legacy-parser (if unused)
   uv remove legacy-parser

   ⚠ Used by: scripts/data_processor.py
   💡 Alternative: Use built-in ast module

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Apply solution? [1-3, m for more details, q to quit]: _
```

### Key Capabilities

1. **Instant conflict analysis** - Leverage uv's speed to explore solution space in <100ms
2. **Root cause identification** - Show exactly which packages are in conflict and why
3. **Multiple solution paths** - Present 3-5 concrete solutions ranked by recommendation
4. **Impact analysis** - Show what changes with each solution
5. **Interactive application** - Apply fixes with a single keypress
6. **Learning mode** - Explain why conflicts happen (optional `--explain` flag)

### User Interaction Flow

#### Basic Mode (Default)
```bash
$ uv add package-with-conflict

  × Conflict detected

💡 Quick fix available:
   Downgrade package-a from 3.0 to 2.8

Apply? [y/n/m for more options]: m

[Shows full interactive menu with 3-5 solutions]

Choose [1-5] or 'a' to analyze impact: 2

📊 Impact Analysis for Solution 2:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Changes:
  ⬆ package-a: 2.5 → 3.0
  ⬆ shared-dep: 1.0 → 2.1 (required by package-a 3.0)
  ⬇ legacy-tool: 5.0 → 4.8 (conflicts with shared-dep 2.1)

Risk assessment: LOW
  ✓ All packages actively maintained
  ⚠ legacy-tool downgrade (2 versions back)

Apply this solution? [y/n]: y

✓ Resolved! Lock file updated.
```

#### Expert Mode
```bash
$ uv add package --resolve-conflicts --explain

[Shows detailed PubGrub algorithm steps]
[Explains version incompatibilities]
[Shows full dependency tree with conflicts highlighted]
```

#### Automated Mode (for CI)
```bash
$ uv add package --resolve-conflicts=auto --prefer=minimal-changes

✓ Auto-resolved using minimal-changes strategy
  Changed: package-a 3.0 → 2.8

⚠ Review changes in uv.lock before committing
```

## Technical Approach

### How It Works

1. **Detect conflict** - PubGrub resolver fails with incompatibility report
2. **Analyze solution space** - Use uv's resolver to test alternative version combinations
3. **Rank solutions** - Score based on:
   - Number of packages changed
   - Recency of versions (prefer newer when safe)
   - Severity of changes (major vs minor downgrades)
   - Package maintenance status
   - Community adoption metrics
4. **Present interactively** - Show top 3-5 solutions with clear tradeoffs
5. **Apply solution** - Update `pyproject.toml` and/or `uv.lock`

### Implementation Strategy

1. **Extend existing resolver** - Build on uv's PubGrub implementation
2. **Solution exploration** - When resolution fails:
   - Identify conflicting requirements
   - Generate alternative version constraints
   - Re-run resolver for each alternative
   - Cache results for performance
3. **Ranking algorithm**:
   ```rust
   fn score_solution(solution: &Solution) -> u32 {
       let mut score = 100;
       score -= solution.major_downgrades * 30;
       score -= solution.minor_downgrades * 10;
       score -= solution.packages_changed * 5;
       score += solution.uses_latest_versions * 10;
       score
   }
   ```
4. **Interactive UI** - Use existing `uv-console` for rich terminal output
5. **CLI integration** - New flags:
   - `--resolve-conflicts` (auto-enable on failure)
   - `--resolve-conflicts=auto` (pick best solution)
   - `--resolve-conflicts=interactive` (show menu)
   - `--prefer=minimal-changes|latest|conservative`

### Data Sources

- **Version metadata**: PyPI JSON API (already used by uv)
- **Dependency graph**: uv's resolver output
- **Package health**: PyPI release dates (optional enhancement)
- **No external services required** - Everything from existing data

### Proposed Code Structure

```
crates/
  uv-resolver/
    src/
      conflict_analyzer.rs   # Analyze conflicts from resolver output
      solution_finder.rs     # Generate alternative solutions
      solution_ranker.rs     # Score and rank solutions

  uv-cli/
    src/
      commands/
        resolve_conflicts.rs # Interactive conflict resolution UI
```

## Benefits

### For Developers
- **Save time**: 15min-2hr → 30 seconds to resolve conflicts
- **Learn faster**: Understand why conflicts happen
- **Make better decisions**: See tradeoffs clearly
- **Confidence**: Know the impact before applying changes

### For Teams
- **Standardize resolution strategy**: Use `--prefer=conservative` in CI
- **Reduce bike-shedding**: Clear data-driven recommendations
- **Faster upgrades**: Less friction upgrading dependencies
- **Better documentation**: Solution explanations serve as upgrade notes

### For the Python Ecosystem
- **Encourage upgrades**: Remove friction from staying up-to-date
- **Reduce version fragmentation**: Easier to move ecosystem forward
- **Better error messages**: Educational, not just errors
- **Unique to Python**: No other Python tool offers this

## Differentiation

| Feature | pip | poetry | pdm | conda | uv + this feature |
|---------|-----|--------|-----|-------|-------------------|
| Detect conflicts | ✅ | ✅ | ✅ | ✅ | ✅ |
| Explain conflicts | ❌ | ⚠️ (basic) | ⚠️ (basic) | ❌ | ✅ |
| Suggest solutions | ❌ | ❌ | ❌ | ❌ | ✅ |
| Interactive resolution | ❌ | ❌ | ❌ | ❌ | ✅ |
| Solution comparison | ❌ | ❌ | ❌ | ❌ | ✅ |
| Speed | Slow | Slow | Medium | Slow | **Instant (<100ms)** |

**Key advantage**: uv's speed makes exploring multiple solutions practical - other tools are too slow to do this interactively.

## Prior Art

### Similar Features in Other Ecosystems

1. **cargo** (Rust) - Shows helpful conflict messages but no interactive resolution
2. **npm/yarn** - Suggests `--force` or `--legacy-peer-deps` (not helpful)
3. **bundler** (Ruby) - Good error messages but no solution suggestions
4. **apt** (Linux) - Suggests conflicting packages but manual resolution

**None offer interactive solution exploration with ranked alternatives.**

### Academic Background

- **PubGrub algorithm** already identifies conflicts precisely
- Version SAT solvers can find satisfying assignments
- uv's speed makes exhaustive search of solution space practical

## Implementation Effort

**Estimated scope**: Medium feature (smaller than full audit system)

- **Conflict analysis**: ~3-5 days (parse PubGrub output)
- **Solution finder**: ~1 week (test alternative constraints)
- **Ranking algorithm**: ~3-5 days (scoring system)
- **Interactive UI**: ~1 week (terminal UI with prompts)
- **CLI integration**: ~3-5 days (flags, options, config)
- **Testing**: ~1 week (conflict scenarios, edge cases)
- **Documentation**: ~3-5 days (guides, examples)

**Total**: ~3-4 weeks for MVP

### Phases

**Phase 1 (MVP)**:
- Detect conflicts
- Suggest 1-3 simple solutions (downgrade X, upgrade Y, remove Z)
- Interactive prompt to apply

**Phase 2**:
- Full solution ranking
- Impact analysis
- Multiple resolution strategies

**Phase 3**:
- Explain mode
- Learning features
- Advanced conflict visualization

## Open Questions

1. **Default behavior**: Auto-enable on conflict or require flag?
2. **Solution limit**: Show top 3, 5, or all valid solutions?
3. **Ranking weights**: How to balance recency vs stability?
4. **CI mode**: Default strategy for non-interactive environments?
5. **Conflict visualization**: Text-based tree or optional graph output?
6. **Solution caching**: Cache explored solutions across runs?

## Alternative Approaches

If interactive resolution is too complex initially, simpler alternatives:

### Option A: Just Explain Conflicts Better
- Parse PubGrub output into human-readable explanations
- Show dependency chain that led to conflict
- No solution suggestions

### Option B: Single Best Solution
- Skip interactive menu
- Just suggest the single best fix
- Add `--apply` flag to accept automatically

### Option C: Config-Based Strategies
- Define resolution strategy in `uv.toml`
- Auto-apply based on strategy (no interaction needed)

## Real-World Use Cases

### Use Case 1: Upgrading Django
```
Current: Django 3.2 (LTS ending)
Want: Django 4.2 (current LTS)
Conflict: 12 packages incompatible

Without this feature: 2-4 hours of manual work
With this feature: 2 minutes (review + apply solution)
```

### Use Case 2: Adding ML Library
```
uv add torch
Conflict: numpy version requirements clash

Solution suggested: Update scipy 1.9 → 1.11 (compatible numpy range)
Time saved: 30 minutes of investigation
```

### Use Case 3: Monorepo Workspace
```
Workspace package A updated, breaks package B
Interactive mode shows: which workspace members affected
Suggests: version bounds to maintain compatibility
```

## Success Metrics

If implemented, success would look like:

- **Time to resolution**: <5 minutes for 80% of conflicts (vs 30+ min currently)
- **User satisfaction**: Reduced complaints about dependency hell
- **Adoption**: Feature used in majority of conflict scenarios
- **Education**: Users learn about their dependency trees

## Why This Feature Matters

Dependency conflicts are a **fundamental problem** in package management, not a nice-to-have feature. They:

- Block security updates
- Prevent framework upgrades
- Waste developer time
- Create technical debt
- Discourage keeping dependencies current

This feature would position uv as not just **faster**, but **smarter** - helping developers make informed decisions instead of leaving them stuck.

## Next Steps

I'm eager to implement this feature and would take full ownership of:

1. Creating detailed technical design document
2. Implementing the conflict analyzer and solution finder
3. Building the interactive UI
4. Writing comprehensive tests for various conflict scenarios
5. Creating documentation and examples
6. Iterating based on code review feedback

This feature would make uv the first Python package manager to **actively help solve conflicts** rather than just reporting them.

---

_Comment by @cherchyk on 2025-11-09 02:52_

To clarify my intent: I want to implement this feature myself and contribute it to uv.

I'm offering to:
- Take full ownership of the implementation
- Create the detailed technical design
- Implement the conflict analyzer, solution finder, and interactive UI
- Write comprehensive tests covering various conflict scenarios
- Provide documentation and usage examples
- Iterate based on code review and community feedback

I believe this feature addresses a fundamental pain point in Python dependency management, and I'm committed to delivering a high-quality implementation. I'm ready to start work once we align on the approach and scope.

Looking forward to your feedback on whether this aligns with uv's roadmap!

---

_Comment by @zanieb on 2025-11-09 02:59_

Can you please disclose how much, if any, of this issue was written using an LLM?

---

_Comment by @cherchyk on 2025-11-09 23:46_

Is there a "humans only" policy I missed in the contributing guidelines? 😄

The feature idea is mine, the commitment to implement is mine, and the PR will be mine. Happy to let the code speak for itself when we get there.

---

_Comment by @zanieb on 2025-11-10 14:38_

There's not a humans only policy, but the issue description is extremely verbose and hard to engage with.

You're welcome to try to write some code for this, I'd start with a proof-of-concept deriving a possible fix from an error tree. It seems _very_ hard to do well though, and I think it's quite unlikely we'll accept a contribution here. If the pull request is clearly authored by an LLM, we will not review it.

---

_Label `enhancement` added by @zanieb on 2025-11-10 14:38_

---
