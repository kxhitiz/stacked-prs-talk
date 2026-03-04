---
theme: default
title: The stacking workflow
info: Ship Faster, Review Better
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---

# The stacking workflow

## Ship Faster, Review Better

---

<div class="flex justify-center">
  <img src="/images/code-review-tweet.png" class="h-60 rounded shadow" />
</div>

---

# The Problem

<v-clicks>

- Large PRs sit in review for **days** or even [years](https://github.com/matplotlib/matplotlib/pull/9598#issuecomment-2037292898)
- Reviewers skim instead of reading
- Merge conflicts compound over time
- "LGTM" becomes a rubber stamp

</v-clicks>

---

# The Ideal PR Size

<v-clicks>

- **~50 lines** — the sweet spot, reviewed & merged ~40% faster than 250-line PRs
- **< 200 lines** — practical target most teams aim for
- **> 400 lines** — widely considered too large
- **< 25 lines** — higher revert rate (too small can lack context)

</v-clicks>

---

# What Are Stacked PRs?

Break one large feature into small, dependent PRs

<v-clicks>
```
main
 └── PR 1: Add experiment config       (12 lines)
      └── PR 2: Add component           (180 lines)
           └── PR 3: Add tracking        (45 lines)
```

Each PR has a **single purpose** and is **small enough to review in short time**.
</v-clicks>

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

<div class="command-list">
<v-clicks>

```bash
# Branch off main
git town hack BIG-1023-setup_split_experiment
```

```bash
# Push + open PR with correct base
git town propose
```

```bash
# Stack a child branch
git town append BIG-1023-add_tracking_events
```

```bash
# Sync entire stack
git town sync --stack
```

```bash
# Navigate the stack
git town up / git town down / git town switch
```

<span class="!hidden"></span>

</v-clicks>
</div>

<style>
.command-list .slidev-vclick-target {
  transition: opacity 0.3s ease;
}
.command-list .slidev-vclick-hidden {
  opacity: 0.2 !important;
}
.command-list:not(:has(.slidev-vclick-hidden)) .slidev-vclick-target {
  opacity: 1 !important;
}
.command-list .slidev-vclick-prior {
  opacity: 0.2 !important;
}
.command-list .slidev-vclick-current {
  opacity: 1 !important;
}
</style>

---

# The Missing Piece

### Stack Visualization in PRs

<v-clicks>

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

</v-clicks>

---

# Stack Visualization in Action

<div class="flex gap-4 justify-center items-center px-8">
  <img src="/images/stack-viz-1.png" class="max-w-[45%] rounded shadow" />
  <img src="/images/stack-viz-2.png" class="max-w-[45%] rounded shadow" />
</div>

--- 

# Questions?

Start small — try stacking your next feature into 2 PRs.
