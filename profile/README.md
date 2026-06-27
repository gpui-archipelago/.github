# gpui-archipelago

> Every fork is an island. This is where islands find each other: no mainland, no canonical shore, just builders and the water between them.

Nobody wanted to maintain a fork. We wanted Zed to maintain GPUI: versioned, stable, published. That didn't happen.

So forks emerged. Each solving a piece of the problem differently:

- **gpui-unofficial** — published up-to-date crates on crates.io, solving the distribution problem. Needs a maintainer.
- **gpui-ce** — a semi-official community fork, trying to solve the governance problem. Searching its own direction.
- **Kael** — abandoned upstream sync by design, built for desktop apps (music player, video editor). Solves the stability problem by stepping off the upstream entirely. Makes it incompatible with other forks.
- **Wgpui** — a capability fork built around a game engine, replacing the rendering backend with wgpu across all platforms and adding an escape hatch for 3D.

Meanwhile people keep appearing in GitHub issues, Discord servers and Reddit threads, sharing the same frustrations, advertising their forks, looking for a home.

This is that home.

## What this is

A community-maintained space for people building with GPUI outside of Zed — whether that means running your own fork, depending on someone else's, or just trying to ship something without it breaking next week.

The goal is not to pick a winner, but to understand the landscape: what each approach solves, what it doesn't, and where common ground might exist across forks.

## Two kinds of problems

### Ecosystem problems
Problems with how GPUI exists in the world: distribution, stability, versioning, documentation, getting started. Often solvable without touching GPUI source code.

### Capability problems
Problems with what GPUI can actually do: missing elements, missing escape hatches, missing extension points. These require code changes, and different forks may solve them incompatibly. This is where a shared spec matters most.

## How issues are structured

Each issue lives in `issues/`. The format:

```
## The problem
Written from the perspective of someone who just hit this wall.
What did they try to do, and what went wrong?

## Why it exists upstream
Honest, neutral explanation of Zed's priorities or constraints
that produced this gap. Not blame — just context.

## How people currently solve it
Known approaches — forks, crates, workarounds, conventions.
Each approach: what it solves, what it doesn't, who maintains it.

## What nobody has solved yet
The honest remainder.

## Common ground opportunity
Optional. A convention, interface, or spec that multiple forks
could adopt to stay compatible.
```

## How to contribute

- **Add an issue** — open a PR with a new file in `issues/`. Partial drafts and LLM generated welcome, just be reasonable and concise.
- **Add an approach** — know how a fork addresses an existing issue? Add it.
- **Correct something** — wrong, outdated, or unfair? Open an issue or PR.
- **Add your fork** — see `FORKS.md`. Any fork is welcome regardless of maturity.
- **Share your experience** — started a fork, abandoned one, or learned something the hard way? That knowledge belongs here.

## Honest disclaimer

This repo was started by someone who tried to avoid forking, failed to use upstream as is, and learned things in the process. There is bias here. The structure is designed to eliminate it — contributions from other islands are how this stays unopinionated.

## Status

Early. The structure exists. The issues don't yet. That's the contribution opportunity.

---

*Forks are not fragmentation. They're how a distributed community expresses disagreement productively.*
