---
theme: default
title: Stacked Diffs/PRs
info: Ship Faster, Review Better
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---

# Stacked Diffs/PRs

## Ship Faster, Review Better

---

# Ship Faster, Review Better

<div class="flex justify-center">
  <img src="/images/code-review-tweet.png" class="h-60 rounded shadow" />
</div>

---

# The Problem

<v-clicks>

- 1000+ line PRs sit in review for **days**
- Reviewers skim instead of reading
- Merge conflicts compound over time
- "LGTM" becomes a rubber stamp

</v-clicks>

---

# What Are Stacked PRs?

Break one large feature into small, dependent PRs

```
main
 └── PR 1: Add experiment config       (12 lines)
      └── PR 2: Add component           (180 lines)
           └── PR 3: Add tracking        (45 lines)
```

Each PR has a **single purpose** and is **small enough to review in short time**.

---

# Why Stacked PRs?

<v-clicks>

- **Authors** — ship incrementally, get early feedback
- **Reviewers** — focused context, faster review cycles
- **Teams** — unblocks parallel work, higher velocity

</v-clicks>

---

# Tools I Tried

| | Graphite | Machete | Git Town |
|---|---|---|---|
| Type | SaaS + CLI | CLI only | CLI only |
| Cost | Free / Paid | Free (OSS) | Free (OSS) |
| Vendor lock-in | Yes | No | No |
| Team adoption needed | Yes | No | No |

---

# Recommendation: Git Town

<v-clicks>

- Pure Git — GitHub, GitLab, Bitbucket
- No account, no dashboard, no lock-in
- `sync --stack` rebases the entire chain
- `propose` sets PR base branch automatically

</v-clicks>

---

# Git Town Commands

```bash
# Branch off main
git town hack feature-a

# Stack a child branch
git town append feature-b

# Sync entire stack
git town sync --stack

# Push + open PR with correct base
git town propose

# Navigate the stack
git town up / git town down / git town switch
```

---

# The Missing Piece

### Stack Visualization in PRs

GitHub Action: [**git-town/action**](https://github.com/git-town/action)

```yaml
# .github/workflows/git-town.yml
on:
  pull_request:
    types: [opened, synchronize, edited]

jobs:
  git-town:
    runs-on: ubuntu-latest
    steps:
      - uses: git-town/action@v1
        with:
          skip-single-stacks: true
```

Reads PR base branches — no local config needed.

---

# Stack Visualization in Action

<div class="flex gap-4 justify-center items-center px-8">
  <img src="/images/stack-viz-1.png" class="max-w-[45%] rounded shadow" />
  <img src="/images/stack-viz-2.png" class="max-w-[45%] rounded shadow" />
</div>

---

# My Workflow

```bash
# PR 1: branch off main
git town hack setup-split
git town propose

# PR 2: stack on top
git town append secondary-nav
git town propose

# PR 3: stack another
git town append tracking-events
git town propose

# Rebase all after changes
git town sync --stack
```

---
layout: center
class: text-center
---

# Questions?

Start small — try stacking your next feature into 2 PRs.
