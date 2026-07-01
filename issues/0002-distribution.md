# Issue: Distribution and resilience across the GPUI fork ecosystem

**Status**: Discussion

**Type**: Ecosystem

**Raised by**: @Vanuan

## The problem

Building on a GPUI fork means betting on that fork's survival. Forks are maintained by individuals with limited time, no formal succession plans, and no obligation to continue. If the fork you depend on goes quiet — its maintainer burns out, moves on, or loses interest — you're left either maintaining it yourself or migrating to another fork and absorbing the breaking changes.

This isn't hypothetical. `gpui-unofficial` already needs a maintainer. Any fork in this ecosystem could find itself in the same position.

The deeper problem is that contributions are coupled to forks. If you fix a bug or add a feature in fork A, that work doesn't automatically exist anywhere else. Every other fork has to rediscover and reimplement it. Knowledge and effort that should belong to the ecosystem gets trapped on a single island.

## Why it exists upstream

Zed maintains GPUI as an internal tool for building their editor, not as a standalone library with downstream consumers in mind. There is no stable release cadence, no crates.io publishing workflow, no API stability guarantee, and no succession plan for the broader ecosystem. Downstream consumers are on their own.

This means every fork starts by solving the same bootstrapping problem — how do I depend on something that doesn't publish releases? — and then immediately faces the sustainability problem: how do I keep depending on it as upstream moves?

## Approaches in the wild

| Approach | Who | Solves | Doesn't solve | Tradeoff |
|---|---|---|---|---|
| Hard fork, own release cadence | Kael, wgpui | Full independence, own versioning, own stability guarantees | Upstream improvements don't flow in automatically; maintaining divergence is ongoing work | Clean but isolating — your fork is an island with no bridges |
| Mirror and republish upstream tags | gpui-unofficial | Makes upstream consumable via crates.io without changes | Fork survival still depends on one maintainer; no mechanism for community patches | Simple and auditable, but fragile if maintainer disappears |
| Community fork tracking upstream | gpui-ce | Shared maintenance burden, more eyes on upstream changes | Still a single codebase; contributor effort is tied to this fork's survival | Better than one maintainer, but still a single point of failure |
| Git dependency, no published crate | wgpui, others | Always current, no publish overhead | Not crates.io compatible; transitive git deps break downstream publishing; fork health = your health | Fast but fragile |
| Patch set on top of upstream | Proposed | Contributions are portable — patches can be applied to any fork; not tied to any single fork's survival | Requires coordination infrastructure; API negotiation across forks is harder than a single PR | Resilient but more complex |

## The patch model in more detail

In broader open source, soft forks often work by maintaining a set of patches rebased on top of upstream rather than a full independent codebase. The patches travel independently of any particular fork.

Applied to the GPUI ecosystem this would work roughly as follows:

- A contributor identifies a problem or capability gap
- They implement it as a patch set, not as a commit to a specific fork
- They propose the patch to each relevant fork, negotiating the API collaboratively with each maintainer
- Distribution maintainers — people whose job is packaging, not feature development — apply the agreed patch set to whichever upstream fork they're building on and publish the result

The key property: the contribution exists independently of any single fork. If one fork goes dark, the patch still exists. Another fork can pick it up. The contributor's work isn't lost.

This also changes what "contributing to the GPUI ecosystem" means. Instead of opening a PR to one fork and hoping it merges, you're proposing a portable change that the ecosystem as a whole can adopt at its own pace.

## What nobody has solved yet

- No shared infrastructure for proposing and tracking patches across forks
- No convention for what a "portable GPUI patch" looks like technically
- No defined role for "distribution maintainer" separate from "fork maintainer"
- No succession mechanism — if a fork maintainer disappears, there's no clear path for the community to take over
- API negotiation across forks has no precedent in this ecosystem yet

## Possible common ground

The patch model separates three roles that are currently collapsed into one person per fork:

1. **Feature development** — proposing and implementing changes
2. **Fork maintenance** — tracking upstream, resolving conflicts
3. **Distribution** — packaging and publishing to crates.io

Separating these means no single person has to do all three. A contributor can propose a patch without maintaining a fork. A distributor can package releases without developing features. A fork maintainer can track upstream without also being responsible for downstream consumers.

Whether this needs formal tooling or just a shared convention is an open question — see `proposals/` for emerging ideas.

## Discussion

Open — link to Discord forum post TBD.

A few things worth raising:

- Nate Butler has suggested a hard fork is the only realistic model for a community project that wants to innovate independently of Zed. The patch model is a counterproposal — it accepts that forks will diverge but tries to keep contributions portable anyway. Both views deserve honest examination.
- The patch model assumes forks are willing to negotiate APIs collaboratively. That requires trust and goodwill across fork maintainers. Whether that exists in this ecosystem is an open question.
- Distribution maintainers as a separate role only makes sense if there are enough contributors to fill multiple roles. For a small early ecosystem this might be premature.

## Resolution

Open — no consensus yet.
