# Issue: Versioning conventions across the GPUI fork ecosystem

**Status**: Discussion

**Type**: Ecosystem

**Raised by**: @Vanuan

## The problem

Every project in the GPUI fork ecosystem has come up with its own answer to the same question: how do you version a crate whose upstream is a fast-moving, not-always-tagged Git repository, when crates.io won't accept `git =` dependencies? None of the current answers are wrong, but they're all different, and someone arriving from one fork has no way to predict how another fork's version numbers work. Anyone trying to depend on two of these projects at once (a downstream crate wanting to support both `gpui-ce` and a vanilla mirror, say) has to learn each one from scratch.

## Why it exists upstream

Zed ships `gpui` as part of building their editor, not as a standalone product with its own release discipline. There's no reason for them to tag frequently or guarantee that crates.io publishes track every revision people care about — that's a reasonable choice on their end. It does mean every downstream consumer ends up deciding for themselves how to express "what Zed actually has right now" in a format Cargo understands, and they're all doing it differently.

## Context for newcomers: why things look stale, and who started when

This trips people up enough that it's worth spelling out plainly. If you search crates.io for `gpui`, you'll find Zed's own crate sitting at `0.2.2` and looking abandoned. It isn't abandoned — Zed simply doesn't publish to crates.io as part of their normal workflow. The actual GPUI source moves constantly in Zed's main repo; the crates.io listing just doesn't reflect that. This one fact is the root cause of basically every fork and every versioning scheme covered in this issue: each project exists because someone wanted a way to consume current GPUI without depending on Zed's Git repo directly.

From there, the forks branched off from each other, not all from Zed directly, which is part of why their version numbers don't line up:

- **`gpui-ce`** forked from Zed's `gpui` to give the wider community a semver-friendly, crates.io-publishable line that still tries to track Zed's lineage.
- **WGPUI** forked from `gpui-ce` rather than from Zed directly, inheriting `gpui-ce`'s versioning quirks (and its hybrid Git/semver split) on top of its own.
- **Kael** appears to have forked independently and deliberately chose not to chase Zed's releases at all.
- **`gpui-unofficial`** takes a different angle entirely: rather than patching GPUI, it republishes Zed's own tags verbatim to crates.io, plus proposed nightly/errata conventions for the gaps between tags.

We don't have a reliable, agreed-upon timeline of exact fork dates yet — if anyone has the actual history (first commit, point of divergence, etc.) for `gpui-ce`, WGPUI, or Kael, that would be worth pinning down here so newcomers have something concrete to read rather than tribal knowledge scattered across Discord threads.

## Approaches in the wild

| Approach | Who | Solves | Doesn't solve | Tradeoff |
|---|---|---|---|---|
| Publish once, then stop | Zed (`gpui` `0.2.2`) | Established crates.io presence | No ongoing tracking; consumers needing recent revisions fall back to Git deps | Crate listing goes stale relative to actual development |
| Fork a fork, pin to Git master | WGPUI (forks `gpui-ce`) | Always current with the fork's tip | Not publishable to crates.io downstream; consumers inherit a Git dependency transitively | Fast-moving but fragile, breaks as soon as a downstream wants a crates.io-only tree |
| Hybrid: semver for one crate, Git revision for a sibling | `gpui-ce` (`0.x` for the GPUI crate, Git revision for `gpui-platform`) | Lets the more stable parts of the API look normal to Cargo | Inconsistent guarantees across the workspace — one crate is pinned, its sibling isn't | Reflects real differences in volatility, but confusing from outside |
| Independent `0.x` series, no upstream sync | Kael | Full control over release cadence and what counts as a breaking change | Version numbers carry no information about which Zed revision underlies a release | Cleanest in isolation, hardest to reason about across the ecosystem |
| Verbatim upstream tag as version | `gpui-unofficial` | Obvious, auditable mapping to the upstream tag | Nothing for unreleased revisions or a botched-publish fix | Simple but inflexible — see below |
| `[next-patch]-nightly.{yyyymmdd}.{sha7}` | Proposed for `gpui-unofficial` | Auditable snapshot of an untagged revision; never auto-selected by `cargo update`; sorts correctly | Version list grows unless old nightlies are yanked on each new publish | Needs CI discipline to stay clean |
| `[same-version]-fix.N` | Proposed for `gpui-unofficial` errata | Corrects a botched publish without yanking, so unaffected consumers aren't broken | One-off only, not a tracking mechanism | Quietly superseded by the next real release |
| Pre-release suffix per fork target (`1.8.2-ce.1`, `1.8.2-wgpui.1`) | Proposed for downstream crates like `gpui-component-unofficial` | One concrete dependency per published version, no feature-flag matrix; each fork shows up as its own row on crates.io | Needs its own release cadence per fork; stretches what semver pre-release usually means | Explicit, deliberate pinning rather than implicit feature unification |

## Discussion

Open thread — link to Discord forum post or GitHub discussion (TBD).

A few things worth raising early, given how different everyone's starting point is:

- Zed publishing once and then relying on Git afterward means any shared convention has to work without their cooperation — we can't assume they'll start tagging nightlies or anything like that.
- `gpui-ce`'s hybrid setup (semver for one crate, Git revision for its sibling `gpui-platform`) suggests one convention might not fit every crate in a workspace. Should this be a per-crate decision rather than a workspace-wide rule?
- Kael choosing not to sync with Zed is a deliberate design choice, not something to fix. Whatever convention comes out of this needs to leave room for "I'm intentionally diverging," not just "I'm tracking upstream with a delay."
- WGPUI forking `gpui-ce`, which is itself a fork, raises the question of whether a versioning convention needs to show lineage, not just identity — something like "this targets gpui-ce, which itself tracks Zed revision X."
- Separately from versioning itself: should the newcomer-facing explanation above live in this issue, or graduate to a FAQ/README in gpui-archipelago once the history is actually confirmed?

### A related, possibly bigger idea: shared distribution tooling

Versioning conventions only solve part of the problem — even with a perfect naming scheme, every fork still has to build, test, and actually push releases to crates.io on its own, by hand, on its own schedule. That's a separate but related gap, and it might be worth raising here rather than as a new issue, since whatever versioning convention we land on would need to be encoded somewhere in CI anyway.

The idea: a shared, opt-in distribution scripts repo — something like `gpui-archipelago-release-tools` — that any fork could plug into to get a predictable, automated release cadence (e.g. "check upstream for new tags/revisions weekly, build, run a basic smoke test, publish with the agreed suffix convention, yank what needs yanking"). Forks would still control their own source and their own decision about *when* to cut a release; this would just standardize the *mechanics* of cutting one, instead of every fork hand-rolling its own GitHub Actions workflow for the same problem.

This wouldn't require anyone to give up independence — `gpui-ce` could still decide its own cadence, Kael could still skip Zed syncs entirely — it would just mean the actual publish step (and the nightly/fix/fork-suffix conventions from this issue) lives in one well-tested place instead of being reimplemented per fork.

Open question: is this in scope for this issue, or should it be split into its own issue once/if there's appetite for it? Flagging here for now since it overlaps so directly with the suffix conventions above.

## Possible common ground

The nightly/fix/fork-suffix ideas from the `gpui-unofficial` proposal all rest on the same idea: a pre-release identifier as an explicit, opt-in signal rather than strictly "less stable than the next number." That might generalize across the ecosystem no matter how each project tracks (or doesn't track) upstream. Kael's independent series could still adopt a `-{fork}.N` suffix someday if it wants to express compatibility with a specific Zed revision, without giving up its no-sync philosophy. WGPUI's Git-pinned approach doesn't rule out also publishing semver snapshots for consumers who want crates.io only. None of this requires anyone to change what they're doing now — it's a shared vocabulary on top, not a mandate.

If there's appetite for the shared distribution tooling idea too, that's a natural next step once (or if) the suffix conventions converge — the conventions are what the tooling would actually encode.

## Resolution

Open — no consensus yet.
