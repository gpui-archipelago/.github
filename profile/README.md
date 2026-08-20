# gpui-archipelago
### (aka GPUI-lago — "jee pyu ay la go")

> Every fork is an island. There's no mainland and no one's building one, so the islands have to find each other. Here they can.

Ideally, Zed would have published GPUI as a stable, versioned crate, but that didn't happen. Various people forked it instead, and each fork ended up solving a different part of the problem:

- **gpui-unofficial** — publishes up-to-date crates on crates.io, which covers distribution. Currently needs a maintainer.
- **gpui-ce** — a semi-official community fork aimed at governance, still working out where it's headed.
- **Kael** — forked from GPUI direct; selectively tracks upstream while building out general-purpose features (gradients, webviews, form controls, component library).
- **WGPUI** — a fork of gpui-ce that swaps the rendering/windowing backend for wgpu + winit. Unified code path across Windows, macOS, Linux, and WebAssembly. `WgpuSurface` lets you embed raw wgpu render passes (3D, game engines, custom shaders, anything) directly in your UI tree, makes WGPUI suitable as a shell for 3D applications and mixed-mode interfaces.

Meanwhile people keep turning up across GitHub issues, Discord, and Reddit threads with similar frustrations across these multiple projects.

## What this is

`gpui-archipelago` is a community-maintained space for anyone building with GPUI outside of Zed. Maybe you run your own fork, maybe you depend on someone else's, maybe you want to ship something on top of GPUI and minimize your chances of encountering unnecessary instability and breakage.

The goal is not to pick a winner, but to understand the landscape: what each approach solves, what it doesn't, and where common ground might exist across forks.

## Forks at a glance

| Fork | Stars | Forks | Crate |
|---|---|---|---|
| **[Zed (upstream GPUI)](https://github.com/zed-industries/zed)** | 88,000 | 10,000 | ⌛ |
| **[gpui-ce](https://github.com/gpui-ce/gpui-ce)** | 925 | 105 | ⌛ |
| **[gpui-unofficial](https://github.com/iamnbutler/gpui-unofficial)** | 44 | 7 | ✓ |
| **[WGPUI](https://github.com/Far-Beyond-Pulsar/WGPUI)** | 29 | 6 | ✗ |
| **[Kael](https://github.com/Augani/kael)** | 17 | 0 | ✓ |

## Two kinds of problems

### Ecosystem problems
Problems with how GPUI presents itself as a software project: distribution, stability, versioning, docs, guides for getting started at all. These are usually fixable without touching GPUI source code.

### Capability problems
Problems with what GPUI can do: missing elements, missing escape hatches, missing points where the API can be extended. These need code changes, and different forks may make those changes in incompatible ways. This is where a shared spec is useful.

## How issues are structured

Each issue lives in `issues/`. The format:

```
## The problem
Written from the perspective of someone who just hit this wall.
What did they try to do, and what went wrong?

## Why it exists upstream
A plain explanation of the Zed priorities or constraints behind
the gap. Give context, don't assign blame.

## How people currently solve it
Known approaches: forks, crates, workarounds, conventions.
For each one: what it solves, what it doesn't, who maintains it.

## What nobody has solved yet
The part that's still genuinely open.

## Common ground opportunity
Optional. A convention, interface, or spec that several forks
could adopt to stay compatible with each other.
```

## How to contribute

- **Add an issue** — open a PR with a new file in `issues/`. Partial drafts and LLM-generated text are fine, just keep it reasonable and concise.
- **Add an approach** — know how a fork handles an existing issue? Add it.
- **Correct something** — wrong, stale, or unfair? Open an issue or PR.
- **Add your fork** — see the Forks table above. Any fork is welcome, however immature.
- **Share your experience** — started a fork, abandoned one, got burned and learned from it? Write it down here.

## Where this comes from

I started this repo after trying not to fork, failing to use upstream as-is, and picking up a few things along the way. So yes, there's a bias. Contributions from the other islands are what would keep this from being one person's opinion.

## Status

Still extremely alpha.
